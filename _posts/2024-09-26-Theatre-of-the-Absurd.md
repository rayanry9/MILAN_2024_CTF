---
layout: post
title: Theatre of the Absurd 
date: 2024-09-25
categories: binaryExploitation 
---
# Theatre of the Absurd
Question:
![Question](https://media.discordapp.net/attachments/1009492231658418296/1288492984861462558/image.png?ex=66f5621a&is=66f4109a&hm=eb721409d84d2ee123acc0e98aeb3968c5e53f020e64339e8b348d78b7f23c03&=&format=webp&quality=lossless&width=880&height=603)

Clearly, the question wants us to connect to a server with port 6000
![connect to nc](https://media.discordapp.net/attachments/1009492231658418296/1288493321769193534/image.png?ex=66f5626a&is=66f410ea&hm=d127d8ef69e943f6aa12fc264189bb99c8cab8273fcd46af3d173acf82d2dbe3&=&format=webp&quality=lossless&width=1092&height=482)

After close inspection we can see references of Python abbreviated "py-4.51". This hints that the terminal program is written in Python.
![Confirm python](https://media.discordapp.net/attachments/1009492231658418296/1288494125628522496/image.png?ex=66f5632a&is=66f411aa&hm=520873388e01d90f0ad607a0cf189bb4a613f331dcdfe85ebcc778a9775d9ed1&=&format=webp&quality=lossless&width=677&height=191)
The above error confirms the program is written in python, but this also hints that the program tries to evaluate what is written since the error refers to accessing an unknown variable. This could come in handy when we try to inject our own scripts into the shell.

![Checking out commands](https://media.discordapp.net/attachments/1009492231658418296/1288494683516829761/image.png?ex=66f563af&is=66f4122f&hm=31d1c32b41f3e9557d19ddd904d766ed5c7dafc2372a8b0d70a0f756863ecdc7&=&format=webp&quality=lossless&width=796&height=353)

Checking out the help command refers us to a few other commands, most importantly the "dir" command to check what files are on the server. As the name suggests, the files, or contents of these files can be useful ahead.

Since the server can evaluate python code, we can quickly formulate a one line python script to print the content of the files.

```py
print(eval("open('warp-drive.txt').readlines()"))
```
```py
print(eval("open('top-secret.enc').readlines()"))
```

Running the above scripts in the shell gives us:
![Evaluated output](https://media.discordapp.net/attachments/1286609033343533058/1288240146310893671/image.png?ex=66f51f61&is=66f3cde1&hm=02692d64b5daf1349199595b70fe06fc602368082e49d86c55d65f550487bc75&=&format=webp&quality=lossless&width=1142&height=662)

The key as expected is in front of us.
`Key: milanCTF{pr3t3Nd_s3cur1ty_1s_n0t_c23af1}`
