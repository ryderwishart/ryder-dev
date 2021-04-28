---
title: TypeScript property does not exist on type '{}' for imported object
date: "2021-04-27"
description: "Avoid type errors when you import JSON directly"
---

## Problem

Imagine you import a JSON file directly (like in your Next.js API):

`import root from '../../../../public/data/stages/N1904.Matt.json';`

When you try to access properties you know exist on `root`, e.g. `root.childProperty` you may get a nasty type-checking surprise:

```bash
error TS2322: Type '{ text: { key: string; content: { turn: { key: string; content: { stage: { key: string; content: { move: { key: string; expressions: { word: { key: string; lemma: string; norm: string; string: string; }; }[]; meanings: { ...; }[]; }; }[]; }; }[]; }; }; }; } | undefined' is not assignable to type 'TextContainer'.
  Type 'undefined' is not assignable to type 'TextContainer'.
```

Essentially, your TypeScript server cannot validate the types for the imported JSON.

## Solution

Just access the problem property as a string index:

Change  `root.childProperty` to `root['childProperty']`. Boom!
