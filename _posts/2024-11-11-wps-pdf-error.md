---
layout: post
title:  "WPS Office Export to PDF Error"
author: tap
categories: [ Logs, English ]
tags: [ Linuxmint, Ubuntu, WPS, PDF ]
image: s/images/wps-pdf-error.webp
beforetoc: "It was a fresh install but WPS can not export to PDF"
toc: false
---

I've just installed Linuxmint 22 Xfce on one of my laptops. And for interoperabilty with other colleagues I had to have WPS Office. But it seems that it doesn't work as expected, the PDF export experienced an error with the message as displayed on the image above.

A quick search suggested to check if libtiff5 installed and also bring me to a Reddit post with the solution to check if I have libtiff.so.6.

```shell-session
me:~$ file /usr/lib/x86_64-linux-gnu/libtiff.so.6
libtiff.so.6      libtiff.so.6.0.1  
```

and make a symlink to libtiff5

```shell-session
me:~$ sudo ln -s /usr/lib/x86_64-linux-gnu/libtiff.so.6 /usr/lib/x86_64-linux-gnu/libtiff.so.5
```

... and it works!

![Yes, it works!](https://static.tapsaja.eu.org/s/images/wps-pdf-ok.webp)
