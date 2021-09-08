---
title: "Fix random Error: pageNotFoundError while building Next.js app"
date: "2021-09-07"
description: "'Build error occurred Error: Export encountered errors on following paths' shows different list of paths every time."
---

## Problem

I almost always have problems building my Next.js app locally (which almost always means I'll have problems deploying it). Here's my top two fixes to troubleshoot:

## Delete all your `/pages/` except your `/pages/api/` routes

Whenever you add a new API route, you need to deploy the API route *before* you deploy the pages that use that route.

If you build your nifty page at the same time, you can just try deploying without any of my `/pages/` routes in order to allow the deploy to complete with the API endpoints only. (You could also try only deleting the routes that utilize your new API endpoints, but I haven't always had success with this approach.)

1. Push to `master` the final code you would like to see deployed.
2. Delete all your `/pages/` routes **except the `/pages/api/` routes**, and push this change to `master` (in most cases this will trigger a new deploy). If you are building locally, then run `yarn build`, `next build`, or whatever your build command is.
3. Revert your most recent commit where you deleted all those `/pages/` routes, and push this to `master`.

This should result in your app successfully deploying, including the new API routes and all the routes that query those endpoints for server-side rendering.

If you have a live app people are relying on, you might want to do this at night! 🌙

## 

Sometimes, whatever I do, I just repeatedly hit this error:

```bash
> Build error occurred
Error: Export encountered errors on following paths:
...
```

Confusingly, when I retry building I end up with a different list of error-throwing paths virtually every time.

If you're bashing your head against the wall trying to figure out why you're encountering a build error, try this:

Start your local Next.js server using `next start` or `yarn start` or `npm start` (or whatever you use), and then **navigate to all of the pages or paths giving you build problems**. 

Now try building again in another terminal window. In some cases simply building the pages on the fly locally through your development server allows the build to proceed. 

Weird, right?
