---
layout: post
title:  "Docker Study 1"
date:   2015-12-27
comments: True
---

Docker는 좋은 거임!    
관련 방화벽 열어주기   

    sudo ufw allow 4243/tcp   

권한 설정   

    $ sudo groupadd docker    
    $ sudo gpasswd -a ${USER} docker    
    $ sudo service docker restart   

이미지 생성

    docker commit [container] [repository:(tag)]

docker file example

    #기본정보
    FROM ubuntu:12.04
    MAINTAINER Daekwon Kim <propellerheaven@gmail.com>

    # 업데이트 및 저장소 추가
    # Run upgrades
    RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" >     /etc/apt/sources.list
    RUN apt-get update

    # apt 설치
    # Install basic packages
    RUN apt-get -qq -y install git curl build-essential

    # apt 설치
    # Install apache2
    RUN apt-get -qq -y install apache2
    ENV APACHE_RUN_USER www-data
    ENV APACHE_RUN_GROUP www-data
    ENV APACHE_LOG_DIR /var/log/apache2
    RUN a2enmod rewrite

    # apt 설치
    # Install php
    RUN apt-get -qq -y install php5
    RUN apt-get -qq -y install libapache2-mod-php5

    # 설정 등...
    # Install Moniwiki
    RUN apt-get install rcs
    RUN cd /tmp; curl -L -O http://dev.naver.com/frs/download.php/8193/moniwiki-1.2.1.tgz
    RUN tar xf /tmp/moniwiki-1.2.1.tgz
    RUN mv moniwiki /var/www/
    RUN chown -R www-data:www-data /var/www/moniwiki
    RUN chmod 777 /var/www/moniwiki/data/ /var/www/moniwiki/
    RUN chmod +x /var/www/moniwiki/secure.sh
    RUN ./var/www/moniwiki/secure.sh

    #실행 시 정의    
    EXPOSE 80
    CMD ["/usr/sbin/apache2", "-D", "FOREGROUND"]

이미지는 각 RUN 마다 생성이 된다.
> 그렇다면 어떻게 이미지의 내용을 알 수 있을까??

다음은 천천히 알아보자.
* shipyard
* Doku
* Docker ui
[nacyot doker]:(http://blog.nacyot.com/articles/2014-01-27-easy-deploy-with-docker/)
