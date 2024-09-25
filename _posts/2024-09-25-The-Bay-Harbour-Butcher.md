---
layout: post
title: The Bay Harbour Butcher 
date: 2024-09-25
categories: forensics 
---

# Problem

![](../../../../assets/bhb-1.png)

This challenge gave a zip file containing 3989 text files and asked us to get the key from it.

# Solution

+ On examining, `1.txt` we find that it contains `ELF` which usually corresponds to a binary that can be run on linux.

+ So, I made a script to combine all the files starting from `1.txt` to `3989.txt`.

+ The Bash Script being:

```{bash}
#!/bin/bash

for i in {1..3989};
do
    cat "./Q2/$i.txt" >> "./merge";
done
```

+ This results in a file.
+ On `binwalk`ing the file, We say that it is an ELF image file ( A BINARY ).

![](../../../../assets/bhb-2.png)

+ We run the file.

![](../../../../assets/bhb-3.png)

+ We get a encrypted message which looks like `base64`.

+ We decode the `base64` message using any online tools.

+ We get our key : milanCTF{Daaaayam_y0u_smart_b0iiii11}
