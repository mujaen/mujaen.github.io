---
layout: post
title: "MongoDB + Mongoose 데이터 조작(1)"
description: "Node.js + Mongoose 데이터 CRUD"
category: blog
tags: nodejs
---


* this unordered seed list will be replaced by the toc
{:toc}

## Models

지난 포스팅에서 Mongoose의 장점을 Express에 스키마와 모델을 적용할 수 있다고 했죠    
스키마(schema)는 데이터베이스 문서의 값에 대해 타입을 정의해 주는 것이고 모델은 이러한 스키마를   
인스턴스로 구성합니다. 즉, 객체로 인식시켜 DB의 값을 조회하고 업데이트할 수 있는 것이죠!    
이번 포스팅에서는 지역명을 추가하고 업데이트하는 예제를 통해 데이터를 CRUD 하는 방법을 알아볼 겁니다     


### Schema
 
루트 경로에 models로 폴더를 하나 생성하고 local.js 파일을 생성합니다  
먼저, 지역 스키마를 하나 만들어 주세요 지역을 추가할 때마다 배열에 객체로 담을 겁니다

```javascript
const mongoose = require("mongoose");
const schema = mongoose.Schema;

const LocalSchema = new schema ({
    result: [
        {
            area: {
                type: String,
                required: true
            },
            status: {
                type: String,
                required: true
            }
        }
    ]
});

module.exports = mongoose.model("Local", LocalSchema);
```

### Model

생성한 'LocalSchema'를 'Local'로 지정하여 모델을 하나 만들어 내보냅니다. 




## Controllers



![Check Console Message](/assets/img/2021-01-08/terminal.png)