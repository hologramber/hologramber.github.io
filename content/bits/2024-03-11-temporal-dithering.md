---
title: "Disable Temporal Dithering on MacOS (Apple Silicon)"
date: 2024-03-11
tags: ["apple", "config"]
draft: false
---
Temporal dithering (aka frame rate control) flips each pixel between two nearby color codes frame-by-frame at the speed of the display’s refresh rate, so your eyes merge them into the “missing” shade, tricking your eyes into perceiving more colors than the display is capable of. 

One of my external monitors had severe banding/flicker whenever dark+cool tones were covering the majority of the screen, making it practically unusable. <a href=“https://github.com/aiaf/Stillcolor”>This open source MacOS utility (Stillcolor)</a> disables GPU/DCP-generated temporal dithering from the user space, and solved the issue entirely — thank you, kind author, you are a treasure of humanity and a savior of eyes.

To check whether temporal dithering is enabled on MacOS: `ioreg -lw0 | grep -i enableDither`; each display will have a separate record for `”enableDither”` (and either `Yes` or `No` accordingly).