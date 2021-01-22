---
layout: post
title: "블로킹(Blocking) 함수와 논블로킹(Non-blocking) 함수"
description: "자바스크립로 논블록 for문 만들기"
category: blog
tags: javascript
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## Non-Blocking

블로킹 : 함수의 수행결과가 끝날 때까지 제어권을 가지고 있는 것  
논블로킹 : 함수가 호출되었을 때 제어권을 바로 반납하며, 자신을 호출한 쪽에서 다른 일을 할 수 있도록 하는 것

```javascript
function foo(){
    console.log("2");
}

setTimeout(foo, 3000);
console.log("1");
```

위의 코드의 결과는 '1' 다음으로 '2' 순서대로 출력이 되는데요      
setTimeout 함수를 호출할 때 3초 뒤에 'foo' 함수가 실행하기 때문에 제어권을 바로 반납하므로               
다음 코드인 console.log("1")이 수행됩니다.    
이와 같이 제어권을 바로 반납하는 방식을 Non-Blocking 방식이라 합니다 
 

### 일반적인 for문 

```javascript
const count = (i) => {console.log(i)};
for(let i = 0; i < 10000; i++) count(i);
```

위의 코드는 우리가 일반적으로 사용하는 for문이죠   
설정한 조건만큼 'count' 함수를 10000번 반복시키는 단순한 코드입니다  
      
그러나 이때 'count' 함수 안에서 또 다른 for문이 있다고 가정한다면? 더나아가 그 for문안에       
또 다른 반복문이나 조건문이 존재한다고 하면 현재 조건이 절대 단순하다고 생각되지 않을 것 입니다     
이와 같이 Blocking 함수는 함수 자신이 제어권을 계속해서 가지고 있기 때문에 다른 함수의 실행을 지연시킬 수 있고       
그로 인해 rendering - check queue - callback queue - run JS 전체에 영향을 줄 수 있다. 

그렇다면 이런 블록킹 함수를 처리할 때 이벤트 루프에서 100번 정도로 나눠서 조금만 실행하고 패스 한다면 어떨까요?        
랜더링도 하고 큐체크도 하고 실행도 하면서 다시 함수를 이어서 실행한다면 해결되지 않을까 해서 고안한 것이 바로 논블록이죠 

### 논블록 for문

```javascript
const count = () => {};

const nbFor = (max, load, block) => {
  	let i = 0;
  	const f = time => {
  	  	let curr = load;
  	  	while(curr-- && i < max) {
  	  	   	block();
  	  	   	i++;
  	  	}
  	  	if(i < max - 1) requestAnimationFrame(f);
  	};
  	requestAnimationFrame(f);
}; 

nbFor(10000, 100, count);
```

'nbFor' 함수를 보면 max, load, block 이라는 3개의 인자를 가지고 있습니다    
1. max   : max로드가 얼마나 걸리도록 할 것인지    
1. load  : 한 번에 로드를 얼마만큼 처리하고 패싱할 것인지
1. block : 그다음에 어떤 block을 실행할 것인지 


'nbFor' 함수가 실행이 되면 변수로 설정한 i 는 계속해서 증가 하지만 100으로 넘긴 curr 값이 후위 감소 연산 되어      
load 만큼만 실행하게 됩니다 마지막으로 if(i < max - 1) requestAnimationFrame(f); 조건을 주어    
계속해서 루프를 실행하게 만들고 i 값이 조건인 max 보다 크면 비로소 멈추게 되는거죠   

> 자바스크립트 내장함수인 [requestAnimationFrame()](https://developer.mozilla.org/ko/docs/Web/API/Window/requestAnimationFrame){: target="_block"}을 참고해 주세요.
 

