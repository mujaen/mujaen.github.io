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
맛이 전부 다르고 때때로 큰 실수도 저질렀습니다 손님들은 다신 그 매장을 방문하지 않았습니다   
그에 반해 타입스크립트는 정해진 레시피대로 만들기 때문에 실수가 없고 완벽한 맛?과 코드를 유지할 수 있었죠

### 컴파일 타임
         
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

undefined 에러가 발생합니다! 하지만 타입스크립트는 이런 오류를 컴파일 타임에서 발견하고 컴파일 자체가 안되도록 막아버리죠            
더불어, 아래와 같이 옵셔널체이닝인 ?를 추가해주면 인자인 gender가 null 혹은 undefined 일 시, undefined를 반환합니다     
  
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

### 에러

타입선언 오류는 가장 흔한 오류입니다 이 역시 컴파일 타임에서 에러를 발생시키죠           
그렇다면 먼저, 변수에 선언된 타입과 다른 유형의 값을 선언했을 때 생기는 오류를 알아 봅시다.  

```typescript
let hour: string = 'eleven';
hour = 11;
``` 

변수 hour에 문자열 타입을 선언하고 'eleven'이라고 값을 지정 하였습니다    
그 뒤에 '11'로 값을 바꿔 준다면 타입스크립트는 어떻게 반응할까요   

```shell
Error:(2, 1) TS2322: Type '11' is not assignable to type 'string'.
``` 

"TS2322: '11'은 문자열 유형에 할당할 수 없다" 라고 정확히 해당 오류를 명시해 줍니다

```typescript
let hour = 'eleven';
hour = 11;
``` 

변수에 타입을 선언하지 않았을 때도 미리 지정한 유형과 다르다면 같은 오류를 내뱉어 주는 것을 확인할 수 있죠

```shell
Error:(2, 1) TS2322: Type '11' is not assignable to type 'string'.
``` 

이처럼 타입스크립트로 프로젝트를 진행하다 보면 생각 못 했던 부분에서 에러가 자주 발생됩니다      
반대로 우리가 생각하지 못한 부분까지 타입스크립트가 보완해 준다는 의미겠죠? 
 

