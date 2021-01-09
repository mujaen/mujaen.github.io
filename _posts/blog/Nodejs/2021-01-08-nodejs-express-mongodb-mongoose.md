---
layout: post
title: "Node.js(Express)로 MongoDB(Mongoose) 연결하기(2)"
description: "Node.js + MongoDB 데이터베이스 연결"
category: blog
tags: nodejs
---


* this unordered seed list will be replaced by the toc
{:toc}

## MongoDB

데이터베이스를 관리하는 시스템(DBMS)에게 명령을 주고받아 DB의 데이터를 조작해야 하는데  
이때, DBMS가 사용자의 요청을 이해할 수 있도록 도와주는 언어가 바로 SQL이죠 대표적으로 MySQL이 있습니다   
MongoDB는 NoSQL, 도큐먼트 지향 데이터베이스 시스템입니다. 이 둘의 특징과 차이점은 이번 포스팅에서 다루지 않습니다   
DB를 만들고 서버에 연결해서 데이터를 CRUD하는 내용을 진행 하도록 하겠습니다.


### Create Account

데이터베이스 생성을 위해 [MongoDB](https://www.mongodb.com/)에 접속해 주세요   
저는 공부 목적이므로 Start free 버튼을 눌러 무료용으로 계정 가입을 할 겁니다.

![Start Free](/assets/img/2021-01-08/join.png)

> MongoDB는 현재기준 무료계정으로 가입 시 512MB의 저장소를 제공하고 있습니다!

### Create Cluster

계정을 생성한 뒤 build a Cluster 버튼을 눌러 새로운 클러스트를 만들어 줍니다.  
클라우드를 선택하는 화면이 나오면 원하는 옵션을 선택합니다. 저는 AWS로 선택하고 진행하겠습니다.   
대략 5-10분 정도 지나면 클러스터가 생성됩니다. 

![Create a New Cluster](/assets/img/2021-01-08/connect.png)

### Connect to Cluster

화면 중간에 SANDBOX 영역에서 CONNECT 버튼을 눌러 레이어창이 나오면 단계별로 진행합니다.     
IP를 입력해주고 DB에 사용될 아이디와 패스워드를 입력해 줍니다.  
다음 단계로 넘어가면 애플리케이션 연결(Connect your Application)을 눌러줍니다  

![Create a New Cluster](/assets/img/2021-01-08/code.png)

마지막 단계입니다. 드라이버와 버전을 확인하고 우측의 Copy 버튼을 눌러 코드를 복사합니다  
Mongoose를 사용하여 복사한 코드를 입력해 서버와 DB를 연결할 겁니다

## Mongoose