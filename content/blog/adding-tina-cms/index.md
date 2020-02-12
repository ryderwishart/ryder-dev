---
title: Adding TinaCMS to my Gatsby Starter Blog
date: "2020-02-01"
description: "How I added TinaCMS to my Gatsby starter blog for faster creation of blog posts."
---

## TLDR

I did manage to add TinaCMS to my Gatsby site, and I couldn't be happier! But it took me longer than I thought, because I'm a rookie.

All I can say is **when in doubt, reverse engineer something that works!**

## How to add TinaCMS to your Gatsby site

> Make your site editable in five minutes.

That's the claim on the [TinaCMS homepage](https://tinacms.org/).

![Tina-5-min](/tina-5-min.png)

Well, I put it to the test, and, as it turns out it took me way longer, but next time it probably *will* take 5 minutes.

These are their four steps, but they only come with three bits of code:

> Install and configure Tina plugins
> Wrap and export your templates with Tina components specific to your data
> Customize and define the content fields by passing an options argument
> Open the sidebar 🤩, edit your site and watch the content updating in realtime

And here's the complete code they stick beside those instructions:

```javascript
yarn add gatsby-plugin-tinacms

module.exports = {
  // ...
  plugins: [
    'gatsby-plugin-tinacms',
    // ...
  ],
};

export WithTina( Component );
```

## Step 1: Install the TinaCMS plugin for Gatsby using your package manager

If you are using a Gatsby starter, then you're either using NPM or Yarn to manage your packages. Just take a look in your site's root directory and look for a `package-lock.json` file. If you see that, then you're using NPM, otherwise, you can use Yarn!

So I'm following the instructions on tinacms.org, and step one is:

`yarn add gatsby-plugin-tinacms`

(I'm using NPM, not Yarn, so I'll use `npm add gatsby-plugin-tinacms`, which is an alias for `npm install gatsby-plugin-tinacms`).

## Step 2: Import the TinaCMS plugin into your gatsby-config.js file

The next code block looks like this:

```javascript
module.exports = {
  // ...
  plugins: [
    'gatsby-plugin-tinacms',
    // ...
  ],
};
```

But, really, what does that mean anyways?

I took a wild stab based on a vague memory I have about adding a plugin last week, and opened up my `gatsby-config.js` file. Wouldn't you know it, but after some imports, that file begins with:

```javascript
module.exports = {
  pathPrefix: config.pathPrefix,
  siteMetadata: {
    title: config.siteTitle,
  },
  plugins: [
    'gatsby-plugin-react-helmet',
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
          ...
```

That looks like the file I want!

So, at the top of my plugins array, I added the TinaCMS plugin (which I already installed using NPM).

Now it looks like this:

```javascript
  plugins: [
    'gatsby-plugin-tinacms',
    'gatsby-plugin-react-helmet',
    {
        ...
```

I think so far I've completed the first prose instruction:

> Install and configure Tina plugins

So, on to the second prose instruction:

> Wrap and export your templates with Tina components specific to your data

## Step 3: Wrap your components in a TinaCMS function

Alright, so the TinaCMS site just has this third bit of code:

`export WithTina( Component );`

Hm.

Well, I'll go look for a template that is also a React component.

This sounds like `/src`-folder territory.

In my Gatsby starter blog folder structure, I've got both a `/templates` folder and a `/components` folder. Maybe I can use both. I'm guessing if I wrap a component in the `WithTina()` function, it will allow me to edit the component, and if I wrap a template, it will let me edit the template.

In both cases, I'm guessing I'll need to input some parameters, you know, to tell the `WithTina()` function what exactly it's supposed to be editing, but I'll cross that bridge when I come to it.

Since I've added new plugins since I've started, I'm going to shut down my `gatsby develop` process and restart it.

There is only one template in my `/templates` folder, `blog-post.js`, so I opened that up.

Near the bottom, I see this line of code:

```javascript
export default BlogPostTemplate
```

So I swapped it for:

```javascript
//export default BlogPostTemplate

export default WithTina( BlogPostTemplate );
```

And then I fired up `gatsby develop` in my terminal.

Nothing. 😓

Oh yeah!—It would probably help if I saved my `blog-post.js` file! 😅

But then I got an error

```bash
 ERROR #98123  WEBPACK

Generating development JavaScript bundle failed


/Users/ryderwishart/Documents/Projects/Work/Marketing/Websites/gatsby-sites/gatsby-starter-blog/src/templates/blog-post.js
  85:16  error  'WithTina' is not defined  no-undef

✖ 1 problem (1 error, 0 warnings)


File: src/templates/blog-post.js

failed Re-building development bundle - 0.799s
```

Of course, how could `blog-post.js` know what `WithTina()` means unless I import it first?

At the top of `blog-post.js` I added this line:

```javascript
import WithTina from "gatsby-plugin-tinacms"
```

Well, I'm still getting errors! I tried destructuring the import, but that didn't help.

It's time to actually read the in-depth instructions. This is taking more than 5 minutes, but I bear all the responsibility for that.

## Starting over

For adding TinaCMS to a site, our [Forestry.io](https://forestry.io) friends (the creators of TinaCMS) offer this [guide](https://tinacms.org/docs/gatsby/manual-setup/).

### Step 1 of manual setup

The first step is `npm install --save gatsby-plugin-tinacms styled-components`.

I already installed `gatsby-plugin-tinacms`, but maybe I didn't `--save` it? Hm.

### Step 2 of manual setup

Then we need to modify our `gatsby-config.js` file, but I already did that too. I noticed there is a comment in the middle of this block that makes me think I should match my call for `gatsby-plugin-tinacms` to this one:

```javascript
module.exports = {
  // ...
  plugins: [
    {
      resolve: 'gatsby-plugin-tinacms',
      options: {
        plugins: [
          // We'll add some Tinacms plugins in the next step.
        ],
      },
    },
    // ...
  ],
}
```

In fact, I'm starting to think I may have just imported the plugin incorrectly.

I restarted my local version using ctrl+c and `gatsby develop`.

Well, it still doesn't seem to be working! 

I am getting a type error: `Unhandled Rejection (TypeError): e is undefined`

That's it, time to start over again, and head to a working TinaCMS demo.

## Step 1 of reverse engineering a working demo

Looking at this working [demo's config file](https://github.com/tinacms/gatsby-starter-tinacms/blob/master/gatsby-config.js), I am installing these plugins:

`npm install --save gatsby-tinacms-remark gatsby-tinacms-git gatsby-tinacms-json`

I'm also going to add the plugins to my config file using this code:

```javascript
  plugins: [
    {
      resolve: "gatsby-plugin-tinacms",
      options: {
        plugins: ["gatsby-tinacms-git", "gatsby-tinacms-remark", "gatsby-tinacms-json",],
        sidebar: {
          hidden: process.env.NODE_ENV === "production",
          position: "displace"
        },
      },
    },
```

Wow, WOW.

It worked.

I can see the pencil icon.

![pencil-icon-tina](/pencil-icon.png)

Perhaps it was the double-quotation marks `"`? Perhaps it was the `sidebar` propery? Perhaps it was the lack of line breaks between subsequent plugins?? 

I just don't know. But if you were in my shoes, I hope this helped you.

All I know is that I'm committing this right now before I break something. 😎

## Conclusion

When in doubt, reverse engineer something that works.

That's what I did to add Tina to my blog, and I hope you found this helpful.

If you read this far, it's because you're a learner like me. If it was helpful, please share it!
