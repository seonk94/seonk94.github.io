---
title:  "Javascript - Event"
search: true
categories: 
  - Javascript
tag:
  - Javascript
last_modified_at: 2020-09-01T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Javascript Event
article_section: Javascript Event
meta_keywords: Javascript Event
---


# 이벤트

DOM Event 는 무언가 발생하것에 대한 것을 코드에 알리기위해 전달됩니다.

Event Interface 를 기반으로 객체에 의해 표현되며, 발생한 것에 대한 부가적인 정보를 얻는데 사용되는 추가적인 커스텀 필드 또는 함수를 가질 수 있습니다.

> [이벤트 목록](https://developer.mozilla.org/ko/docs/Web/Events)


# 이벤트 등록

```html
<button>button</button>
```

위와 같은 button 에 이벤트를 등록해보자.

```js
const button = document.querySelector('button');

const clickHandler = (event) => {
    console.log(event);
}

button.addEventListener('click', clickHandler);
```

# 이벤트 버블링 - Event Bubbling


이벤트 버블링은 이벤트가 발생했을 때 해당 이벤트가 더 상위 태그로 전달되어가는 특성이다.

```html
<div class="div-one">
    <div class="div-two">
        <div class="div-three">
            <button class="button"></button>
        </div>
    </div>
</div>
```

```js
const divs = document.querySelectorAll('div');
const button = document.querySelector('button');


const clickHandler = (e) => {
    console.log(e.currentTarget.className)
}
button.addEventListener('click', clickHandler)
divs.forEach(div => {
    div.addEventListener('click', clickHandler)
})
```

위와같은 코드에서 `button`을 눌렀을때 다음과같은 결과를 얻을 수 있다.

```
button
div-three
div-two
div-one
```


# 이벤트 캡쳐링 - Event Capture

이벤트 캡쳐링은 버블링과 반대 개념이다.

```html
<div class="div-one">
    <div class="div-two">
        <div class="div-three">
            <button class="button"></button>
        </div>
    </div>
</div>
```


```js
const divs = document.querySelectorAll('div');
const button = document.querySelector('button');


const clickHandler = (e) => {
    console.log(e.currentTarget.className)
}
button.addEventListener('click', clickHandler, {
    capture: true
})
divs.forEach(div => {
    div.addEventListener('click', clickHandler, {
        capture: true
    })
})
```

아까의 코드에서 event를 등록할때, 변수로 `{capture : true}`를 주면 버블링과 반대방향으로 탐색할 수 있다.

아까와 같이 button을 누른다면 다음과같은 결과를 얻을 수 있다.

```
div-one
div-two
div-three
button
```

# 이벤트 전파 막기

```js
event.stopPropagation();
```

위의 코드는 이벤트 전파를 막습니다.

이벤트 버블링의 경우에는 클릭한 요소의 이벤트만 처리하고, 상위로 이벤트를 전달하는것을 막고,

이벤트 캡쳐링의 경우에도 하위로 이벤트를 전달하지 않습니다.

## `event.preventDefault` 는 뭔가요 ?  

이벤트를 취소할 수 있는 경우, 이벤트의 전파를 막지않고 그 이벤트를 취소합니다.

```html
<input type="text" onkeypress="checkName(event);"/>
```

```js
function checkName(evt) {
var charCode = evt.charCode;

  if (charCode != 0) {
    if (charCode < 97 || charCode > 122) {
      evt.preventDefault();
      alert("소문자만 입력할 수 있습니다." + "\n"
            + "charCode: " + charCode + "\n"
      );
    }
  }
}
```

위와 같은 코드들을 예로든다면, 조건문에 맞지 않는 문자의 경우 input 에 올바르지 않는 텍스트를 막을수 있습니다.

---
