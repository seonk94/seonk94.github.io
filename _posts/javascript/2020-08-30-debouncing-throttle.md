---
title:  "Javascript - Debouncing & Throttle"
search: true
categories: 
  - Javascript
tag:
  - Javascript
last_modified_at: 2020-08-30T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Javascript Debouncing & Throttle
article_section: Javascript Debouncing & Throttle
meta_keywords: Javascript Debouncing & Throttle
---

# Debouncing & Throttle

웹 페이지를 구성하는 많은 이벤트들 중에, 윈도우 크기의 변화, 스크롤 변화, 변수의 변화에 맞춰 발생하는 이벤트들이 있습니다.

예를들어 스크롤에 할당되어있는 이벤트 리스너가 있을경우, 스크롤을 빨리 내려버리면 수많은 이벤트들이 호출되고, 이는 성능에 안좋은 영향을 미치게됩니다.

위와같은 문제를 해결하고자 Debouncing 또는 Throttle 를사용합니다.

## Debounceing

아래는 debounce 예제이다.

clearTimer 을 이용해 마지막 호출 이후, 일정시간이 지난후에 한번만 이벤트를 호출한다.

```js
const debounceTimer = null;

function debounce (callback, milliseconds) {
    return function() {
        if (debounceTimer) {
            clearTimeout(debounceTimer);
        }
        debounceTimer = setTimeout(() => {
            callback();
        }, milliseconds)
    }
}
```

## Throttle

아래는 Throttle 예제이다.

setTimeout을 이용해 설정한 주기마다 이벤트가 실행되며, 실행이 끝난이후에 다시 timer를 false로 만들어, 설정한 주기마다 이벤트가 한번씩 호출된다.

```js
let throttleTimer;
function throttle(callback, milliseconds) {
    return function() {
        if (!throttleTimer) {
            throttleTimer = setTimeout(() => {
                callback();
                throttleTimer = false;
            }, milliseconds)
        }
    }
}
```