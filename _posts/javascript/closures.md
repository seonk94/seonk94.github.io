---
title:  "Javascript - Closures"
search: true
categories: 
  - Javascript
tag:
  - Javascript
last_modified_at: 2020-08-28T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Javascript Closures
article_section: Javascript Closures
meta_keywords: Javascript Closures
---

# Closures

클로저는 함수와 함수가 선언된 어휘적 환경의 조합이다. 클로저를 이해하려면 자바스크립트가 어떻게 변수의 유효범위를 지정하는지(Lexical scoping)를 먼저 이해해야 한다.

## Lexical Scoping

```js
function init() {
    const name = 'Mozilla'; // name 은 init에 의해 생성된 지역 변수이다.
    function displayName() {    // displayName()은 내부 함수이며, 클로저다.
        alert(name);    // 부모 함수에서 선언된 변수를 사용한다.
    }
    displayName();
}
init();
```

`init()`은 지역 변수 `name`과 함수 `displayName()`을 생성한다. `displayName()`은 `init()` 안에 정의된 내부 함수이며 `init()` 함수 본문에서만 사용할 수 있다.

여기서 주의할 점은 `displayName()` 내부엔 자신만의 지역 변수가 없닫는 점이다. 그런데 함수 내부에서 외부 함수의 변수에 접근할 수 있기 때문에 `displayName()`역시 부모 함수 `init()` 에서 선언된 변수 `name`에 접근할 수 있다.

만약 `displayName()`가 자신만의 `name`변수를 가지고 있었다면, `name` 대신 `this.name`을 사용했을 것이다.

이는 Lexical Scoping 의 한 예이다.

여기서 'Lexical'이란 `Lexical Scoping`과정에서 변수가 어디에서 사용 가능한지 알기 위해 그 변수가 소스코드 내 어디에서 선언되었는지 고려한다는 것을 의미한다.

중첩된 함수는 외부 범위(scope)에서 선언한 변수에도 접근할 수 있다.

## Closure를 알아보자

```js
function makeFunc() {
  const name = 'Mozilla'
  function displayName() {
    alert(name);
  }
  return displayName;
}

const myFunc = makeFunc();
// myFunc 변수에 displayName을 리턴.
// 유효범위의 어휘적 환경을 유지.
myFunc();
// 리턴된 displayName 함수를 실행 (name변수에 접근)
```

이 코드는 바로 전의 예제와 완전히 동일한 결과가 실행된다. 하지만 흥미로운 차이는 `displayName()`함수가 실행되기 전에 외부함수인 `makeFunc()`로부터 리턴되어 `myFunc` 변수에 저장된다는 것이다.


한 눈에 봐서는 이 코드가 여전히 작동하는 것이 직관적으로 보이지 않을 수 있다. 

몇몇 프로그래밍 언어에서, 함수 안의 지역 변수들은 그 함수가 처리되는 동안에만 존재한다. `makeFunc()` 실행이 끝나면(`displayName`함수가 리턴되고 나면) `name` 변수에 더 이상 접근할 수 없게 될 것으로 예상하는 것이 일반적이다.

하지만 위의 예시와 자바스크립트의 경우는 다르다. 

그 이유는 자바스크립트는 함수를 리턴하고, 리턴하는 함수가 클로저를 형성하기 때문이다. 

클로저는 함수와 함수가 선언된 어휘적 환경의 조합이다. 

이 환경은 클로저가 생성된 시점의 유효 범위 내에 있는 모든 지역 변수로 구성된다. 

첫 번째 예시의 경우, `myFunc은` `makeFunc`이 실행될 때 생성된 `displayName` 함수의 인스턴스에 대한 참조다. 

`displayName`의 인스턴스는 변수 `name` 이 있는 어휘적 환경에 대한 참조를 유지한다. 이런 이유로 `myFunc`가 호출될 때 변수 `name`은 사용할 수 있는 상태로 남게 되고 "Mozilla" 가 alert 에 전달된다.

```js
function makeAdder(x) {
  let y = 1;
  return function(z) {
    y = 100;
    return x + y + z;
  };
}

const add5 = makeAdder(5);
const add10 = makeAdder(10);
// 클로저에 x, y 의 환경이 저장됨.

console.log(add5(2)); // 107 (x: 5, y: 100, z: 2)
console.log(add10(2));  // 112 (x: 10, y: 100, z: 2)
// 함수 실행 시 클로저에 저장된 x, y 값에 접근하여 값을 계산.
```

이 예제에서 단일 인자 x를 받아서 새 함수를 반환하는 함수 `makeAdder(x)`를 정의했다. 반환되는 함수는 단일 인자 z를 받아서 x와 y와 z의 합을 반환한다.

본질적으로 `makeAdder`는 함수를 만들어내는 공장이다. 

이는 `makeAdder`함수가 특정한 값을 인자로 가질 수 있는 함수들을 리턴한다는 것을 의미한다. 

위의 예제에서 `add5`, `add10` 두 개의 새로운 함수들을 만들기 위해 `makeAdder`함수 공장을 사용했다. 

하나는 매개변수 x에 5를 더하고 다른 하나는 매개변수 x에 10을 더한다.


`add5`와 `add10`은 둘 다 클로저이다. 

이들은 같은 함수 본문 정의를 공유하지만 서로 다른 맥락(어휘)적 환경을 저장한다. 

함수 실행 시 `add5`의 맥락적 환경에서 클로저 내부의 x는 5 이지만 `add10`의 맥락적 환경에서 x는 10이다. 

또한 리턴되는 함수에서 초기값이 1로 할당된 y에 접근하여 y값을 100으로 변경한 것을 볼 수 있다. (물론 x값도 동일하게 변경 가능하다.) 

이는 클로저가 리턴된 후에도 외부함수의 변수들에 접근 가능하다는 것을 보여주는 포인트이며 클로저에 단순히 값 형태로 전달되는 것이 아니라는 것을 의미한다.


## 어디에 사용하죠 ?

### private method 흉내내기

```js
const counter = (function() {
  let privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  };
})();

console.log(counter.value()); // logs 0
counter.increment();
counter.increment();
console.log(counter.value()); // logs 2
counter.decrement();
console.log(counter.value()); // logs 1
```

카운터를 익명함수로 생성하지 않고, 별도의 변수 makeCounter로 저장하고 여러개의 카운터를 만들수도있다.

```js
const makeCounter = function() {
  let privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  };
};
```


## loop에서 클로저 생성하기

ECMAScript2015 의 let 소개전에는 클로저와 관련된 일반적인 문제는 loop안에서 클로저가 생성되었을때 발생하다.

```html
<p id="help">Helpful notes will appear here</p>
<p>E-mail: <input type="text" id="email" name="email"></p>
<p>Name: <input type="text" id="name" name="name"></p>
<p>Age: <input type="text" id="age" name="age"></p>
```

```js
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = function() {
      showHelp(item.help);
    }
  }
}

setupHelp(); 
```

`helpText` 배열은 세 개의 도움말 힌트를 정의한다. 

각 도움말은 문서의 입력 필드의 ID와 연관된다. 루프를 돌면서 각 입력 필드 ID에 해당하는 엘리먼트의 `onfocus` 이벤트에 관련된 도움말을 보여주는 메소드에 연결한다.

이 코드를 사용하면 제대로 동작하지 않는 것을 알게 된다. 어떤 필드에 포커스를 주더라도 나이에 관한 도움말이 표시된다.

`onfocus` 이벤트에 연결된 함수가 클로저이기 때문이다. 이 클로저는 함수 정의와 `setupHelp` 함수 범위에서 캡처된 환경으로 구성된다. 

루프에서 세 개의 클로저가 만들어졌지만 각 클로저는 값이 변하는 변수가 (`item.help`) 있는 같은 단일 환경을 공유한다. 

`onfocus` 콜백이 실행될 때 콜백의 환경에서 `item` 변수는 (세개의 클로저가 공유한다) `helpText` 리스트의 마지막 요소를 가리키고 있을 것이다.

이 경우 한 가지 해결책은 더 많은 클로저를 사용하는 것이다: 특히 앞에서 설명한 함수 팩토리를 사용하는 것이다.

```js
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function makeHelpCallback(help) {
  return function() {
    showHelp(help);
  };
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = makeHelpCallback(item.help);
  }
}

setupHelp(); 
```

모두 단일 환경을 공유하는 콜백대신, `makeHelpCallback` 함수는 각각의 콜백에 새로운 어휘적 환경을 생성한다. 

여기서 `help`는 `helpText` 배열의 해당 문자열을 나타낸다.


더 많은 클로저를 사용하는 것이 싫다면 ES2015의 `let` 키워드를 사용할 수 있다.

```js
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  const helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (let i = 0; i < helpText.length; i++) {
    let item = helpText[i];
    document.getElementById(item.id).onfocus = function() {
      showHelp(item.help);
    }
  }
}

setupHelp();
```

위의 경우 `var` 대신 `let`을 사용하여 모든 클로저가 블록 범위 변수를 바인딩할 것이므로 추가적인 클로저를 사용하지 않아도 완벽하게 동작할 것이다.


## 성능 관련 고려 사항

특정 작업에 클로저가 필요하지 않는데 다른 함수 내에서 함수를 불필요하게 작성하는 것은 현명하지 않다. 

이것은 처리 속도와 메모리 소비 측면에서 스크립트 성능에 부정적인 영향을 미칠 것이다.

예를 들어, 새로운 객체/클래스를 생성 할 때, 메소드는 일반적으로 객체 생성자에 정의되기보다는 객체의 프로토타입에 연결되어야 한다.

그 이유는 생성자가 호출 될 때마다 메서드가 다시 할당되기 때문이다 (즉, 모든 개체가 생성 될 때마다).

```js
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
}
MyObject.prototype = {
  getName: function() {
    return this.name;
  },
  getMessage: function() {
    return this.message;
  }
};
```

그러나 프로토타입을 다시 정의하는것은 권장되지 않음으로 기존 프로토타입에 추가하는 다음 예제가 더 좋다.

```js
function MyObject(name, message) {
  this.name = name.toString();
  this.message = message.toString();
}
MyObject.prototype.getName = function() {
  return this.name;
};
MyObject.prototype.getMessage = function() {
  return this.message;
};
```

더 깨끗한 방법

```js
function MyObject(name, message) {
    this.name = name.toString();
    this.message = message.toString();
}
(function() {
    this.getName = function() {
        return this.name;
    };
    this.getMessage = function() {
        return this.message;
    };
}).call(MyObject.prototype);
```

앞의 두 가지 예제에서 상속된 프로토타입은 모든 객체에서 공유될 수 있으며 메소드 정의는 모든 객체 생성시 발생할 필요가 없다. 객체 모델의 세부 사항을 참고하라.

---

> 대부분 let 을 사용하면 클로저에 관한 실수를 고려안해도 괜찮은건지 ...

> 개념은 익혔지만 어디에 잘 사용해야할지...

---

[참조](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures)