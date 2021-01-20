---
layout: post
title: "Typescript의 장점과 단점"
description: "타입스크립트를 써야 하는 이유"
category: blog
tags: typescript
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 장점

타입스크립트의 장점을 쉽게 이해하려면 다음과 같은 상황이라고 가정해보면 어떨까요?  
두 친구가 각각 다른 매장에 바리스타로 취업을 하였는데 자바스크립트는 정해진 레시피가 없어서          
맛이 전부 다르고 때때로 큰 실수도 저질렀습니다 손님들은 다신 그 매장을 방문하지 않았죠.   

그에 반해 타입스크립트는 정해진 레시피대로 만들기 때문에 실수가 없고 완벽한 맛?과 코드를 유지할 수 있었죠         
타입, 클래스, 인터페이스, 제네릭, 데코레이터와 같은 주류 개념을 가지고 있으며 모든 자바스크립트 코드는     
타입스크립트에도 유효하므로 기존 사용하던 코드를 마이그레이션할 수 도 있습니다  
그럼 아래의 예제를 통해 타입스크립트의 어떤 점이 좋은지 알아보죠  
 
```javascript
const name = "JINJER",
	age = 32,
	gender = "male";

const sayHello = (name, age, gender) => {
  	console.log(`Hello ${name}, you are ${age}, you are a ${gender}`);  
};

sayHello(name, age, gender);
``` 

평범한 자바스크립트 코드인데요 문제는 여기서 gender를 사용하지 않았을 때 어떤 일이 발생 할까요? 

```typescript
sayHello(name, age);

//Hello JINJER, you are 32, you are a undefined
``` 

undefined 에러가 발생합니다! 하지만 타입스크립트는 이런 오류를 미리 발견하고 컴파일 자체가 안되도록 막아버립니다          
더불어, 아래와 같이 옵셔널체이닝인 ?를 추가해주면 인자인 gender가 null 혹은 undefined 일 시,    
undefined를 반환합니다     
  
```typescript
const name = "JINJER",
	age = 32,
	gender = "male";

const sayHello = (name, age, gender?) => {
  	console.log(`Hello ${name}, you are ${age}, you are a ${gender}`);  
};

sayHello(name, age);
``` 

함수를 파악할 때 name과 age가 필요하지만 gender는 필요하지 않겠구나 하고 코드를 이해할 수 있겠죠
 
 
## 단점

타입스크립트의 단점이라면 현재까지도 적지 않은 개발자가 다음과 같은 이유로 사용하기를 꺼리고 있는데요       
결국엔 타입스크립트도 컴파일을 하면 자바스크립트로 변환되는데 번거롭게 과정을 늘릴 필요가 있나 하는 의문과   
수 많은 오류로 인해서 타입스크립트를 사용하면서 느끼는 장점보다 오류해결로 인한 시간 소요가 크다는 단점 그리고       
타입 정의 및 추가 작업들을 요구한다는 점이 있겠네요.
 

