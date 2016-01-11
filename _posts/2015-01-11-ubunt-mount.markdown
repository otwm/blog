---
layout: post
title:  "ubuntu mount"
---
    
디스크 보기   
sudo fdisk -l   
    
언마운트    
sudo umount ...   
    
마운트    
sudo mount <디바이스> <연결할포인트>    
    
현 마운트 상태 보기   
cat /etc/mtab   
    
시작시 마운트 정의    
cat /etc/fstab    
    
장치 UUID 정보
sudo blkid

[파티션자동마운트]:(http://blog.simplism.kr/?p=2578)    

