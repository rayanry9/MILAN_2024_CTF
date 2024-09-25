---
layout: post
title: Undercover 
date: 2024-09-25
categories: webExploitation
---

# Problem

![](../../../../assets/u-1.png)

We are given a website link.
And a goal to get admin creds.

# Solution

+ Opening the website gives us a login page asking Username and password.

![](../../../../assets/u-2.png)

+ Given the username as admin. 
+ As the password is not given, we try to bypass the password by using SQL injection.

+ By entering the username as “admin” and password as ' OR '1'='1' –
+ The ' (single quote) at the beginning breaks out of the current string. As entered by the user is generally given as a string to the SQL query.
+ By using some OR condition with some always True statement like '1'='1' which is always true makes the password part of the query to True.
+ Finally, – is used to comment out everything after, which may include extra logics like additional checks which may be present in the query.

![](../../../../u-3.png)

+ Doing this gives the output as “Welcome, admin! Here is the secret message: milanCTF{wh0_1s_7h3_adm1n_n0w_89324}”

