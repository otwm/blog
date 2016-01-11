---
layout: post
description:true
title:  "new blog process (jekyll install)"
---
1.ruby 설치      
2.jekyll 설치 및 설정      
3.에디터         


1.ruby 설치         
[ruby install](http://bigmatch.i-um.net/2013/12/%EB%A9%98%EB%B6%95%EC%97%86%EC%9D%B4-rvm%EA%B3%BC-%EB%A3%A8%EB%B9%84-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0/)  

루비는 2.0 이상 버전이 필요하다. 그러므로 apt-get 을 이용한 설치는 추천하지 않는다.  
또한 레포지토리 중 비 정상적인 레포지토리가 있을 경우 삭제 하여야 한다.  
루비는 rvm 을 통해 일반적으로 설치 버전이 관리된다.  

(1) curl -L https://get.rvm.io | bash -s stable  
(2) source ~/.rvm/scripts/rvm (설치하는 환경에 따라 경로는 바뀔 수 있다.)  
(3) rvm list known  
(4) rvm install 2.2  
(5) rvm use 2.2  

2.jekyll 설치  
[jekyll install](https://nolboo.github.io/blog/2013/10/15/free-blog-with-github-jekyll/)  
[about jekyll](https://jekyllrb.com/)  
[about jekyll(ko)](http://jekyllrb-ko.github.io/)  

(1)sudo apt-get install nodejs  
(2)gem install jekyll  

자, 이제 jekyll이 설치 되었다면, 프로젝트를 생성하고 서버를 생성해 보자.  
(3) jekyll new 깃허브사용자명.github.com  
(4) cd 깃허브사용자명.github.com  
    jekyll serve --watch  
    localhost:4000 으로 접속하여, 생성된 사이트를 확인한다.  
(5) 깃허브사용자명.github.io 라는 저장소 생성 후 커밋 앤 푸쉬  

  git init  
  git remote add origin 저장소URL  
  git add .  
  git commit -m "Initialize blog"  
  git push origin master  

깃허브사용자명.github.io으로 가면 생성되어 있다!  

이제는 블로그 프로젝트 생성  
(6) jekyll new blog // blog 디렉토리를 만들어 기본 디렉토리와 화일을 생성한다.  
    cd blog  
    jekyll serve --watch  

    git init  
    git checkout --orphan gh-pages //gh-pages 로 생성해야 깃허브 플젝으로 생성 되나 보다.  

    git remote add origin 저장소url  
    git add .  
    git commit -m "Initialize blog"  
    git push origin gh-pages  // 반드시 gh-pages 브랜치로 푸시해야 깃허브가 블로그 페이지를 만들어 준다.  

    깃허브사용자명.github.io/blog 로 가서 확인!  

(7) 파일_config.xml 을 수정하여 기본적인 정보를 수정한다.  

3.에디터  
에디터는 sublime 과 atom 이 있으나, 서브라임 경우는 한글이 번거로운 관계로 atom을 추천한다.  
참고로 아톰에는 jekyll 관련 플러그인도 있으니 살펴보자.  

[atom](https://atom.io/)  
[atom-jekyll](https://atom.io/packages/jekyll)  

[jekyll theme](http://jekyllthemes.io/)  
