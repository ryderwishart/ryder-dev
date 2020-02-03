---
title: Using XML for Linguistics Research
date: "2020-02-03"
description: "Learn the basic ideas behind XPath, xQuery, XSLT, and the Python LXML library."
---

## Searching an XML tree with xPath

My XML tree has a few different types of elements, and this is important to understand before you search the tree:

1. `<seg>` elements are segments of text. When you look at an actual document of human-readable text, these are the kinds of units normally delimited or 'chunked' by syntactic analyses.
1. `<m>` elements are meaningful units. For any given `<seg>`, you can have many units of meaning both above and below it. Parts of segments, or groups of segments.
1. `<temp>` elements are temporary placeholders, until we decide what to do with them. (We are transitioning from an older annotation and don't want to lose information in the process.)

The document also features several top-level elements (`<OpenText>` and `<text>`), and then within these an overarching `<m>` element, which is the text as a unit of meaning.

Here's an example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<OpenText>
   <text>
      <m unit="text">
         <m unit="s">
            <seg type="cl"
                 clType="Verbless"
                 nodeId="410010010010072"
                 unitRule="P2CL">
               <m func="subjprop" funcRule="Np2P" funcId="410010010010071">
                  <m rel="=">
                     <m unit="nm" type="np" nodeId="410010010010070" unitRule="NPofNP">
                        <m rel="=">
                           <temp type="np" head="true" nodeId="410010010010011" unitRule="N2NP">
                              <seg type="w"
                                   pos="noun"
                                   case="nominative"
                                   formal="N-NSF"
                                   functional="N-NSF"
                                   gender="feminine"
                                   gloss="[The] beginning"
                                   head="true"
                                   morphId="41001001001"
                                   n="1"
                                   nodeId="410010010010010"
                                   norm="Ἀρχή"
                                   number="singular"
                                   osisID="Mark.1.1!1"
                                   strong="746"
                                   posClass="common"
                                   text="Ἀρχὴ"
                                   lemma="ἀρχή"
                                   xml:id="_1904.Mark.1.1.1"/>
                           </temp>
                        </m>
```

As you can see, XML is a lot like HTML. Thus, learning how to work with XML documents can be very helpful when it comes to working with HTML or the DOM.

### Searching the XML tree

Imagine I want to find something in this XML tree, maybe every `<seg>`.

I can't just search for `seg`, since an XPath query always starts from the root element of the document. 

Think about navigating your computer file system in the terminal. You can't just move from `Users` to `screenshot.png` if the file isn't located in your `Users` folder.

In the same way, XPath always searches from the root (in this case `<OpenText>`).

If you want to get to any descendent (or, more specifically, all the descendents) that have a `<seg>` tag, then you need to use `//`.

The query `//seg` will find all the segment _descendents_ of the root.

### The key to understanding XPath

I didn't really understand XPath until I got the difference between **paths** and **predicates**. 

A **path** is what I just showed you, some hierarchical path from the root to whatever you are looking for.

A **predicate** is a way of powerfully specifying any path.

When I search for `//seg`, I will get all `<seg>` elements as a result. But what if I only want segments that have a `@mood` property (properties or attributes are prepended with an `@` sign in all XML-related languages)?

Then I can add square brackets to my path, at any point, and add some predicates—think of these as tests that must pass in order to return the specific results.

I can search for `//seg[@mood]`, which looks for any segment that has a mood attribute.

If I only want a certain value for a mood attribute, I can add an `=` sign and specify exactly what the value must be. For example, `//seg[@mood="indicative"]`.

## You can use XSLT

Sometimes your XML data would just work better if it was restructured a bit. This is where XSLT (eXtensible Stylesheet Language Transformations) come in. An  XSLT is an XSL transformation, which transforms an XML document based on matching templates. I'll write more about this some other time, since it's very powerful (and a completely different language in its own right!).

## You can use xQuery (BaseX)

If you want to run a complex query on your data, you can use xQuery.

The heart of an xQuery query is the FLWOR expression, ('flower expression').

FLWOR stands for the core components of what amounts to a looping query:

- For (some set of results)
- Let (some results can be accessed as a variable you set)
- Where (use some test to restrict the results)
- Order by (return the results in the order you want)
- Return (set up your return statement to output the format you need)

I'll write about xQuery later as well, since it is yet another language and it is a very powerful tool.

## You can use Python's LXML

If you don't have proprietary XML tools, you can run Python's LXML library. 

You just use `from lxml import etree`

And then create your tree as a variable:

`tree = etree.parse(file_path)`

Then you can access the tree using the `xpath` method:

```python
for element in tree.xpath('//seg[@type="w"]'):
    current_text.append(element.get('lemma'))
```

Remember, and XPath query generates an array of results, so the LXML library allows you to interact with the results just like any other Python array (with indexing, or looping through results, etc.).

## Output graphable data for graphing, statistics, etc.

Sometimes you just want a text result, but I've found it very helpful to build up a Python dictionary as I work, and then I can dump all of that using Python's JSON library.

Import using `import json`, and save your dictionary as a JSON using the `json.dumps()` method.

By the way, if you're using non-English characters, you probably need to use a special flag for the `dumps()` method. For example,

`json.dumps(my_xpath_results_dictionary, ensure_ascii=False)`

## That's it for now!

If you found this interesting or helpful, please share it on Twitter (I'll build a button for this some day–just copy the URL for now), and follow me as well.
