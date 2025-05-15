---
title: "Organizing & Spacing for MacOS Menubar Icons"
date: 2024-02-14
tags: ["apple", "config"]
draft: false
---
Hold ⌘ (Command) while dragging to rearrange system icons in the MacOS menubar and adjust spacing using the `defaults` CLI tool. Smaller values = tighter spacing.
```
defaults -currentHost write -globalDomain NSStatusItemSpacing -int 8
defaults -currentHost write -globalDomain NSStatusItemSelectionPadding -int 8
```

Change your mind and revert to default: 
```
defaults -currentHost delete -globalDomain NSStatusItemSpacing
defaults -currentHost delete -globalDomain NSStatusItemSelectionPadding
```

This lightweight app has a convenient UI to achieve the same effect: https://sindresorhus.com/menu-bar-spacing.