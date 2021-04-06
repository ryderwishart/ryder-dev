---
title: Grommet DataTable ReferenceError defaultProps
date: "2021-04-06"
description: "Fix the ReferenceError: defaultProps is not defined when using Grommet's DataTable Component."
---

## Error

`ReferenceError: defaultProps is not defined`

## Solution

Did you forget to import React?

Add `import React from 'react'` to the top of your file.

## Prevent this error in the future

Consider using Typescript for future projects, as it will help you sort out these and many other similar issues.
