---
layout: post
title: "Typescript 인터페이스와 클래스 사용하기"
description: "인터페이스와 클래스 차이점"
category: blog
tags: typescript
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 1. Interface

자바스크립트만 사용하던 개발자에겐 인터페이스라는 단어가 생소하게 느껴질 수 있습니다      
하지만 자바나 C# 등 우리가 흔히 알고 있는 정적 타입 언어에서는 익히 사용되고 있는 개념이죠  
   
인터페이스는 객체의 프로퍼티의 타입을 미리 선언하는 것이라 이해하면 됩니다     
자세한 내용과 사용방법은 [Typescript 인터페이스 공식문서](https://www.typescriptlang.org/docs/handbook/interfaces.html){: target="_blank"}에서 확인 할 수 있습니다. 

아래의 코드를 보면 person 객체의 각 key에 맞도록 타입을 명시해 주었고    
person을 인자로 sayHello를 실행할 때 인터페이스인 Human을 사용하여 person의 타입을 체크합니다  


```typescript
interface Human {
  	name: string,
  	age: number,
  	gender: string
}

const person = {
  	name: "JINJER",
  	age: 32,
  	gender: "male"  
};

const sayHello = (person: Human): string => {
  	return `Hello ${person.name}, you are ${person.age}, you are a ${person.gender}`;  
};

sayHello(person);

export {};
``` 

> 인터페이스는 자바스크립트에서 노출이 안됩니다! 그렇기에 변환된 코드를 보면   
> js파일에는 인터페이스가 존재하지 않는 것을 확인할 수 있습니다.


## 2. Class

타입스크립트에서 클래스는 권한을 설정하는 속성을 지정해 줄 수 있습니다.    
public과 private을 사용할 수 있는데 아래의 예제에서 public으로 지정한 name과 gender는 어느 곳에서 나     
사용할 수 있는 반면 private으로 지정한 age는 클래스 내부에서만 사용할 수 있는 것이죠
 
```typescript
class Human {
    public name: string;
    private age: number;
    public gender: string;
    constructor(name: string, age: number,  gender: string) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }
}
``` 

아래의 코드처럼 age를 private으로 지정하면 타입스크립트가 컴파일을 할 수 없습니다       
만약 외부에서 변수의 사용을 원치 않는 경우 이처럼 private 을 주게 되면 실행을 제어할 수 있습니다.   

```typescript
class Human {
    public name: string;
    private age: number;
    public gender: string;
    constructor(name: string, age: number,  gender: string) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }
}

const jinjer = new Human("JINJER" , 32, "male");


const sayHello = (person: Human) : string => {
    return `Hello ${person.name}, you are ${person.age}, you are a ${person.gender}!`;
};

console.log(sayHello(jinjer));

export {};
``` 

> protected는 private과 달리 지정한 속성을 상속은 받되 외부에서 수정은 못합니다!
