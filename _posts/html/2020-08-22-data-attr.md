---
title:  "HTML - data-* attributes ??"
search: true
categories: 
  - HTML
tag:
  - HTML
last_modified_at: 2020-08-21T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1:  data-* attributes 
article_section: data-* attributes 는 뭔가요?
meta_keywords: data-* attributes 
---

# data-* 속성이 뭐지?

`
HTML5 is designed with extensibility in mind for data that should be associated with a particular element but need not have any defined meaning. data-* attributes allow us to store extra information on standard, semantic HTML elements without other hacks such as non-standard attributes, extra properties on DOM, or Node.setUserData().
`

HTML5 는 특정 요소와 연관되어야 하지만 정의된 의미를 가질 필요는 없는 데이터에 대해 확장성을 염두에 두고 설계되었습니다.

data-* 속성은 비표준 속성, DOM, Node.setUserData() 와 같이 추가적인 조작(hacks)? 없이도 표준 HTML 요소에 대한 추가 정보를 저장할 수 있다.

# 어떻게 사용하나요 ?

`
The syntax is simple. Any attribute on any element whose attribute name starts with data- is a data attribute. Say you have an article and you want to store some extra information that doesn’t have any visual representation. Just use data attributes for that:
`

태그 안에 data- 로 시작사는 속성 아무거나 사용할 수 있다.

시각적 표현 없이 추가정보를 저장할 수 있다.

```html
<article
  id="electric-cars"
  data-columns="3"
  data-index-number="12314"
  data-parent="cars">
...
</article>
```

## Javascript 에서 사용하기

`
Reading the values of these attributes out in JavaScript is also very simple. You could use getAttribute() with their full HTML name to read them, but the standard defines a simpler way: a DOMStringMap you can read out via a dataset property.
`
`
To get a data attribute through the dataset object, get the property by the part of the attribute name after data- (note that dashes are converted to camelCase).
`

JS 에서 작성한 속성들의 값을 호출하는것도 매우 간단하다.

```js
const article = document.querySelector('#electric-cars');
 
article.dataset.columns // "3"
article.dataset.indexNumber // "12314"
article.dataset.parent // "cars"
```

`getAttribute()`를 이용해 호출할수도 있지만, DOMStringMap 을 이용해 간단히 값을 속성값을 사용할 수 있다.

dataset객체를 통해 data- 뒷 부분의 이름을 사용해서 값을 읽거나 수정할 수 있다.

`article.dataset.columns = 5` 라고 설정하면 해당 속성을 `5`라고 변경한다.

## CSS 에서는 어떻게 활용하지?

`
Note that, as data attributes are plain HTML attributes, you can even access them from CSS. For example to show the parent data on the article you can use generated content in CSS with the attr() function:
`

데이터 속성은 일반 HTML 속성이기 때문에 CSS 에서도 access 할수 있다.

예를들어 `article` 의 `before`에 데이터를 표시하려면 `attr(data-*)` 를 사용하면된다.

```css
article::before {
  content: attr(data-parent);
}
```

`You can also use the attribute selectors in CSS to change styles according to the data:`

CSS의 속성 Selectors 를 사용해 데이터에 따라 스타일을 변경할수도 있다.

```css
article[data-columns='3'] {
  width: 400px;
}
article[data-columns='4'] {
  width: 600px;
}
```

`
Data attributes can also be stored to contain information that is constantly changing, like scores in a game. Using the CSS selectors and JavaScript access here this allows you to build some nifty effects without having to write your own display routines.
`

또한 데이터 속성은 게임의 점수처럼 끊임없이 변화하는 정보를 포함하도록 저장될 수 있다.

여기서 CSS 선택기와 JavaScript 액세스를 사용하면 자신만의 디스플레이 루틴을 작성할 필요 없이 약간의 nifty 효과를 만들 수 있다.


`
Data values are strings. Number values must be quoted in the selector for the styling to take effect.
`

데이터 값은 문자열이다. 스타일링을 적용하려면 선택기에서 숫자 값을 인용해야 한다.


# Issues

`
Do not store content that should be visible and accessible in data attributes, because assistive technology may not access them. In addition, search crawlers may not index data attributes' values.
`

보조 기술이 데이터에 액세스할 수 없을 수 있으므로 데이터 속성에서 볼 수 있고 액세스할 수 있어야 하는 콘텐츠를 저장하지 않도록 주의해야한다. 

또한 검색 크롤러는 데이터 속성의 값을 인덱싱하지 못할 수 있다.

`
The main issues to consider are Internet Explorer support and performance. Internet Explorer 11+ provides support for the standard, but all earlier versions do not support dataset. To support IE 10 and under you need to access data attributes with getAttribute() instead. Also, the performance of reading data-attributes compared to storing this data in a regular JS object is poor.
`

고려해야 할 주요 이슈는 인터넷 익스플로러 지원과 성능이다. 

Internet Explorer 11+는 표준에 대한 지원을 제공하지만 모든 이전 버전은 데이터 집합을 지원하지 않는다. 

IE 10 이하를 지원하려면 getAttribute() 대신 getAttribute()를 사용하여 데이터 특성에 액세스해야 한다. 

또한 이 데이터를 일반 JS 객체에 저장하는 것과 비교했을 때 데이터 특성을 읽는 성능도 좋지 않다.


---

참조 사이트

> [mozilla](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes)