---
title:  "Javascript - Null & Undefined "
search: true
categories: 
  - Javascript
tag:
  - Javascript
last_modified_at: 2020-08-24T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Javascript Null & Undefined
article_section: Javascript Null & Undefined
meta_keywords: Javascript Null & Undefined
---

# Null ??

`The value null represents the intentional absence of any object value. It is one of JavaScript's primitive values and is treated as falsy for boolean operations.`

`null` 값은 임의의 객체 값의 의도적으로 absence(부재) 를 표현한다.

자바스크립트의 원시 값 중 하나이며 boolean 연산의 경우 false(?) 으로 처리된다.

> 원시값이란 ?
객체가 아니면서 method도 가지지 않는 데이터. string, number, bigint, boolean, undefined, symbol 여섯종류. 원시값처럼 보이는 `null` 도 있지만, 사실 모든 Object, 모든 구조화된 자료형은 Prototype Chain 에 따라 `null`의 자손입니다. [출처](https://developer.mozilla.org/en-US/docs/Glossary/Primitive)

```js
const a = null;
console.log(typeof a) // object
```

> > 결국 `null`은 원시값이 아닌듯하다.


`The value null is written with a literal: null. null is not an identifier for a property of the global object, like undefined can be. Instead, null expresses a lack of identification, indicating that a variable points to no object. In APIs, null is often retrieved in a place where an object can be expected but no object is relevant.`

`null` 값은 literal: `null` 로 작성된다.

`null`은 정의되지 않은 것과 같이 글로벌 개체의 속성에 대한 식별자가 아니다.

대신 `null`은 식별의 부족을 나타내며 변수가 아무 물체를 가리키지 않음을 나타낸다.

API에서 `null`은 객체를 예상할 수 있지만 관련 있는 객체가 없는 장소에서 검색되는 경우가 많다. ?


# Undefined

`The global undefined property represents the primitive value undefined. It is one of JavaScript's primitive types.`

글로벌 `undefined` 속성은 정의되지 않은 원시적 가치를 나타낸다. Javascript 의 원시 유형중 하나이다.  


`undefined is a property of the global object. That is, it is a variable in global scope. The initial value of undefined is the primitive value undefined.`

`undefined` 는 전역 객체의 속성입니다. 즉 전역 스코프에서의 변수입니다. `undefined` 의 초기값은 `undefined` 원시값입니다. ?

`In modern browsers (JavaScript 1.8.5 / Firefox 4+), undefined is a non-configurable, non-writable property, per the ECMAScript 5 specification. (Even when this is not the case, avoid overriding it.)`

최신 브라우저에서 `undefined` 는 ECMAScript5 명세에 따라 설정 불가(non-configurable), 쓰기 불가능한(non-writable) 속성입니다.

`A variable that has not been assigned a value is of type undefined. A method or statement also returns undefined if the variable that is being evaluated does not have an assigned value. A function returns undefined if a value was not returned.`

값을 할당받지 않은 변수는 `undefined` 자료형입니다. 또한 method 와 선언도 평가할 변수가 값을 할당받지 않은 경우에 `undefined` 를 반환합니다. 함수(method)는 값을 명시적으로 반환하지 않으면 `undefined` 를 반환합니다.

```js
var x;
if (typeof x === 'undefined') { // typeof 사용한 이유는 선언하지 않은 변수를 사용해도 오류 발생 x
    // true
}
```

# 결론

- `undefined는` 변수를 선언하고 값을 할당하지 않은 것 
```js
var test;
```

- `null` 는 변수를 선언하고 `null`을 할당한 것
```js
var test;
test = null;
```

---

[Null 참조](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null)

[Undefined 참조](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/undefined)