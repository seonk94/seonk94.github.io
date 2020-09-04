---
title:  "HTML - script"
search: true
categories: 
  - HTML
tag:
  - HTML
last_modified_at: 2020-09-04T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1:  script
article_section: script
meta_keywords: script
---

# Script

HTML `<script>` 요소는 데이터와 실행 가능한 코드를 문서에 포함할 때 사용하며 보통 JavaScript 코드와 함께 씁니다.


# 특성

> 이 요소는 전역 특성을 포함합니다.

## async

일반 스크립트에 `async` 속성이 존재하면 `HTML` 구문 분석 중에도 스크립트를 가져오며, 사용 가능해지는 즉시 평가를 수행합니다.

모듈 스크립트에 `async` 속성이 존재하면 모듈 스크립트와 모든 의존 스크립트를 지연 큐에서 실행하므로 함께 병렬로 불러오며, 이와 동시에 구문 분석을 수행하고 사용 가능해지는 즉시 평가합니다.

기존 방식은 브라우저가 `HTML` 분석을 계속하기 전에 스크립트를 불러오고 평가했어야 하므로, `async` 속성을 사용하면 분석기를 멈추는 JavaScript를 제거할 수 있습니다. defer도 비슷한 효력을 냅니다.

`async`는 불리언 속성입니다. 속성이 존재하면 참을 나타내고, 속성이 존재하지 않으면 거짓을 나타냅니다.

## crossorigin

일반 `script` 요소는 표준 CORS를 통과하지 못했을 때 `window.onerror`에 최소한의 정보만 넘깁니다. `crossorigin` 속성은 정적 미디어에 대해 별도의 도메인을 사용하는 사이트의 오류 기록을 허용하기 위해 사용할 수 있습니다.

## defer

브라우저가 스크립트를 문서 분석 이후에, 그러나 `DOMContentLoaded` 발생 이전에 실행해야 함을 나타내는 불리언 속성입니다.

`defer` 속성을 가진 스크립트는 자신의 평가가 끝나기 전까지 `DOMContentLoaded` 이벤트의 발생을 막습니다.

> src 속성이 존재하지 않을 때(인라인 스크립트인 경우 등)에는 아무런 효과도 없으므로 사용해서는 안됩니다. <br> 또한 모듈 스크립트는 기본적으로 지연 평가하므로, defer를 지정해도 변화가 없습니다.

`defer` 속성을 가진 스크립트는 문서상의 순서를 따라 실행됩니다.

기존 방식은 브라우저가 HTML 분석을 계속하기 전에 스크립트를 불러오고 평가했어야 하므로, `defer` 속성을 사용하면 분석기를 멈추는 JavaScript를 제거할 수 있습니다. `async`도 비슷한 효과를 가집니다.

## referrerpolicy

스크립트를 가져올 때, 또는 스크립트가 다른 리소스를 가져올 때 전송할 리퍼러를 나타냅니다.

## src

외부 스크립트를 가리키는 URI입니다. 문서 내에 스크립트를 직접 삽입하는 것 대신 사용할 수 있습니다.


## type

스크립트의 유형을 나타냅니다. 다음 다섯개의 범주 중 하나에 속할 수 있습니다.

| 유형 | 설명 |
|-------|--------|
|생략 또는 Javascript MIME 유형 | 스크립트가 JavaScript임을 나타냅니다.  이 경우, HTML5 명세는 웹 작성자가 불필요한 type을 포함하지 않고 완전히 제외할 것을 촉구합니다. 보다 오래된 브라우저에서는 type 특성의 값으로 삽입 혹은 (src 특성으로) 불러온 스크립트의 언어를 표시하곤 했습니다. |
| module | 스크립트를 Javascript모듈로 간주합니다. 스크립트 콘텐츠 처리가 charset과 defer 특성의 영향을 받지 않습니다. 기존 스크립트와 달리, 모듈 스크립트는 교차 출처 가져오기 시 CORS 프로토콜을 사용해야 합니다.|
| 다른 모든 값 | 내장 콘텐츠를 브라우저가 처리하지 않을 데이터 블록으로 간주합니다. 개발자는 반드시 유효하면서 JavaScript가 아닌 MIME 유형을 지정해야 합니다. src 특성을 무시합니다. |


---

[참조](https://developer.mozilla.org/ko/docs/Web/HTML/Element/script)