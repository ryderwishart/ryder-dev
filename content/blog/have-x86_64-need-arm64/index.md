---
title: "Fix `mach-o file, but is an incompatible architecture (have 'arm64', need 'x86_64')` error 🤦‍♂️"
date: "2023-05-17"
description: "A simple solution for a common problem when your `pip` environment is all messed up on Mac M1 machines."
---

## Problem

If you are using a Mac M1 machine and trying to install some Python libraries using pip, you may encounter this error:
```bash
ImportError: dlopen (/Users/.../site-packages/.../...so, 0x0002): tried: '/Users/.../site-packages/.../...so' (mach-o file, but is an incompatible architecture (have 'arm64', need 'x86_64'))
```

This means that the library you are trying to import was compiled for a different architecture than your machine. In this case, the library was built for x86_64 (Intel) and your machine is arm64 (Apple Silicon).

This is annoying because you can very specifically make sure the arm64 version is installed and still get this error on certain dependencies, AND it doesn't show up until you try to call the code. Boo!

## Solution

One possible solution is to install the library directly from GitHub using pip. For example, if you are having trouble with `hnswlib`, you can try this:

```bash
git clone https://github.com/nmslib/hnswlib.git
cd hnswlib
pip install .
```

This will clone the repository and install it from source, which should compile it for your machine's architecture. You can do this for any library that has a GitHub repository and a setup.py file. (Because, obviously, as you try to fix your environment issues you will inevitably create more issues.)

## Conclusion

I hope this helps you solve this annoying error and enjoy using Python on your Mac M1 machine. Happy coding! 😊