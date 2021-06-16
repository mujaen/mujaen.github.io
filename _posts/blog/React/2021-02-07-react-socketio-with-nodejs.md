---
layout: post
title: "React + Socket.io로 만드는 채팅 애플리케이션(3)"
description: "Express에 Socket.io 연결하기"
category: blog
tags: react
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## Server

이번 포스팅에서는 Socket.io를 적용해서 채팅을 주고받는 방법을 알아볼 겁니다      
Express 서버를 생성하기 위한 프로젝트 폴더를 하나 만들고 터미널을 열어 아래의 명령어를 입력해 줍니다.  

```shell
npm init -y
npm install express socket.io
npm install nodemon --save-dev
```

> Express 서버 구동은 [이전 포스팅](https://mujaen.github.io/blog/2021/01/05/nodejs-express-mongodb-mongoose.html){: target="_blank"}을 참고해 주세요!

### Package.json

Package.json 파일을 열고 nodemon 옵션으로 스크립트를 하나 추가합니다  

```
{
  "scripts": {
    "start": "nodemon server"
  },
  "dependencies": {
    "express": "^4.17.1",
    "socket.io": "^3.1.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  }
}

```

```shell
npm start
```

### Server.js

서버를 구동시켜 줄 메인 스크립트 파일을 만들 겁니다. 루트 경로에 server.js를 만들고 아래와 같이 설정합니다.    
기본적으로 서버는 3000번 포트를 사용하지만 원할 시 변경할 수 있습니다.

```javascript
const express = require('express');
const socketio = require('socket.io');
const http = require('http');
const app = express();
const server = http.createServer(app);
const io = socketio(server, {cors: {origin: '*'}});

//Socket io
io.on('connection', socket => {
    socket.on('send message', (item) => {
        socket.emit('receive message', { message: item.message });
    });

    socket.on('disconnect', () => {
        console.log('user disconnected');
    });
});

//서버 연결
server.listen(3000, () => {
    console.log('Express server has started on port 3000');
});
```

## Socket.io

메인 스크립트를 한번 쭉 살펴보죠 Express의 Server(3000포트)를 구동시킨 뒤 Socket.io로    
수신을 하기 위해 아래와 같이 지정해 줍니다

```javascript
const io = socketio(server, {cors: {origin: '*'}});
```

> 'Access-Control-Allow-Origin'오류 발생을 막기 위해 cors 옵션을 지정해야 합니다   

### events

클라이언트에서 서버로 연결이 되면 자동으로 'connection'이 되면서 이벤트 함수가 실행됩니다     
반대로 클라이언트에서 연결이 끊어지거나 강제로 종료하게 된다면 'disconnect' 이벤트가 실행됩니다 

```javascript
io.on('connection', socket => {
    socket.on('send message', (item) => {
        socket.emit('receive message', { message: item.message });
    });

    socket.on('disconnect', () => {
        console.log('user disconnected');
    });
});
```

* emit: 지정한 이벤트를 내보낼 때 사용합니다
* on: 내보낸 이벤트를 받아서 실행할 때 사용합니다

> 자세한 내용은 [공식문서](https://socket.io/docs/v3/emitting-events/){: target="_blank"}를 참고해 주세요


