---
title: Fix random Error: pageNotFoundError while building Next.js app
date: "2021-06-17"
description: "'Build error occurred Error: Export encountered errors on following paths' shows different list of paths every time."
---

## Problem

I almost always have problems building my Next.js app locally (which almost always means I'll have problems deploying it).

Sometimes I need to try deploying without any of my /pages/ routes in order to allow the deploy to complete with the API endpoints only.

However, sometimes I just repeatedly hit this error:

```bash
> Build error occurred
Error: Export encountered errors on following paths:
```

Confusingly, when I retry building I end up with a different list of paths virtually every time.

## Solution

If you're bashing your head against the wall trying to figure out why you're encountering a build error, try this:

Start your local Next.js server using `next start` or `yarn start` or `npm start`, and then navigate to all of the pages or paths giving you build problems. Now try building again in another terminal window. In some cases simply building the pages on the fly locally through your development server allows the build to proceed.
