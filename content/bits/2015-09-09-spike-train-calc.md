---
title: "Electrophysiology Spike Train Calculator"
date: 2015-09-09
tags: ["science"]
draft: false
---
Converts between inter-pulse intervals/train burst widths and frequency/number of pulses & plots an example waveform (also my first foray into JS/jQuery). Mostly useful for visualizing spike trains in discussions or double checking math when hopping between ephys equipment.

- Demo: https://hologramber.github.io/tools/ephys_calc.html
- Code: https://github.com/hologramber/ephys-stimulation

The example waveform will not plot until all values have been entered. Choose between calculating the waveform from the inter-pulse period and the train burst width, or from the # of pulses and the frequency of those pulses -- the alternate option will be displayed automatically.  Scientific notation is displayed on the right for reference with systems that require SN (AM Systems, yay).