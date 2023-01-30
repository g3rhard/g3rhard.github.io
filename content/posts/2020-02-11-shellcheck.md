---
layout: post
title:  "Useful utilities - shellcheck"
date:   2020-02-11 09:00:00 +0800
categories: english nix
---

ShellCheck - is a new tools for me, which can help to debug and beauty my shell scripts. Only today I refactored my backup script, using hints in VSCode.
This tool have big documentation with explanation, why you must check and fix your code. This is a useful utility that everyone needs in his environment.

Simple install (in MacOS):

```sh
brew install shellcheck
```

You can also install plugin for Visual Studio Code:

```sh
code --install-extension timonwong.shellcheck
```

This plugin also support run from WSL.

That's all.

## Additional links

1. [ShellCheck](https://www.shellcheck.net/)
2. [Github - ShellCheck](https://github.com/koalaman/shellcheck)
