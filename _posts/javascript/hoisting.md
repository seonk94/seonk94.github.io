---
title:  "Javascript - Hoisting "
search: true
categories: 
  - Javascript
tag:
  - Javascript
last_modified_at: 2020-08-25T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Javascript Hoisting
article_section: Javascript Hoisting
meta_keywords: Javascript Hoisting
---

# Hosting 은 뭔가요 ?

`Hoisting is a term you will not find used in any normative specification prose prior to ECMAScript® 2015 Language Specification. Hoisting was thought up as a general way of thinking about how execution contexts (specifically the creation and execution phases) work in JavaScript. However, the concept can be a little confusing at first.`

`Conceptually, for example, a strict definition of hoisting suggests that variable and function declarations are physically moved to the top of your code, but this is not in fact what happens. Instead, the variable and function declarations are put into memory during the compile phase, but stay exactly where you typed them in your code.`

`hoisting`는 ECMAScript2015 이전에서는 사용되지 않던 용어다.

`hoisting`은 실행 Contexts(특시 생성 및 실행 단계)가 Javascript에서 어떻게 작용하는지에 대한 일반적인 사고방식으로 생각되었다. 하지만 그 개념은 처음에는 약간 혼란스러울 수 있다.

예를들어, 개념적으로 `hoisting` 에 대한 엄격한 정의는 변수 및 함수 선언이 물리적으로 코드의 맨 위로 이동된다는것을 암시하지만, 실제로 일어나는것은 아니다.

변수 및 함수 선언은 컴파일 단계에서 메모리에 저장되지만, 코드에서 입력한 위치와 정확히 일치한 곳에 있습니다.

> 결론 : 함수나 변수가 컴파일 단계에서 가장 위로 이동된다고 생각하면 될거같다.

---

아래 내용은 통상적으로 코드를 작성하는 예시 이다.

```js
function catName(name) {
    console.log("My cat's name is " + name);
}

catName("Tigger");
```

하지만 아래와 같이 함수를 작성하기 전에 함수를 호출해도 코드는 여전히 동작한다. (`hoisting` 때문이다.)

```js
catName("Tigger");

function catName(name) {
    console.log("My cat's name is " + name);
}
```

`hoisting`은 다른 데이터 타입 및 변수와도 잘 작동합니다. 

변수는 선언하기 전에 초기화하여 사용될 수 있습니다. <b>그러나 초기화 없이는 사용할 수 없습니다.</b>

---

```js
num = 6; // 6
num + 7; // 13
var num; // num = 6 
// num 이 선언되지 않더라도 에러가 나타나지 않습니다.
```

Javascript는 초기화가 아닌 선언만 끌어올립니다.(`hoisting`)

만약 변수를 선언한 뒤 나중에 초기화시켜 사용한다면, 그 값은 undefined로 지정됩니다. 아래는 그 예시입니다.

```js
var x = 1; // x 초기화
console.log(x + " " + y); // '1 undefined'
var y = 2;

var x = 1; // x 초기화
var y; // y 선언
console.log(x + " " + y);// '1 undefined'
y = 2; // y 초기화
```

---

[참조 mozilla](https://developer.mozilla.org/ko/docs/Glossary/Hoisting)