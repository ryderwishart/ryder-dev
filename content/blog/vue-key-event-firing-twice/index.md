---
title: "Stop Vue 3 key events from firing twice"
date: "2022-06-10"
description: "How to stop Vue 3 key events from firing twice with one word: 'STOP'."
---

## The Problem: Vue3 Key Events Sometimes (or Always) Fire Twice

If you are using Vue 3, it can be annoying and confusing when key events fire twice.

Here's the fix:

## Solution: Use the `.stop` Modifier

Instead of `@keydown.enter.exact.prevent="brk('node-and-siblings')"`, use `@keydown.enter.exact.stop.prevent="brk('node-and-siblings')"`.

Notice the `.stop` modifier.

To translate these modifiers, they mean, in order:

- `@keydown` is the keydown event you want to capture.
- `.enter` means use the 'enter' key.
- `.exact` means use the exact keycode (i.e., 'shift + enter' won't fire this event handler).
- `.stop` means stop the event from bubbling up, only let it fire **once**.
- `.prevent` means prevent the default action (whatever else 'enter' would have done in context).

Hope you found this helpful!
