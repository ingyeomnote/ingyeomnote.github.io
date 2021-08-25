---
title: "JavaScript - prototype 2"
excerpt: "JavaScript : 프로토타입(prototype)의 이해 2"

toc: true
toc_sticky: true

categories:
  - JavaScript	
tags: 
  - JavaScript
  - Prototype

last_modified_at: 2021-08-22T21:25:00

---

JavaScript는 객체지향언어이다. 하지만 JavaScript에는 클래스라는 개념이 없고 대신 Prototype이라는 것이 존재한다. 이것이 JavaScript가 Prototype 기반 언어라고 불리는 이유이다.

저번 포스팅에서도 프로토타입에 대해 올렸지만 쉽게 이해가 가지 않았기 때문에 다른 분들이 올린 글을 보며 공부를 하다 잘 정리되어 있는 글을 발견해서 같이 보면 좋을 거 같아 다시 한 번 작성합니다.

---



# JavaScript

 JavaScript는 객체지향언어이다. 하지만 JavaScript에는 클래스라는 개념이 없고 대신 Prototype이라는 것이 존재한다. 이것이 JavaScript가 Prototype 기반 언어라고 불리는 이유이다.



### Prototype이 언제 쓰일까?

Javascript에 클래스는 없지만 함수(function)와 new를 통해 클래스를 비스무리하게 흉내낼 수는 있다.

```js
function Person(){
    this.eyes = 2;
    this.noes = 1;
}

var kim = new Person();
var park = new Person();

console.log(kim.eyes); // 2
console.log(kim.nose); // 1

console.log(park.eyes); // 2
console.log(park.nose); // 1
```

kim과 park은 eyes와 nose를 공통적으로 가지고 있는데, 메모리에는 eyes와 nose가 두 개씩 총 4개가 할당된다. 객체를 100개 만들면 200개의 변수가 메모리에 할당되는데 바로 이런 문제를 프로토타입으로 해결할 수 있다.



```js
function Person(){}

Person.prototype.eyes = 2;
Person.prototype.nose = 1;

var kim = new Person();
var park = new Person();

console.log(kim.eyes); // 2
```

위 소스를 간단히 설명하면 Person.prototype이라는 빈 Object가 어딘가에 존재하고, Person함수로부터 생성된 객체 (kim, park)들은 어딘가에 존재하는 Object에 들어있는 값을 모두 갖다 쓸 수 있다.

즉, eyes와 nose를 어딘가에 있는 빈 공간에 넣어놓고 kim과 park이 공유해서 사용하는 것이다.



### Prototype Link와 Prototype Object

JavaScript에는 **Prototype Link**와 **Prototype Object**라는 것이 존재한다. 그리고 이 둘을 통틀어 **Prototype**이라 부른다.

**Prototype Object**

객체는 언제나 함수(Function)로 생성된다.

```js
function person(){} // 함수

var personObject = new Person(); // 함수로 객체를 생성
```

personObject 객체는 Person이라는 함수로 생성된 객체이다. 이렇듯 언제나 객체는 함수에서 시작된다. 우리가 많이 쓰는 일반적인 객체 생성도 예외는 아니다.

```js
var obj = {};
```

위의 소스는 함수와 전혀 상관없는 코드로 보이지만 위 코드는 사실 다음 코드와 같다.

```js
var obj = new Object();
```

위 코드에서 Object가 JavaScript에서 기본적으로 제공하는 함수이다.

<p align="center"><img src="https://miro.medium.com/max/289/1*AJIDIoBFrGtUb8Nv-IonQg.png"></p>

Object와 마찬가지로 Function, Array도 모두 함수로 정의되어 있다. 이것이 첫 번째 포인트이다.

그렇다면 이것에 Prototype Object랑 무슨 상관이 있느냐? **함수가 정의될 때는 2가지 일이 동시에 이루어진다.**



**1. 해당 함수에 Constructor(생성자) 자격 부여**

Constructor 자격이 부여되면 new를 통해 객체를 만들어 낼 수 있게 된다. 이것이 함수만 new 키워드를 사용할 수 있는 이유이다.

<p align="center"><img src="https://miro.medium.com/max/386/1*rADwBTPKeORv_Qf-lhbFRA.png"></p>



**2. 해당 함수의 Prototype Object 생성 및 연결**

함수를 정의하면 함수만 생성되는 것이 아니라 Prototype Object도 같이 생성된다.

 <p align="center"><img src="https://miro.medium.com/max/700/1*PZe_YnLftVZwT1dNs1Iu0A.png" ></p>

그리고 생성된 prototype이라는 속성을 통해 Person Object에 접근할 수 있다. Prototype Object는 일반적인 객체와 같으며 기본적인 속성으로 **constructor**와 **\__proto__**를 가지고 있다.

 <p align="center"><img src="https://miro.medium.com/max/307/1*NpSb7ha6lMdZpc8hFvBl2g.png" ></p>
**constructor**는 **Prototype Object**와 같이 생성되었던 함수를 가리키고 있다.

**\__proto__**는 **Prototype Link**이다. 밑에서 자세히 설명

이제 위에서 kim과 park이 나왔던 예제를 다시 보겠다.

```js
function Person(){}

Person.prototype.eyes = 2;
Person.prototype.nose = 1;

var kim = new Person();
var park = new Person();

console.log(kim.eyes); // 2
...
```

![그림5](https://miro.medium.com/max/294/1*PLRkoBdVZv9vZW1Z4FlLJw.png)

**Prototype Object**는 일반적인 객체이므로 석성을 마음대로 추가/삭제 할 수 있다. **kim**과 **park**은 Person 함수를 통해 생성되었으니 Person.prototype을 참조할 수 있게 된다.

**Prototype Link**를 보기 전에 **Prototype Object**를 어느정도 이해하고 보는 것이 좋다. 함수가 정의될 때 이루어지는 일들을 이해하는 것이 두 번째 포인트, **Prototype Object**를 이해하는 것이  세 번째 포인트이다.



**Prototype Link**

 <p align="center"><img src="https://miro.medium.com/max/226/1*TPkfy4eqiHHpWDvEOjfQCg.png" ></p>

kim에는 eyes라는 속성이 없는데도 kim.eyes를 실행하면 2라는 값을 참조하는 것을 볼 수 있다. 위에서 설명했듯이 **Prototype Object**에 존재하는 eyes 속성을 참조한 것인데, 이게 어떻게 가능한 걸까?

바로 kim이 가지고 있는 딱 하나의 속성 **\__proto__**가 그것을 가능하게 해주는 열쇠이다.

**prototype**속성은 함수만 가지고 있던 것과는 달리(Person.prototype) **\__proto__**속성은 모든 객체가 빠짐없이 가지고 있는 속성이다.

**\__proto__는 객체가 생성될 때 조상이었던 함수의 Prototype Object를 가리킨다.** kim 객체는 Person함수로부터 생성되었으니 Person 함수의 Prototype Object를 가리키고 있는 것이다.

 <p align="center"><img src="https://miro.medium.com/max/270/1*4V9q1tS5GWLU4sMkhOVNEg.png" ></p>

\__proto__를 까보니 역시 Person 함수의 Prototype Object를 가리키고 있었다.

<p align="center"><img src="https://miro.medium.com/max/700/1*jMTxqTYDZGhykJQoimmb0A.png" ></p>



**kim**객체가 **eyes**를 직접 가지고 있지 않기 때문에 **eyes** 속성을 찾을 때까지 상위 프로토타입을 탐색한다. 최상위인 **Object**의 **Prototype Object** 까지 도달했는데도 못찾을 경우 **undefined**를 리턴한다. 이렇게 **\__proto__** 속성을 통해 상위 프로토타입과 연결되어 있는 형태를 **프로토타입 체인(Chain)**이라고 한다.

<p align="center"><img src="https://miro.medium.com/max/700/1*mwPfPuTeiQiGoPmcAXB-Kg.png" ></p>

위의 그림이 프로토타입 체인을 나타낸 것이며 최상위 객체는 **Object**이다. 이런 프로토타입 체인 구조 때문에 모든 객체는 **Object의 자식**이라고 불리고, **Object Prototype Object**에 있는 모든 속성을 사용 할 수 있다. 한 가지 예를 들면 **toString** 함수가 있다.

<p align="center"><img src="https://miro.medium.com/max/395/1*VW4PFea8x7LQiHp3PI8Hrg.png" ></p>



[출처]: https://medium.com/@bluesh55/javascript-prototype-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-f8e67c286b67	"프로토타입의 이해"

