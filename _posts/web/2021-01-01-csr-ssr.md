---
title: "WEB - CSR(SPA) - SSR(MPA)"
search: true
categories:
  - WEB
tag:
  - WEB
last_modified_at: 2021-01-01T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: CSR(SPA) - SSR(MPA)
article_section: CSR(SPA) - SSR(MPA)
meta_keywords: CSR(SPA) - SSR(MPA)
---

# CSR(SPA) vs SSR(MPA) 비교

## SSR (Server Side Rendering)

SSR은 사용자가 페이지를 요청할때마다 서버에서 완성된 페이지를 보내주어 렌더링하는 방식.

### 특징

- 검색 엔진 최적화 (SEO)에 용이하다.
- 초기 로딩속도가 빠르지만, 이후 페이지 요청할때마다 서버에 요청하기때문에 UX적으로나, 트래픽이 커질수있다.

---

## CSR (Client Side Rendering)

CSR은 초기에 HTML, CSS, JAVASCRIPT 파일을 내려받고, 자바스크립트를 이용해 동적으로 화면요소를 재
배치하는 방식.

이후에는 필요한 데이터를 요청해 화면에 보여준다.

SPA가 CSR 방식을 사용한다. ( SPA !== CSR )

### 특징

- 처음 요청은 모든 html과 static 파일들을 로드하느라 느릴수 있지만, 이후에는 빠르게 랜더링 된다. (code splitting, Chunking 등을 활용해 어느정도 완화 가능.)

---

{
"order" : "{{strategy.order.action}}",
"contracts" : "{{strategy.order.contracts}}",
"price" : "{{strategy.order.price}}",
"prev_market_position": "{{strategy.prev_market_position}}"
}

## 참고글

[The Benefits of Server Side Rendering Over Client Side Rendering](https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8)

[네이버 D2 SSR](https://d2.naver.com/helloworld/7804182)
