---
title:  "HTML - Web Storage "
search: true
categories: 
  - HTML
tag:
  - HTML
last_modified_at: 2020-08-23T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Storage, Cookies, Session
article_section: Storage, Cookies, Session
meta_keywords: Storage, Cookies, Session
---

# Storage, Cookies.. 왜 사용하는건가요?

HTTP 프로토콜 특징중 Connectionless, Stateless 가 있다고 한다.


- Connectionless 

    - 클라이언트가 서보와 한번 연결을 맺은 후에 클라이언트 요청에 대해 서버가 응답을 끝내면 연결을 끊어버림

    > 연결을 유지할수는 없나요? :  keepalive는 지정된 시간동안 서버와 클라이언트 사이에서 패킷 교환이 없을경우, 상대방이 살아있는지 패킷을 주기적으로 보내 확인합니다. 반응이없으면 연결을 끊게됩니다.


- Stateless ??

    - 통신이 끝나면 상태정보를 연결하지 않는다.

위 특징들을 보완하기 위해, 쿠키 및 세션을 사용한다.

# Storage, Cookies 뭐가 다른가요?

## Cookies


>[whatarecookies.com](http://www.whatarecookies.com/)에 따르면 웹사이트에 의해 유저의 컴퓨터에 놓여지는 최대 4KB의 용량을 가질수 있는 텍스트 파일들이라고 한다.

`An HTTP cookie (web cookie, browser cookie) is a small piece of data that a server sends to the user's web browser. The browser may store it and send it back with later requests to the same server. Typically, it's used to tell if two requests came from the same browser — keeping a user logged-in, for example. It remembers stateful information for the stateless HTTP protocol.`

HTTP 쿠키는 서버가 사용자의 웹 브라우저에 보내는 작은 데이터 조각이다.

브라우저는 그것을 저장하고 나중에 같은 서버에 요청과 함께 그것을 다시 보낼 수 있다.

일반적으로 동일한 브라우저에서 두 개의 요청이 수신되었는지 여부를 확인하는데 사용된다고 한다. (ex: 사용자 로그인 유지)

쿠키는 stateless HTTP protocol 에 대한 상태 저장 정보를 기억한다.

쿠키는 주로 세 가지 목적으로 사용된다.

- 세션 관리 - 로그인, 장바구니 또는 서버가 기억해야하는 기타 사항

- 개인화 - 사용자 기본 설정, 테마 및 기타 설정

- 추적 - 사용자 행동 기록 및 분석


`
쿠키는 한때 일반적인 클라이언트 측 저장에 사용되었습니다. 이것이 클라이언트에 데이터를 저장하는 유일한 방법이었을 때는 합법적 이었지만 이제는 최신 스토리지 API를 사용하는 것이 좋습니다. 쿠키는 모든 요청과 함께 전송되므로 성능이 저하 될 수 있습니다 (특히 모바일 데이터 연결의 경우). 클라이언트 스토리지를위한 최신 API는 Web Storage API ( localStorage및 sessionStorage)와 IndexedDB 입니다.
`


---



Cookies는 두가지 유형이 있다.

- Session Cookies

  - Session Cookies는 만료일을 포함하지 않는다. 

  - 만료일이 없는대신 브라우저나 탭이 열려있는 동안에만 유지된다.

  - 닫힐경우에는 영구적으로 삭제된다고한다.

- Persistent Cookies

  - Persistent Cookies 는 만료일을 가진다고 한다.

  - 만료일 까지 유저의 디스크에 저장되어있다가, 만료일이 지난후에 삭제된다.


## Storage

웹 스토리지 API는 브라우저가 키/값 쌍을 쿠키 사용보다 훨씬 직관적인 방식으로 저장할 수 있는 메커니즘을 제공한다.

Web Storage 내의 두가지 메커니즘은 다음과 같습니다.

### SessionStorage

페이지 세션 기간동안 사용할 수 있는 각 origin에 대해 별도의 저장 영역을 유지 관리 (브라우저가 열려 있는 한 페이지 다시 로드 및 복원 포함)

- 세션에 대한 데이터만 저장, 즉 브라우저 or 탭 이 닫힐 때 까지 데이터가 저장된다는 의미다.

- 데이터는 서버로 전송되지 않는다.

- 저장한도가 Cookies 보다 크다 (최대 5MB)

### LocalStorage

localStorage는 동일한 작업을 수행하지만 브라우저를 닫고 열어도 계속 유지된다.

- 만료 날짜 없이 데이터를 저장하고 JS를 통해서 지울 수 있다..

- 저장 한도는 3개 (Cookies, SessionStorage, LocalStorage)중 최대다.

### Storage 사용법

```js
const sessionStorage = window.sessionStorage;
const localStorage = window.localStorage;
```

```js
const localStorage = window.localStorage;

localStorage.setItem('key', 'value');

localStorage.length // 1

localStorage.key(0) // 'key' 

localStorage.getItem('key') // value

localStorage.removeItem('key')

localStorage.clear()
```