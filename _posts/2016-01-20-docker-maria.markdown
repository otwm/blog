---
layout: post
title:  "docker maria"
comments: True
---

docker search mariadb   

docker pull mariadb   

docker run --name mariadb-10.0 -e MYSQL_ROOT_PASSWORD=mypass -d -p 3306:3306 mariadb    

docker ps   

docker restart mariadb-10.0

[install maria](https://mariadb.com/kb/en/mariadb/installing-and-using-mariadb-via-docker/)
