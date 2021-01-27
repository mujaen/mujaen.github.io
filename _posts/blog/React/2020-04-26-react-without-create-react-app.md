---
layout: post
title: "React 프로젝트 작업 환경 세팅하기"
description: "create-react-app 사용하지 않고 프로젝트 생성"
category: blog
tags: react
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## Package.json

이번 포스팅에서는 React의 빠른 환경 설정인 Create React App을 사용하지 않고     
직접 리액트 프로젝트를 세팅해 보도록 하죠 먼저 프로젝트 폴더를 하나 만들고  
package.json 생성을 위해 터미널을 열어 아래의 명령어를 입력해 줍니다.

```shell
npm init -y
```

> -y 명령어를 사용하게 되면 package.json 생성 시 나오는 모든 문항에 '예'로 대답한 것과 같습니다   

package.json 파일이 루트 경로에 생성되었다면 npm 명령어로 React를 설치합니다  

```shell
npm install react 
npm install react-dom
```

> ReactDOM에 대한 자세한 설명은 [공식문서](https://ko.reactjs.org/docs/react-dom.html){: target="_blank"}를 참고해 주세요! 

## Babel

ES6문법을 사용하기 위해서 Babel을 사용한 경험이 있으셨다면.. 리액트 문법 또한 Babel의 도움을 받아야 합니다   
Babel을 설치해보죠 터미널을 열어 아래의 명령어를 입력해 줍니다.

```shell
npm install --save-dev @babel/preset-core @babel/preset-env @babel/preset-react
```

> @babel/preset-core: ES6 문법을 ES5 문법으로 변환하기 위해 설치   
> @babel/preset-env: 타깃 환경에 필요한 구문 변환과 브라우저 폴리필을 제공  
> @babel/preset-react: 리액트의 JSX 문법을 자바스크립트로 컴파일하기 위해 설치   

### .babelrc

루트 경로에 .babelrc 파일을 만들어 지역 설정을 합니다 presets과 plugins 옵션을 아래와 같이 지정해 주세요     
번들러에서 자동으로 루트 경로의 .babelrc 파일을 인식하고 적용할 겁니다   

```javascript



```


## Bundler

Webpack 혹은 gulp를 자주 쓰는 편이지만 이번 프로젝트는 [Parcel](https://ko.parceljs.org/){: target="_blank"} 번들러를 사용하여 진행해 보겠습니다    
그럼 번들러를 설치해보죠 터미널을 열어 아래의 명령어를 입력해 줍니다.  

```shell
npm i --save-dev parcel-bundler
```

### index.html

번들러를 설치하였다면 바로 테스트를 해봐야 겠죠?   
루트 경로에 'src' 폴더를 하나 생성하고 index.html 파일을 하나 만들어 주세요         

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>React with Parcel</title>
  <script src="./index.js"></script>
</head>
<body>
    <div id="app"></div>
</body>
</html>
``` 

### app.js

생성한 'src' 폴더에 app.js 파일을 하나 만들어 주세요  
ReactDOM의     

```javascript
import React from "react";
import ReactDOM from "react-dom";

class HelloMessage extends React.Component {
    render() {
        return <div>Hi, my name is {this.props.name}</div>;
    }
}

ReactDOM.render(
    <HelloMessage name="Jinjer" />,
    document.getElementById("app")
);

```


터미널을 열어 아래의 명령어로 번들링한 파일을 서버에 띄워보겠습니다  

```shell
parcel src/index.html
// Server running at http://localhost:1234
```
  
브라우저를 열고 주소창에 http://localhost:1234를 입력하여 정상적으로 페이지가 나온다면 성공입니다  

 



