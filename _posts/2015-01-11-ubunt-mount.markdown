---
layout: post
comments:true
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
{% if post.comments %}
<div id="disqus_thread"></div>
<script>
/**
* RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
* LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL; // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');

s.src = '//otwm.disqus.com/embed.js';

s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
{% endif %}
