---
layout: post
title: Spectral Secrets
date: 2024-09-25
categories: 
---

# Problem

![](../../../../assets/ss-1.png)

We get audio file `chall.wav` from this

# Solution

+ Running `binwalk` gives us a source code with characters eaten away and a binary.
+ We fill the source code.
+ A comment is there regarding key and temp_conv
+ using that and the formulas in the source key we calculate the flag.
