---
title: "Remove Rosetta from MacOS on Apple Silicon"
date: 2024-01-23
tags: ["apple"]
draft: false
---
1. Reboot into recovery mode: https://support.apple.com/en-us/102518
1. Mount the data partition using Disk Utility.
1. Open Terminal & use `pkgutil` to list the relevant files on the mounted partition, e.g.  `pkgutil --files com.apple.pkg.RosettaUpdateAuto --volume /Volumes/Data`.
1. Delete those files & reboot.
1. Verify w/the CLI, e.g. `arch -x86_64 whoami` should return `arch: posix_spawnp: whoami: Bad CPU type in executable`.