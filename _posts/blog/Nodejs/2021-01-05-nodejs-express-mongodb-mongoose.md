---
layout: post
title: "Node.js(Express)로 MongoDB(Mongoose) 연결하기(1)"
description: "Node.js + MongoDB 프로젝트 생성"
category: blog
tags: nodejs
---


* this unordered seed list will be replaced by the toc
{:toc}

## Express

서버 구동을 위한 프레임워크로 자바에는 Spring이 있죠 Node.js는 Express를 사용합니다!    
이번 포스팅에서는 Express를 이용하여 서버를 구동시켜 보고 라우팅을 통해  
간단하게 페이지를 보여주는 예제를 다뤄보겠습니다.

### Install express

프로젝트 생성을 위해 폴더명을 express로 하나 만들어 줍니다.  
[Node.js](https://nodejs.org/ko/download){: target="_blank"}와 npm은 미리 설치되어 있다는 전제 하에 진행하도록 하겠습니다  
터미널을 열고 아래의 명령어를 입력하여 express와 nodemon을 설치! 

```shell
npm install express
npm install nodemon --save-dev
```

> 'nodemon'은 파일 변경 감지 시 자동으로 서버를 재시작 시켜주는 편리한 도구입니다

### Package.json

package.json을 열어 제대로 설치가 완료되었는지 확인해보세요.   
'scripts'에 start를 지정해 nodemon을 사용하여 app을 동작시키도록 할 겁니다 

```json
{
  "name": "express-mongoose",
  "main": "app.js",
  "scripts": {
    "start": "nodemon app"
  },
  "author": "Jinjer",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  }
}
```

```shell
npm start
```


### App.js

이제 서버를 구동시켜 줄 메인 스크립트 파일을 만들 겁니다. 루트 경로에 app.js를 만들고 아래와 같이 설정합니다.  
기본적으로 서버는 3000번 포트를 사용하지만 원할 시 변경할 수 있습니다.

```javascript
const express = require('express');
const app = express();
const router = require('./routes/app');

//express 설정
app.set('views', __dirname + '/views');
app.set('view engine', 'pug');
app.use('/', router);

//서버 연결
const server = app.listen(3000, () => {
    console.log('Express server has started on port 3000');
});

```

### Pug

http://localhost:3000에서 확인해보니 Cannot GET / 이라는 문구와 함께 아무것도 노출되지 않습니다..    
index가 없기 때문이죠! 그래서 우리는 pug라는 템플릿 엔진을 사용할 겁니다 물론 pug도 설치를 해줘야 합니다  

```shell
npm install pug
``` 

pug는 Node Express에서 사용되는 템플릿 엔진입니다 /views 폴더에 두고 사용하는 것으로 하겠습니다   
pug의 장점이라면 html 문법보다 마크업이 단순하고 짧아 깔끔하고 가독성이 좋습니다 들여쓰기 한번으로  
자식 엘리먼트를 추가할 수 있죠 또한 동적으로 데이터를 표현하는데도 유용하게 쓰입니다.  
그럼 생성한 views 폴더에 index.pug 파일을 하나 만들어 보도록 하죠  

```pug
doctype html
html
    head
		title 보셨죠? Pug는 이렇게나 마크업이 단순합니다.
	body
                main 
                        p Hello World!
``` 

> 잠깐! pug는 탭과 스페이스 둘 중 하나만 사용하여 들여쓰기 해야 오류가 발생하지 않습니다.

### Router

드디어 마지막 단계입니다! 이제 작성된 페이지를 보여줄 일만 남았죠  
루트 경로에 routes 폴더를 만들고 app.js 파일을 하나 만듭니다.

```javascript
const express = require('express');
const app = express.Router();

app.get('/', (req, res) => {
    res.render('index', {

    });
});

module.exports = app;
```

라우팅이란 URL을 사용하여 클라이언트에 요청하고 응답받는 방식을 일컫습니다.  
위의 예제를 한번 같이 보죠 app으로 정의된 라우팅은 '/' 루트인 URL을 get으로 받아    
response 즉, 응답을 할건데 무엇을? index라는 템플릿을! 페이지에 렌더링 하여 보여주겠다는 내용입니다.   

> 자세한 내용은 [Express 라우팅가이드](https://expressjs.com/ko/guide/routing.html){: target="_blank"}를 참조해 주세요

![page](/assets/img/2021-01-05/page.png)


### Folder Structure

전체 폴더 구조입니다. 궁금한 내용은 아래 댓글로 남겨주세요!   
다음 포스팅에서는 Mongoose로 데이터베이스를 연결하는 내용을 진행하도록 하겠습니다.

![FolderStructure](/assets/img/2021-01-05/folder.png)