---
title:  "Javascript - .call & .apply "
search: true
categories: 
  - Javascript
tag:
  - Javascript
last_modified_at: 2020-08-25T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Javascript .call & .apply
article_section: Javascript .call & .apply
meta_keywords: Javascript .call & .apply
---

# Javascript 함수 기본 method

## .call

call() method는 주어진 this값, 각각 전달된 인수와 함께 함수를 호출합니다.

> func.call(this, arg1, arg2 ...)

`The call() allows for a function/method belonging to one object to be assigned and called for a different object.
call() provides a new value of this to the function/method. With call(), you can write a method once and then inherit it in another object, without having to rewrite the method for the new object.`

`call()` 은 한 객체에 속하는 function/method 가 다른 객체에 할당되고 호출되는것을 허용한다.

`call()`은 function/method 에 this 의 새로운 가치를 제공한다. 

method 를 한번 작성하면 새 객체를 위한 method를 재작성할 필요 없이 `call()`을 이용해 다른 객체에 상속할 수 있습니다.

### 객체의 생성자 연결에 .call 사용

`
You can use call to chain constructors for an object (similar to Java).
In the following example, the constructor for the Product object is defined with two parameters: name and price.
Two other functions, Food and Toy, invoke Product, passing this, name, and price. Product initializes the properties name and price, both specialized functions define the category.
`

객체의 생성자 연결에 call을 사용할 수 있다.(Java와 비슷하게,)

다음 예제에서 Product 객체의 생성자는 `name` 및 `price`를 매개변수로 정의합니다.

다른 두 함수 `Food`, `Toy` 는 `this` 및 `name`, `price`를 전달하는 `Product`를 호출합니다.

`Product`는 `name`, `price`속성을 초기화하고, 특수한 두 함수 (`Food`, `Toy`) 는 `category`를 정의합니다.

```js
function Product(name, price) {
    this.name = name;
    this.price = price;

    if (price < 0) {
        throw RangeError(`Cannot create product ${this.name} with a negative price`);
    }
}

function Food(name, price) {
    Product.call(this, name, price);
    this.category = 'food';
}

function Toy(name, price) {
    Product.call(this, name, price);
    this.category = 'toy';
}

const cheese = new Food('feta', 5);
const fun = new Toy('robot', 40);
```

### 익명 함수 호출에 .call 사용

`
In this example, we create an anonymous function and use call to invoke it on every object in an array.
The main purpose of the anonymous function here is to add a print function to every object, which is able to print the correct index of the object in the array.
`

이 예제에서는 익명 함수를 만들고 배열 내 모든 객체에서 이를 호출하기 위해 `call()` 을 사용합니다.

여기서 익명 함수의 주 목적은 배열 내 객체의 정확한 `index`를 출력할 수 있는 모든 객체에 `print` 함수를 추가하는 것 입니다.

> `this`값으로 객체 전달이 반드시 필요하지는 않지만, 설명의 목적으로 사용

```js
const animals = [
    { species: 'Lion', name: 'King' },
    { species: 'Whale', name: 'Fail' },
];

for (let index = 0; index < animals.length; index++) {
    (function(index) {
        this.print = function() {
            console.log(`#${index} ${this.species} : ${this.name}`)
        }
    }).call(animals[index], index)
}
```


### 함수 호출 및 'this'를 위한 문맥 지정에 .call 사용

`
In the example below, when we call greet, the value of this will be bound to object obj.
`

아래 예제에서, `greet`를 호출하면, `this` 값은 객체 `obj`에 bind 됩니다.

```js
function greet() {
    const reply = [this.animal, 'typically sleep between', this.sleepDuration].join(' ');
    console.log(reply);
}

const obj = {
    animal: 'cats', sleepDuration: '12 and 16 hours'
};

greet.call(obj); // cats typically sleep between 12 and 16 hours
```

### 첫번째 인수 지정 없이 함수 호출에 .call 사용

`
In the example below, we invoke the display function without passing the first argument. If the first argument is not passed, the value of this is bound to the global object.
`

아래 예제에서, `display` 함수에 첫번째 인수를 전달하지 않고 호출합니다.

첫번째 인수를 전달 하지 않으면, `this`의 값은 전역 객체에 바인딩됩니다.

```js
const sData = 'Wisen';
function display() {
    console.log(`sDate value is ${this.sDate}`);
};

display.call(); // sDate value is Wisen
```

> 주의 : `strict mode` 에서, `this`는 `undefined`값을 가집니다.

```js
'use strict';

const sData = 'Wisen';

function display() {
    console.log(`sDate value is ${this.sDate}`);
};

display.call(); // Cannot read the property of 'sData' of undefined
```


## .apply

apply() method는 주어진 this값, 전달된 배열 (또는 유사 배열 객체)와 함께 함수를 호출합니다.

> func.apply(this, [argsArray]])

> 나머지 설명들은 call과 거의 유사함.

### 배열에 배열을 붙이기 위해 .apply 사용하기

```js
const array = ['a', 'b'];
const elements = [0, 1, 2];
array.push.apply(array, elements);
console.log(array); //['a', 'b', 0, 1, 2]
```

### .apply 와 내장함수 사용

apply 를 보다 효과적으로 이용하면 일부 내장 함수는 어떤 작업에 대해서는 배열과 루프없이 쉽게 처리됩니다. 

다음 예제에서는 배열에서 최대값과 최소값을 구하기 위해 Math.max/Math.min 함수를 이용하고 있습니다.


```js
const numbers = [5, 6, 2, 3, 7];

const max = Math.max.apply(null, numbers);

const min = Math.min.apply(null, numbers);
```

> Math.max(...numbers) 이거랑 다른건가..?


> Javascript 엔진의 인수 길이 제한을 초과하는 위험성에대해 이해한 뒤, 조심할것.


### 생성자 체이닝을 위한 .apply 사용

Java와 유사하게, 객체를 위한 constructors 체이닝을 위해 `apply`를 사용할 수 있다.

```js
Function.prototype.construct = function(args) {
    const oNew = Object.create(this.prototype);
    this.apply(oNew, args);
    return oNew;
}
```

```js
function MyConstructor() {
    for(let nProp = 0; nProp < arguments.length; nProp++) {
        this.['property' + nProp] = arguments[nProp]
    }
}

const myArray = [4, 'Hello World', false];
const myInstance = MyConstructor.construct(myArray);

console.log(myInstance.property1) // 'Hello World!'
console.log(myInstance instanceof MyConstructor) // 'true'
console.log(myInstance.constructor) // 'MyConstructor'
```

---

> .call 은 인수목록을 받고, .apply는 인수 배열 하나를 받는차이가 있다.

---

[call 참조 mozilla](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

[apply 참조 mozilla](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)