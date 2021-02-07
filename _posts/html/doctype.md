---
title:  "HTML - doctype 은 뭔가요?"
search: true
categories: 
  - HTML
tag:
  - HTML
last_modified_at: 2020-08-20T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: doctype
article_section: doctype 은 뭔가요?
meta_keywords: doctype
---

# DOCTYPE 이란?

HTML은 한 종류가 아니라, 여러 종류의 HTML이 있습니다. 

HTML 출력을 올바르게 하기위해 html 파일 최상단에 DOCTYPE 을 선언합니다.

브라우저는 DOCTYPE을 사용하여 페이지 렌더링 방법을 결정합니다. 

[W3C에서 권장하는 DOCTYPE 종류](https://www.w3.org/QA/2002/04/valid-dtd-list.html)

Doctype은 또 Document Type Definition, DTD 라고도 합니다.

---


- 요약 :  DOCTYPE은 문서의 종류를 선언하는 태그



# DOCTYPE이 뭐야 안할래

DOCTYPE 을 선언하지 않는다면 웹 브라우저는 quirks mode 로 읽는다.

quirks mode 는 관용모드 또는 비표준 모드라고 읽는듯 하다.

렌더링 모드에는 네가지 모드가 있는듯 하다.

 - 표준 모드 standard mode  

 - 비표준 모드 quirks mode  

 - 거의 표준 모드 almost standards mode  

 - 엄격 모드 strict mode

 ## 렌더링 모드별로 뭐가 다르죠?

 (업데이트 예정)