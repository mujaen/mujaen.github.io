---
layout: post
title: "이터레이터(iterator)와 제너레이터(Generator) 사용하기"
description: "이터러블과 제너레이터 살펴보기"
category: blog
tags: javascript
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## iterables

이터러블에 있는 메소드를 호출하면 이터레이터를 얻을 수 있는데요 결과적으로 이터러블은 이터레이터를 받기 위해서 쓰는 것이며     
이터러블을 구현하고 있는 것 중에선 대표적으로 String, Array, Set 등이 있습니다.


### 반복자(iterator)

이터레이터는 반복을 위해 설계된 특별한 인터페이스를 가진 객체이며 이터레이터에게 next()라는 키함수를 호출하면 오브젝트를    
리턴하게 하는 인터페이스를 정의하고 있습니다!

```javascript
const arr = [0, 1, 2, 3, 4, 5];

function fromArray(arr) {
    var i = -1;
    var length = arr.length;

    return {
        next: function () {
            return ++i >= length
                ? { done: true }
                : { value: arr[i], done: false };
        }
    }
}

function oddCoEs5(arr) {
    return {
        next: function () {
            var cur = arr.next();

            console.log(`oddCoEs5 ${cur.value}`);

            return cur.done
                ? { done: true }
                : cur.value % 2
                    ? { value: cur.value, done: false }
                    : this.next();
        }
    }
}

const oddCoEs5Next = oddCoEs5(fromArray(arr));
let oddCoEs5Value = oddCoEs5Next.next();
console.log(oddCoEs5Value.value);
oddCoEs5Value = oddCoEs5Next.next();
console.log(oddCoEs5Value.value);
oddCoEs5Value = oddCoEs5Next.next();
console.log(oddCoEs5Value.value);
```

위의 코드를 보면 0-5까지 값을 가진 arr 배열이 존재하고  
fromArray는 받은 arr의 length만큼 루프를 돌려 값을 다시 반환하죠   
oddCoEs5는 받은 fromArray값을 next() 키함수를 사용해 다시 반환하는데     
이때 value와 done이라는 오브젝트가 담긴 키를 리턴하게되고 done의 true와 false를 이용하여 조건을 나누고   
나머지 값이 존재한다면 value에 값이 담기고 아니라면 계속해서 next()로 반복하게 됩니다.

하지만 잘 만들어진 Itorator는 유용한 도구인 반면, 이것을 생성할 때는 주의해서 프로그래밍을 해야 하는데요      
반복자 내부에 명시적으로 상태를 유지할 필요가 있기 때문이죠 Generator는 이에 대한 강력한 대안을 제공합니다.

## Generator

Generator는 iterator를 반환하는 함수이며 function 옆에 별표 *를 사용하여 작성되는데요       
이때 생성자 함수는 yield 키워드를 만날 때까지 실행됩니다  

```javascript
const createIterator = function *(arr) {
    for (let i = 0; i < arr.length; i++) {
        yield arr[i];
    }
};

let iterator = createIterator([1, 2, 3]);

console.log(iterator.next()); //{value: 1, done: false}
console.log(iterator.next()); //{value: 2, done: false}
console.log(iterator.next()); //{value: 3, done: false}
console.log(iterator.next()); //{value: undefined, done: true}
```

위의 코드는 arr 라는 배열을 createIterator 함수에 전달하게 되고 루프가 실행되면서    
Iterator 요소를 생성하는데요 이때 yield를 만나게 되고 루프는 멈추가 됩니다  

> 예제와 같이 표현식으로 만들 수 있으나 Arrow 함수를 이용해 Generator를 만들 순 없는 것 같네요  

  
    
   
          
