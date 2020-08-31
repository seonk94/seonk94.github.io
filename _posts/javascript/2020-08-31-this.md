---
title:  "Javascript - this"
search: true
categories: 
  - Javascript
tag:
  - Javascript
last_modified_at: 2020-08-31T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Javascript this
article_section: Javascript this
meta_keywords: Javascript this
---


# Javascript - this.

Javascript 에서 함수의 `this`는 다른 언어와 조금 다르게 동작합니다. 

또한 엄격모드와 비 엄격 모드에서도 일부 차이가 있습니다.

> 실행 컨텍스트 (global, function 또는 eval)의 프로퍼티는 비 엄격모드에서 항상 객체를 참조하며, 엄격 모드에서는 어떠한 값이든 될 수 있습니다.

대부분의 경우 `this`의 값은 함수를 호출한 방법에 의해 결정됩니다.

실해중에는 할당으로 설정 할 수 없고 함수를 호출할 때 마다 다를 수 있습니다.

ES5는 함수를 어떻게 호출했는지 상관하지 않고 `this` 값을 설정할 수 있는 bind method를 도입했고, ES2015는 스스로의 `this` 바인딩을 제공하지 않는 화살표 함수를 추가했습니다. (이는 렉시컬 컨텍스트 안의 `this`값을 유지합니다.).

## 전역 문맥

전역 실행 문맥(global execution context)에서 `this`는 엄격 모드 여부에 관계 없이 전역 객체를 참조합니다.

```js
// 웹 브라우저에서는 window객체가 전역 객체
console.log(this === window);   // true

a = 37;
console.log(window.a)  // 37

this.b = "MDN";
console.log(window.b)   // MDN
console.log(b)  // MDN
```


## 함수 문맥

함수 내부에서 `this`의 값은 함수를 호출한 방법에 의해 좌우됩니다.


### 단순 호출

#### 비 엄격모드

```js
function f1() {
    return this;
}

f1() === window; // true
```

#### 엄격모드

`this`값은 실행 문맥에 진입하며 설정되는 값을 유지하므로 `this`는 `undefine`로 남아있습니다.

```js
function f2() {
    "use strict";
    return this;
}

f2() === undefined; // true
```


`this`의 값을 한 문맥에서 다른 문맥으로 넘기려면 다음 예시와같이 `call()` 이나 `apply()` 를 사용하세요.

```js
var obj = { a: 'Custom' };

var a = 'Global';

function whatsThis() {
    return this.a;
}

whatsThis();    // this는 'Global'. 함수내에서 설정되지 않았으므로 global/window객체로 초기값을 설정.
whatsThis.call(obj);    // this는 'Custom'. 함수 내에서 obj로 설정한다.
whatsThis.apply(obj);    // this는 'Custom'. 함수 내에서 obj로 설정한다.
```


### bind method

`f.bind(someObject)`를 호출하면 f와 같은 범위를 가졌지만, `this`는 원본 함수를 가진 새로운 함수를 생성합니다.

새 함수의 `this`는 호출 방식과 상관없이 영구적으로 `bind()`의 첫번째 매개변수로 고정됩니다.

```js
function f() {
    return this.a;
}

var g = f.bind({a: 'azerty'});
console.log(g());   // azerty

var h = g.bing({a: 'yoo'});     // bind는 한번만 작동함.
console.log(h())    // azerty

var o = { a: 37, f:f, g:g, h:h };
console.log(o.a, o.f(), o.g(), o.h());  // 37, 37, azerty, azerty
```

### 화살표 함수

화살표 함수에서 `this`는 자신을 감싼 정적 범위(lexical context)입니다.

전역 코드에서는 전역 객체를 가리킵니다.

```js
var globalObject = this;
var foo = (() => this);
console.log(foo() === globalObject); // true
```

> 화살표 함수를 call(), bind(), apply()를 사용해 호출할 때 this의 값을 정해주더라도 무시합니다. 사용할 매개변수를 정해주는 건 문제 없지만, 첫 번째 매개변수(thisArg)는 null을 지정해야 합니다.

```js
// 객체로서 메서드 호출
var obj = {func: foo};
console.log(obj.func() === globalObject); // true

// call을 사용한 this 설정 시도
console.log(foo.call(obj) === globalObject); // true

// bind를 사용한 this 설정 시도
foo = foo.bind(obj);
console.log(foo() === globalObject); // true
```


### 객체의 method

함수를 어떤 객체의 메서드로 호출하면 `this`의 값은 그 객체를 사용합니다.

```js
var o = {
  prop: 37,
  f: function() {
    return this.prop;
  }
};

console.log(o.f()); // 37
```

### 생성자

함수를 `new`키워드와 함께 생성자로 사용하면 `this`는 새로 생긴 객체에 묶입니다.

```js
function C() {
    this.a = 37;
}

var o = new C();
console.log(o.a);    // 37
```

### DOM 이벤트 처리기

함수를 이벤트 처리기로 사용하면 `this`는 이벤트를 발사한 요소로 설정됩니다.

> 일부 브라우저는 `addEventListener()`외의 다른 방법으로 추가한 처리기에 대해선 이 규칙을 따르지 않습니다.

```js
// 처리기로 호출하면 관련 객체를 파랗게 만듦
function bluify(e) {
  // 언제나 true
  console.log(this === e.currentTarget); 
  // currentTarget과 target이 같은 객체면 true
  console.log(this === e.target);
  this.style.backgroundColor = '#A5D9F3';
}

// 문서 내 모든 요소의 목록
var elements = document.getElementsByTagName('*');

// 어떤 요소를 클릭하면 파랗게 변하도록
// bluify를 클릭 처리기로 등록
for (var i = 0; i < elements.length; i++) {
  elements[i].addEventListener('click', bluify, false);
}
```

### 인라인 이벤트 핸들러

코드를 인라인 이벤트 처리기로 사용하면 `this`는 처리기를 배치한 DOM 요소로 설정됩니다.

```html
<button onclick="alert(this.tagName.toLowerCase());">
  this 표시
</button>
```

---

[참조](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/this)
