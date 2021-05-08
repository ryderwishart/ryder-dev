---
title: How to Declare a Namespace in XQuery
date: "2021-05-08"
description: "A simple example of how to declare a namespace in XQuery so you can quickly and easily run XPath and XQuery on namespace elements"
---

## Namespace prefix has not been declared error

Namespaces can be a big pain. If you try to run an XQuery on something like an XSL stylesheet (where all the XSL elements are prefixed with the `xsl:` prefix), you might run into an error like this:

`Namespace prefix 'xsl' has not been declared`

## Solution

The solution is as simple as declaring the namespace. Here's an example:

```xml
declare namespace xsl = "http://www.w3.org/1999/XSL/Transform";
count(//xsl:template)
```
