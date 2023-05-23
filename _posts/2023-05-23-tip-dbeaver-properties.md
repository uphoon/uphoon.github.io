---
layout: post
title: DBeaver Table 및 Column 정보 중 Description에서 한글 안나오는 문제
tags:
  - TIP
  - DBeaver
---

>*DBeaver에서 오라클 연결을 여러가지 환경때문에 기존에 Connection을 사용하지 못하고 신규(New Connection...) JDBC을 만들어 연결하였다.*

>*테이블 Create 시 Table이나 Column에 적용한 Comment에 입력한 한글 정보가 보이질 않았다.*


### 해결안

Driver properties에서 아래 속성 값을 추가하였다.

~~~
remarksReporting true
~~~

![dbeaver_driver_properties](https://github.com/uphoon/uphoon.github.io/releases/download/posts/dbeaver_tip.jpg "Driver properties")

*JDBC URL 설정*

~~~
jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS_LIST=(LOAD_BALANCE=ON)(ADDRESS=(PROTOCOL=TCP)(HOST=192.0.0.1)(PORT=1522))(ADDRESS=(PROTOCOL=TCP)(HOST=192.0.0.2)(PORT=1522)))(CONNECT_DATA=(SERVICE_NAME=FOOSRVC)))
~~~


