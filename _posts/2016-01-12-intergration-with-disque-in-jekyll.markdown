---
layout: post
title:  "지킬에서 disque 통합"
comments: True
---
## 지킬에서 disque 통합   
지킬은 static 기반의 데이터베이스가 같은 저장소가 필요한 경우에 난감하다.    
블로그 기반의 서비스이므로 특히, 코멘트가 필요하지만, 지킬의 경우에는 기본적으로 코멘트를 구성할 수가 없다.   
그래서 코멘트를 지원하는 다른 서비스와의 통합이 필요한데, 코멘트를 지원하는 disque 서비스와의 통합에 대해서 설명한다.
disque와의 통합은 매우 간단하다.     
구글 애널리스틱을 서비스에 설치해 본 사람이라면 쉽게 이해할 수 있을듯 하다.   
지킬 역시 간단하게 소스코드를 넣는 것으로 쉽게 설정 가능하다.( jekyll-bootstrap의 경우에는 이 마저도 미리 설정이 되어있다.)   
다음의 절차로 간단히 코멘트를 블로그에 넣을 수 있다.

다음)       
1. disque 가입
2. disque 서비스 내의 코멘트의 설정
3. 지킬 설정에 필요한 shotName과 삽입 코드를 저장해 둔다.
4. _config.yml 수정
5. 삽입코드 저장
6. 포스트 레이아웃 변경
7. 포스트에 커맨트 설정 추가
8. 테스트!!!

====================================================================  
1. disque 가입
  - https://disqus.com/ 로 이동하여 회원을 가입한다.    
  [가입관련 참조 링크](http://onasaju.tistory.com/182) << **disqus 가입 관련이 여기가 더 잘 설명 되어 있어요!**
2. disque 서비스 내의 코멘트의 설정
  - 로그인 후 해당 화면에서 Add disqus to Site 를 클릭!!  
  ![disque1](/blog/images/disque1.png "disque1")
  - Start to Engage 클릭    
  ![disque2](/blog/images/disque2.png "disque2")    
  - Site name, disque url, Category 입력    
  ![disque3](/blog/images/disque3.png "disque3")    
  Site name : 사이트명    
  disque url : 나의 디스커스 댓글 커뮤니티를 확인할 수 있는 SNS 개념의 웹 주소 입니다. 사이트 이름을 입력하면 자동으로 사이트 이름으로 입력이 되며, 변경 또한 가능 합니다.   
  Category : 자신의 웹 사이트(블로그) 카테고리를 설정하는 곳인데, 목록에 없다면 “Auto’ 로 해 주세요!   
  - Universal code 클릭   
  ![disque4](/blog/images/disque4.png "disque4")
  - 이제 스니핏을 볼 수 있습니다만, 실제 댓글에 대해 커스터 마이징 하도록 다음 절차를 실행 해주세요.    
    - Admin 메뉴 선택 후 다음 페이지에서 edit settings 를 선택   
     ![disque4](/blog/images/disque5.png "disque5")    
    -  disque 설정  
       ![disque6](/blog/images/disque6.png "disque6")    
       ![disque7](/blog/images/disque7.png "disque7")    
       ![disque8](/blog/images/disque8.png "disque8")    
        Appearance(외관)    
        ➥ 색상, 서체 등을 설정할 수 있습니다. 기본값으로 두세요!    

        Comment Count Link    
        ➥ 댓글 수 링크 입니다. 기볼값으로 두시면 됩니다!   

        Default Sort(기본 분류)   
        ➥ 어떠한 댓글들이 가장 먼저 보여주게 할건지를 설정 할 수 있습니다. 최고의 댓글, 오래된 댓글, 최신의 댓글…   

        Discovery   
        ➥ 디스커스 댓글목록 아래에 내가 활동한 다른 댓글 커뮤니티 링크를 보여줄건지 말건지를 선태할 수 있습니다. 체크하는 것을 권장 합니다.    

        Site Identity(사이트 정보)   
        ➥ 사이트(블로그) 이름, URL 주소, 카테고리, 설명, 사용 언어를 입력하는 곳 입니다. 이 항목에 있는 사항들을 정확하게 입력해야 install 페이지에서 정상적으로 인증이 완료 됩니다. 그리고 사용 언어에서 ‘korean’으로 해 주어야 디스커스 댓글 폼에 한글로 표시가 됩니다.    

        Community Rules(디스커스 댓글 커뮤니티 규칙)    
        ➥ Guest Commenting 항목에 체크를 해야 ‘비회원’도 이름과 이메일 주소만으로 댓글을 남길 수 있습니다.   
        ➥ Pre-moderation 에서는 댓글을 승인해야 노출이 되게 할건지, 이메일 주소가 없는 사람의 댓글일때만 검토후에 승인할건지, 승인절차 없이 모든 댓글이 노출되게 할건지를 설정할 수 있습니다.   
        ➥Links in Comments 링크가 있는 댓글일 경우에 승인을 해줘야 노출이 되게 할건지를 설정 할 수 있습니다.    
        ➥Flagged Comments 신고된 댓글에 대해서 어떤 조치를 할건지를 설정 할 수 있습니다.
        ➥ Automatic Closing 자동으로 특정시간, 특정기간 동안 댓글 입력 폼을 폐쇄 할 수 있습니다.    

        Social Platform Integration   
        ➥ 트위터 사용자 이름을 지정할 수 있습니다.   

        그리고 맨 아래에 있는 ’Save Changes’ 를 클릭해서 저장을 해 주세요! 그리고 install 페이지로 다시 이동을 해서 디스커스 댓글 폼을 설치 해 주면 됩니다.    
3. 여기 까지 잘 되었다면, 아까의 스니핏을 사용 할 수가 있습니다.단지 사이트 내에 스니핏을 붙이는 것 만으로 댓글이 샆입이 됩니다. 더 많은 설정은 disque 사이트 내의 여러 설정 등을 살펴 보세요.  
   shotName은 프로필 정보의 Username입니다.
4. _config.yml 수정   
아래 처럼 disqus 설정을 추가
       comments :    
         provider : disqus   
         disqus :    
           short_name : disqus_XXXXX(show name)    
5. 삽입코드 저장
_includes/comments.html 생성    
         {% if page.comments %}  
         // disque 스니핏  
        {% endif %}  
프로그래머분들은 블록 안에 코드가 어떤 말인지 쉽게 아시겠죠?          
6. 포스트 레이아웃 변경       
_layouts/post.html에 코멘트를 달 위치에 아래의 코드를 추가  
       {% include comments.html %}  
7. 포스트에 커맨트 설정 추가
헤더(?)에 다음 구문을 추가 해주세요. -> ** comments: True **  
8. 테스트!!!
자 이제 대략 작업이 끝났군요. 모든 것이 잘 되었는지 테스트 하면 되겠습니다. 참고로 구글 애널리스틱 역시 이와 유사하게 진행 하면 되겠습니다~~.
