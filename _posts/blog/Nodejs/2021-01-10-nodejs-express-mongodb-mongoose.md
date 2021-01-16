---
layout: post
title: "Mongoose(Express)로 MongoDB 데이터 추가하기(1)"
description: "Node.js + Mongoose 데이터 추가하기"
category: blog
tags: nodejs
---


* this unordered seed list will be replaced by the toc
{:toc}

## Model

지난 포스팅에서 Mongoose의 장점을 Express에 스키마와 모델을 적용할 수 있다고 했죠    
스키마(schema)는 데이터베이스 문서의 값에 대해 타입을 정의해 주는 것이고 모델은 이러한 스키마를   
인스턴스로 구성합니다. 즉, 객체로 인식시켜 DB의 값을 조회하고 업데이트할 수 있는 것이죠!    
이번 포스팅에서는 예제를 통해 DB에 데이터를 추가 하는 방법을 알아볼 겁니다     


### Defining Schema
 
루트 경로에 models로 폴더를 하나 생성하고 user.js 파일을 생성합니다   
먼저, User 스키마를 하나 만들어 정의해 주세요 User를 추가할 때마다 result 배열에 객체로 담을 겁니다

```javascript
const mongoose = require("mongoose");
const schema = mongoose.Schema;

const UserSchema = new schema ({
    result: [
        {
            name: {
                type: String,
                required: true
            },
            age: {
                type: Number,
                required: true
            }
        }
    ]
}, {collection: 'user'});

module.exports = mongoose.model("User", UserSchema);
```

> 스키마에 정의할 때는 객체의 key 값에 실제 DB 문서에 들어갈 타입을 지정할 수 있습니다  
> 이때, collection 이름을 옵션으로 지정하지 않는다면 collection 이름 뒤에 s가 자동으로 추가되니 지정하는 것이 좋습니다 

### Creating a Model

생성한 'UserSchema'를 'User'로 지정하여 모델을 하나 만들어 내보냅니다.   
이 모델을 가지고 함수를 사용해서 문서를 데이터베이스에 저장하고 조회할 수 있는데요      
아래 예제와 같이 findOne 함수를 사용하여 원하는 객체에 대한 결과를 조회할 수 있습니다.   
자세한 내용은 [Mongoose Model](https://mongoosejs.com/docs/api/model.html) 공식 문서를 참고하세요!

```javascript
// callback
User.findOne({ name: 'Jinjer', age: 32 }, (error, user) => {
    
});

// exec
User.findOne({ name: 'Jinjer', age: 32 }).exec()
    .then(user => {
        
    })
    .catch(error => console.log(error));
```
 
> 현재 Mongoose는 4버전 이후부터 쿼리에서 then을 지원하고 있습니다 이전에는 프로미스를 사용하기 위해 exec()를  
> 반드시 붙여 사용해야 했는데요 최신 버전은 그럴 필요는 없지만 그래도 붙여주시는 것을 추천합니다. 

## View

모델을 만들었으니 이제 유저를 등록할 페이지를 만들고 라우팅을 해보죠  
Pug와 Router 설정은 [지난 포스팅](https://mujaen.github.io/blog/2021/01/05/nodejs-express-mongodb-mongoose.html){: target="_blank"}을 참고해 주세요!


### Routes

루트 경로에 routes 폴더를 하나 만들고 user.js 파일을 생성합니다.  
get으로 유저를 등록하는 페이지를 render 해서 보여주고  
post로 등록페이지에서 form의 action에 해당되는 경로로 데이터를 보내줄 겁니다      

```javascript
const express = require('express');
const app = express.Router();
const UserController = require('../controllers/user');

app.get('/user/register', UserController.listUser);

app.post('/api/user/list', UserController.registerUser);

module.exports = app;
```



### Pug

루트 경로에 views 폴더를 하나 만들고 그 안에 user 폴더를 하나 더 만들겠습니다   
그 후에 생성한 user 폴더에 register.pug 파일을 하나 만들어 보도록 하죠  

```pug
doctype html
html
	head
		title 유저 등록
	body
		form(method="post", action="/api/user/list")
                        input(type="text" name="name" value="")
                        input(type="text" name="age" value="")
                        button(type="submit") 저장
```

서버를 구동시켜 봅니다 페이지가 정상적으로 보이시나요?  
name과 age를 입력하여 form 이벤트로 데이터를 전송할 겁니다

```shell
npm start
``` 


## Controller

루트 경로에 controllers 폴더를 하나 생성하고 user.js 파일을 생성합니다  
require로 불러온 'User'모델을 사용자를 추가하는 함수를 만들어서 내보낼 겁니다.

```javascript
const User = require('../models/user');

exports.listUser = async (req, res) => {
    res.render('user/register', {

    });
};

exports.registerUser = async (req, res) => {
    const user = new User({
        result: []
    });
    
    let data = {
        name: req.body.name,
        age: req.body.age
    };

    user.result.push(data);
    
    User.create({result: user.result})
        .then((result) => {
            res.redirect("/user/register");
        }).catch((error) => {
            console.log(error);
    });
};
```

result 배열에 생성한 data 객체를 담아서 create 함수로 전송한 뒤 redirect로 다시 페이지로 돌아올 겁니다  
그러나 전송을 해보니 undefined 에러가 발생합니다.. 

### body-parser

create함수를 사용하여 json형태로 서버로 값을 전송했는데 서버에서 값을 못찾아서 undefined 에러가   
출력된다면 req.body 프로퍼티에 접근을 못해 원하는 데이터를 얻지 못하는 문제입니다   
해결방법은 body-parser를 설치해서 사용해야 합니다 터미널을 열어 아래의 명령어를 입력해 줍니다.

```shell
npm install body-parser
```

설치가 완료되면 메인 스크립트 파일인 app.js을 열어 bodyParser를 추가합니다     
4.16버전 이후로는 body-parser가 express에 포함되어 따로 설치하지 않아도 됩니다!

```javascript
const express = require('express');
const app = express();

// Previous ver.4.16
const bodyParser = require('body-parser');

app.use(bodyParser.urlencoded({extended: false}));
app.use(bodyParser.json());

// After ver.4.16
app.use(express.urlencoded({extended: false}));
app.use(express.json());
```

> body-parser의 urlencoded extended 옵션은 객체 안에 객체를 허용할 것(true)인지 아닌지(false)를 정합니다

### MongoDB Compass

데이터 전송에 성공했다면 DB에서 확인을 해봐야겠죠? IDE라면 Database를 열어 확인하면 되겠지만         
이번 포스팅에서는 [MongoDB Compass](https://www.mongodb.com/products/compass){: target="_blank"}를 설치해서 확인해 볼 겁니다  
설치가 완료되어 실행을 시키면 DB와 연결할 수 있도록 계정 가입 시 복사했던 코드를 넣어줄 겁니다.   

![FolderStructure](/assets/img/2021-01-10/connection.png)

> MongoDB 계정을 만들고 서버에 연결하는 내용은 [지난 포스팅](https://mujaen.github.io/blog/2021/01/08/nodejs-express-mongodb-mongoose.html){: target="_blank"}을 참고해 주세요!

연결이 되었다면 Compass에서 데이터베이스와 컬렉션을 편집할 수 있습니다.   
메인스크립트에 설정한 DB 주소로 전송된 데이터가 추가되어 있는 것을 확인할 수 있습니다!  
이때, ObjectId는 각 도큐먼트를 식별하기 위한 식별자로 자동 생성됩니다.

![FolderStructure](/assets/img/2021-01-10/user.png)


### Folder Structure

전체 폴더 구조입니다. 궁금한 내용은 아래 댓글로 남겨주세요!   
다음 포스팅에서는 DB에 추가된 데이터를 업데이트 하는 내용을 진행하도록 하겠습니다.

![FolderStructure](/assets/img/2021-01-10/folder.png)