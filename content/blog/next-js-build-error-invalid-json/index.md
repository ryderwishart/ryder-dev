---
title: "Fix FetchError: invalid json response body"
date: "2021-06-17"
description: "Have this kind of error?: 'Unexpected token (< or I or something) in JSON at position 0'. Here's a fix."
---

## Problem

I almost always have problems building my Next.js app locally (which almost always means I'll have problems deploying it). Sometimes you just need to temporarily delete all your `/pages/` except your `/pages/api/` routes.

If you are getting some kind of `FetchError: invalid json response body`, it means there is an error getting results from your API endpoint. If you Next app is trying to build a page before the entire deploy has completed, and that page requires a brand new API endpoint you just made, then you run into the problem where **you can't build your app because it needs to use an API endpoint that doesn't exist on the web yet**.

## Solution

Whenever you add a new API route, you need to deploy the API route *before* you deploy the pages that use that route.

If you build your nifty page at the same time, you can just try deploying without any of my `/pages/` routes in order to allow the deploy to complete with the API endpoints only. (You could also try only deleting the routes that utilize your new API endpoints, but I haven't always had success with this approach.)

1. Push to `master` the final code you would like to see deployed.
2. Delete all your `/pages/` routes **except the `/pages/api/` routes**, and push this change to `master` (in most cases this will trigger a new deploy). If you are building locally, then run `yarn build`, `next build`, or whatever your build command is.
3. Revert your most recent commit where you deleted all those `/pages/` routes, and push this to `master`.

This should result in your app successfully deploying, including the new API routes and all the routes that query those endpoints for server-side rendering.

If you have a live app people are relying on, you might want to do this at night! 🌙
