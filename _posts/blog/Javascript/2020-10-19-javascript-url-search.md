---
layout: post
title: "URL 인터페이스의 Search 속성 알아보기"
description: "자바스크립트에서 URL searchParams 사용하기"
category: blog
tags: javascript
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## URL

URL은 지정한 URL에 대해 구문 분석, 구성, 정규화 및 인코딩 하는데 사용되고 매개 변수를 사용하여   
인터페이스를 사용할 수 있으며 URL() 와 같이 지정된 파라미터로 구성된 URL 객체를 만들고 돌려 줍니다.
이번 포스팅에서는 URL에서 자주 사용되는 몇가지 Properties를 자바스크립트 예제를 통해 알아보죠  

### searchParams

```shell
http://localhost:90/test.html?seq=4
```

현재 URL에 파라미터가 포함되어 있는지 판별할 때 'has'를 이용할 수 있습니다

```javascript
const url = new URLSearchParams(location.search);

if(url.has('seq')) console.log('seq 발견!');
```

파라미터가 존재할 때 콘솔 창에서 확인이 되었다면 다음은 'get'을 사용하여 해당 파라미터의 값을 구해보죠

```javascript
console.log(url.get('seq'));
// 4
```

콘솔창에 4가 정확히 찍히는지 확인 할 수 있습니다 그렇다면 파라미터 seq의 값도 변경할 수 있을까요?     
물론 가능합니다. 'set'을 사용하면 값을 변경할 수 있죠!

```javascript
url.set('seq', '2');

console.log(url.get('seq'));
// 2
```

이 외에 추가적인 속성들은 [URLSearchParams 공식문서](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams){: target="_blank""}를 참고해 주세요

 
   
