---
title: Add Typescript to Existing Next.js project with Yarn 🧞‍♂️
date: "2021-04-06"
description: "Super simple code to add TS to your existing Next.js app using Yarn instead of NPM."
---

## Step 1

In the root folder of your project (where your `package.json` file is), put this into Terminal:

```bash
touch tsconfig.json
```

## Step 2

Now install Typescript into your Next.js app using Yarn:

```bash
yarn add typescript @types/react @types/node
```

## Extras

Don't forget to restart your development server if it's open in another Terminal.

Also, I would highly recommend adding to the file inclusions in your `tsconfig.json` file:

```json
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx",
    "**/*.css", 👈
    "**.*.js" 👈
  ],
```
