---
layout: post
title:  "How-to disable Intel Turbo Boost on Intel NUC"
date:   2023-01-10 09:00:00 +0800
categories: cli linux intel
---

After buying Intel NUC as homelab server I found, that I want to keep it as cold as possible, while I don't have any highloads.
Most logical step, as for me - disable Intel Turbo Boost technology.

## TLDR

### Checking possibility to disable it

  ```sh
  echo "1" | sudo tee /sys/devices/system/cpu/intel_pstate/no_turbo
  ```

### Save parameters for future

  ```sh
  echo 'w /sys/devices/system/cpu/intel_pstate/no_turbo - - - - 1' | sudo tee /etc/tmpfiles.d/noturbo.conf
  ```

## Points

* Of course, you can disable it in BIOS settings
* There is a lot of other tools, which can help with it, but for me it's enough

That's all.

## Additional links

1. [man - tmpfiles.d](https://manpages.ubuntu.com/manpages/jammy/man5/tmpfiles.d.5.html)
2. [github - erpalma/throttled](https://github.com/erpalma/throttled)
3. [reddit - Disable intel turbo - Patch in /sys/devices/system/cpu/](https://www.reddit.com/r/linuxquestions/comments/m4xnts/disable_intel_turbo_patch_in_sysdevicessystemcpu)
