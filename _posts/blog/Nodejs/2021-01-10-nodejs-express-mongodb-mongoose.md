---
layout: post
title: "Node.js(Express)로 MongoDB(Mongoose) 연결하기(3)"
description: "Node.js + MongoDB 데이터 CRUD"
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

[Mongoose](https://mongoosejs.com/docs/guide.html)는 Node.js와 MongoDB를 연결해 주는 ODM 라이브러리 중 가장 많이 쓰입니다.    
문서를 객체와 매칭한다 해서 ODM(Object Document Mapping)이라 하죠.  
Mongoose의 장점은 Express에 스키마와 모델을 적용할 수 있다는 장점이 있고  
그로 인하여 Collection을 수월하게 관리할 수 있다는 점 외에도 여러 기능이 있습니다. 


### Install Mongoose

express 폴더를 열어주세요 이전 포스팅 그대로 이어서 진행하겠습니다.      
터미널을 열고 아래의 명령어를 입력하여 Mongoose를 설치해 주세요  

```shell
npm install mongoose
```


### app.js

Mongoose가 설치되었다면 이제 서버에 DB를 연결해 줄 겁니다. require로 Mongoose 모듈을 불러옵니다.  
아래의 DB 연결 부분을 봐주세요 저는 url을 변수로 사용하여 이전에 복사했던 코드를 넣어줄 겁니다.  
id와 password는 가입할 때 사용했던 그대로 입력해 줍니다. dbname은 사용하고자 하는 DB의 Collection명 입니다.  

```javascript
const express = require('express');
const app = express();
const router = require('./routes/app');
const mongoose = require('mongoose');

//express 설정
app.set('views', __dirname + '/views');
app.set('view engine', 'pug');
app.use('/', router);

//서버 연결
const server = app.listen(3000, () => {
    console.log('Express server has started on port 3000');
});

//DB 연결
let url =  "mongodb+srv://id:password@cluster0.dml5n.mongodb.net/dbname";
mongoose.connect(url, {useUnifiedTopology: true, useNewUrlParser: true});

let db = mongoose.connection;

// DB 연결 실패
db.on('error', function(){
    console.log('Connection Failed!');
});

// DB 연결 성공
db.once('open', function() {
    console.log('Connected!');
});

```

> connect 메서드를 사용하여 DB에 연결합니다 이때, 문자열 처리 관련해서 여러 매개 변수를 옵션으로 지정할 수 있습니다

모든 작업이 완료되었다면 터미널을 열어 'npm start' 명령을 입력하여 서버를 구동시켜 봅니다    
DB 연결에 성공하였다면 아래와 같이 터미널에 콘솔이 출력되는 것을 확인할 수 있습니다!  


![Check Console Message](/assets/img/2021-01-08/terminal.png)