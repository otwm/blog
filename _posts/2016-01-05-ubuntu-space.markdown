---
layout: post
title:  "ubuntu 공간 정리"
comments: True
---
  
[출처]:(http://inia30.tistory.com/9)  
  
1. uname -r  
2. dpkg -S vmlinuz  
3. dpkg -l "*3.13.0-40*" | grep ^ii
4. sudo apt-get purge linux-headers-3.13.0-43 linux-headers-3.13.0-43-generic linux-image-3.13.0-43-generic linux-image-extra-3.13.0-43-generic  
5. du -sh /boot  
  
