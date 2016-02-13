---
layout: post
title:  "new blog process (jekyll install)"
comments: True
---

### z shell and oh my zshell
        
zsh --version
zsh 5.0.7
$ which zsh               #쉘의 위치를 확인한다.
/usr/bin/zsh

$ chsh -s /usr/bin/zsh    #기본 쉘을 변경한다.

$ chsh -s `which zsh`     #위 두 개의 명령을 하나로 줄일 수도 있다.
$ echo $SHELL
/usr/bin/zsh

curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
   
   
#### 터미널 분할  
apt-get install terminator


### ~/.zshrc
[[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm"


rvm use 2.2

### jekyll   

gem install jekyll -v '3.1.1'
