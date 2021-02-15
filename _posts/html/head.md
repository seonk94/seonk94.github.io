# `<head></head>`

> [mozilla.org](https://developer.mozilla.org/ko/docs/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML)

HTML의 head는 페이지를 열 때 웹 브라우저에 표시되지 않습니다. head는 `<title>` 같은 페이지나, `CSS의 링크`(HTML 컨텐츠를 CSS로 스타일링하기를 원한다면),  `파비콘`(favicon), 그리고 다른 `메타데이터`(작성자, 중요한 키워드와 같은 HTML에 대한 내용)를 포함합니다. 이 글에서는 위 내용들과 그 이상에 대해 다룰 것입니다. 이것은 head에 있어야하는 마크업이나 다른 코드들을 다루는데 좋은 기초가 될 것입니다.

```html
<head>
    <meta charset="utf-8">
    <title>My test page</title>
</head>
```

## metadata

메타데이터는 데이터를 설명하는 데이터.

HTML에서 [문서](https://developer.mozilla.org/ko/docs/Web/HTML/Element/meta)에 공식적으로 메타데이터를 적용하는 방법이 있습니다.

### charset

charset 는 문서의 character 인코딩을 지정하는것.

```html
<meta charset="utf-8">
```

이 요소는 문서의 character encoding에 대해서 간단히 표시합니다. 

`utf-8` 은 전세계적인 character 집합으로 많은 언어들을 문자들을 포함합니다. 이는 웹 페이지에서 어떤 문자라도 취급할 수 있다는 것을 의미합니다.

### author, description


많은 <meta> 요소가 name 과 content 속성을 가집니다:

- name 은 메타 요소가 어떤 정보의 형태를 갖고 있는지 알려줍니다.

- content는 실제 메타데이터의 컨텐츠입니다.


이러한 두가지 메타데이터는 당신의 페이지에서 관리자를 정리하고 머릿말을 요약하는데 유용합니다.

```html
<meta name="author" content="Chris Mills">
<meta name="description" content="The MDN Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing web sites and applications.">
```

페이지 콘텐츠 관련 키워드를 포함시키는 것은 검색엔진에서 이 페이지가 더 많이 표시 될 가능성이 생기게 할 수 있다. (이러한 활동을 Search Engine Optimization, 또는 [SEO](https://developer.mozilla.org/ko/docs/Glossary/SEO) 라고 함.)

### Open Graph Data

[Open Graph Data](https://ogp.me/) 는 Facebook이 웹 사이트에 더 풍부한 메타 데이터를 제공하기 위해 발명한 메타 데이터 프로토콜이다.

```html
<meta property="og:image" content="https://developer.cdn.mozilla.net/static/img/opengraph-logo.dc4e08e2f6af.png">
<meta property="og:description" content="The Mozilla Developer Network (MDN) provides
information about Open Web technologies including HTML, CSS, and APIs for both Web sites
and HTML5 Apps. It also documents Mozilla products, like Firefox OS.">
<meta property="og:title" content="Mozilla Developer Network">
```


## css, javascript 적용

### css

`<link>`는 항상 문서의 head 부분에 위치한다. 

link 는 두가지 속성을 표시해줘야한다.

- rel : 문서의 스타일 시트임을 나타냄

> rel 속성값 종류
> | 속성값 |		설명 |
> |-------|-----------|
> | alternate |		프린트 페이지나 번역된 페이지와 같이 해당 문서의 대체 버전에 대한 링크를 제공함. |
> | author |		해당 문서의 저자에 대한 링크를 제공함. |
> | dnsprefetch |		브라우저가 대상 리소스의 원본에 대해 DNS 확인(DNS resolution) 작업을 미리 수행하도록 > 명시함. |
> | help |		도움말 문서에 대한 링크를 제공함. |
> | icon |		사용자 인터페이스에서 해당 문서를 나타낼 리소스(일반적으로 아이콘)를 불러옴. |
> | license |		해당 문서의 저작권 정보에 대한 링크를 제공함. |
> | next |		연관된 문서들의 모음 중 다음 문서에 대한 링크를 제공함. |
> | pingback |		현재 문서에 대한 핑백(pingback)을 처리하는 핑백 서버의 주소를 제공함. |
> | preconnect |		브라우저가 대상 리소스의 원본에 미리 연결하도록 명시함. |
> | prefetch |		사용자가 요청할 가능성이 있으므로, 브라우저가 대상 리소스를 미리 가져와 캐시(cache)> 하도록 명시함. |
> | preload |		브라우저가 as 속성과 해당 대상과 관련된 우선순위에 따라 현재 탐색에 사용할 대상 리소스를 > 미리 가져와 캐시하도록 명시함. |
> | prev |		연관된 문서들의 모음 중 이전 문서에 대한 링크를 제공함. |
> | search |		현재 문서 및 관련 페이지를 검색하는데 사용할 수 있는 리소스에 대한 링크를 제공함. |
> | stylesheet |		스타일 시트(stylesheet)로 사용할 외부 리소스를 불러옴. |

- href : 시트 파일의 경로를 포함하는 href

```html
<link rel="stylesheet" href="my-css-file.css">
```


### script

`<script>` 는 head에 들어갈 필요가 없다. 

`</body>` 태그 바로 앞, 문서 본문의 맨 끝에 넣는 것이 좋으며, JavaScript를 적용하기 전에 모든 HTML 내용을 브라우저에서 읽었는지 확인하는 것이 좋다. 

```html
<script src="my-js-file.js"></script>
```