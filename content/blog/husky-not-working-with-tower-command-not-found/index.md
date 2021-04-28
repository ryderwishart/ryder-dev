---
title: Husky pre-commit hooks not working with Tower GUI
date: "2021-04-27"
description: "Command not found? Here's the fix"
---

## Problem

When you try to commit files in [Tower](https://www.git-tower.com/), you can run into issues if you have Husky pre-commit hooks installed in your project but not globally on your machine.

## Solution

If you don't want to install Husky globally, you can do the following:

Go to your home directory (wherever your `.bashrc` file is located, for example), and create a new file called `.huskyrc`.

Inside `.huskyrc`, write this:

`PATH="/usr/local/bin:$PATH"`

This should help Tower use Husky to run some pre-commit type-checking, or whatever it is you want to do!
