---
layout: post
title:  "virtualbox 게스트 확장 설치"
---
    
 1. 우분투에 로그인을 합니다. (터미널, SSH 등으로 접속하여 작업하지 마시고, 로컬에서 작업하세요.)   
 2. 터미널을 하나 띄웁니다.   
 3. 게스트확장을 설치하기 전에 우분투 패키지의 업데이트 사항을 확인합니다.    
   ($ sudo apt-get upgrade)   
 4. 게스트확장설치에 필요한 패치키를 추가로 설치합니다.   
   ($ sudo apt-get install build-essential module-assistant)    
 5. 다음 명령어를 이용하여 커널 설정을 진행합니다.    
   ($ sudo m-a prepare)   
 6. VirtualBox 메뉴 -> 장치 -> 게스트확장설치를 클릭합니다. 우분투의 시디롬에 나타나는 것을 확인할 수 있습니다.   
 7. 다음 명령어를 이용하여 게스트확장을 설치합니다.   
   ($ sudo sh /media/cdrom/VBoxLinuxAddition.run)   
   
[VirtualBox Guest Addition(게스트확장) 설치하기]:(http://dante2k.tistory.com/452)
