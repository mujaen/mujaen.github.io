---
layout: post
title: "Mongoose(Express)로 MongoDB 데이터 추가하기(2)"
description: "Node.js + Mongoose 데이터 업데이트하기"
category: blog
tags: nodejs
---


* this unordered seed list will be replaced by the toc
{:toc}

## Model

지난 포스팅에서는 Mongoose의 create함수를 이용하여 DB에 데이터를 추가 해봤는데요  
이번에는 DB에 하나씩 추가되는게 아닌 아래의 그림과 같이 타입별로 데이터가 추가되고  
여러개의 데이터를 result 배열에 담아서 DB에 저장하는 예제를 통해 데이터 업데이트 방법을 알아볼 겁니다   
 
![FolderStructure](/assets/img/2021-01-13/user.png)

### Defining Schema
 
먼저, 스키마를 수정해 보죠 type 으로 유저를 구분할 것이기 때문에 값을 String으로 받는    
type 이라는 key를 하나 추가해서 모델을 하나 만들어 내보냅니다. 

```javascript
const mongoose = require("mongoose");
const schema = mongoose.Schema;

const UserSchema = new schema ({
    type: String,
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

## View

모델을 수정하였다면 지난 포스팅에서 만든 등록페이지와 라우팅을 수정합니다.   
Pug와 Router 설정은 [지난 포스팅](https://mujaen.github.io/blog/2021/01/05/nodejs-express-mongodb-mongoose.html){: target="_blank"}을 참고해 주세요!


### Routes

라우팅은 수정없이 registerUser 그대로 사용하겠습니다 아래와 같이 설정해 주세요.      

```javascript
const express = require('express');
const app = express.Router();
const UserController = require('../controllers/user');

app.get('/user/register', UserController.listUser);

app.post('/api/user/list', UserController.registerUser);

module.exports = app;
```



### Pug

register.pug 파일을 열고 아래와 같이 수정해 주세요.   
등록되는 유저를 select로 Admin과 Guest로 구분할 겁니다

```pug
doctype html
html
	head
		title 유저 등록
	body
		form(method="post", action="/api/user/list")
                        select(name="type" id="")
                                option(value="admin" selected) Admin
                                option(value="guest") Guest
                        input(type="text" name="name" value="")
                        input(type="text" name="age" value="")
                        button(type="submit") 저장
```

## Controller

이제 제일 중요한 컨트롤러를 수정해보죠. controllers 폴더의 user.js 파일을 열고 아래와 같이 수정합니다          
registerUser를 같이 보죠. 등록페이지의 form에서 submit 이벤트가 발생하면 req.body를 사용해서     
request로 type의 값을 받아올 겁니다.  

```javascript
const User = require('../models/user');

exports.listUser = async (req, res) => {
    res.render('user/register', {

    });
};

exports.registerUser = async (req, res) => {
    const user = new User({
        type: req.body.type,
        result: []
    });

    if(typeof req.body.name == 'object') {
        for (let i = 0; i < req.body.name.length; i++) {
            user.result.push({
                name: req.body.name[i],
                age: req.body.age[i]
            });
        }
    } else {
        user.result.push({
            name: req.body.name,
            age: req.body.age
        });
    }

    try {
        User.updateOne({type: req.body.type}, {$set: {result: user.result}}, {upsert: true})
            .then((result) => {
                res.redirect("/user/register");
            }).catch((error) => {
            console.log(error);
        });
    } catch (error) {
        console.log(error);
    }
};
```
updateOne 함수로 DB에 데이터를 업데이트 해보죠 try와 catch를 이용합니다  
첫 번째 인자는 데이터를 filter 할 수 있도록 하는데요 type을 가지고 구분할 것이므로 type 값을 넘겨줍니다  

> upsert 옵션은 업데이트할 시 DB에 데이터가 없는 경우 자동으로 데이터를 insert 해줍니다  
> 이때, 변경하고 싶은 데이터를 $set으로 지정해 주지 않으면 해당 데이터를 제외한 다른 데이터는 모두 지워집니다  

  


### Folder Structure

전체 폴더 구조입니다. 궁금한 내용은 아래 댓글로 남겨주세요!   
다음 포스팅에서는 DB에 추가된 데이터를 업데이트 하는 내용을 진행하도록 하겠습니다.

![FolderStructure](/assets/img/2021-01-10/folder.png)