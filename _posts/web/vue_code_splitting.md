---
title: "코드 스플리팅(Code Splitting) 3부 Vue"
search: true
categories:
  - WEB
tag:
  - WEB
last_modified_at: 2021-01-04T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: 코드 스플리팅(Code Splitting)
article_section: 코드 스플리팅(Code Splitting)
meta_keywords: 코드 스플리팅(Code Splitting)
---

# 코드 스필리팅.

[출처](https://webpack.js.org/guides/code-splitting/#entry-points)

코드를 다양한 chunk로 나눌수 있으며 필요에따라 로드할 수 있다.

페이지 렌더링에 필요한 초기 응답 크기를 줄이는데 도움이 된다.

Webpack 문서에 따르면 세가지의 일반적인 방식이 있다고 한다.

- Entry Points: Manually split code using entry configuration.

  > (entry 구성을 사용하서 수동으로 분할.)

- Prevent Duplication: Use Entry dependencies or SplitChunksPlugin to dedupe and split chunks.

  > (Entry dependencies 또는 SplitChunksPlugin 을 사용하여 chunk를 중복제거 및 분할 )

- Dynamic Imports: Split code via inline function calls within modules.
  > (모듈 내 인라인 함수 호출을 통해 코드 분할)

그럼 주로 사용하는 Vue, React에서는 어떻게 활용할수 있을까?

<b>이 글에서는 Vue에서 활용하는 방법을 알아보려고 한다.</b>

## 라우터 코드 분할

```js
const routes = [
  {
    page: "/home",
    name: "home",
    component: () => import(/* webpackChunkName: "home */ "./home.vue"),
  },
  {
    page: "/login",
    name: "login",
    component: () => import(/* webpackChunkName: "login */ "./login.vue"),
  },
];
```

## 컴포넌트 호출

```vue
<template>
  <div>
    <button @click="show = true">Open Modal</button>
    <lazy-modal v-if="show" />
  </div>
</template>
<script>
const LazyModal = () => import(/* webpackChunkName: "modal" */ "./modal.vue");

export default {
  components: {
    LazyModal,
  },
  setup() {
    const show = ref(false);

    return {
      show,
    };
  },
};
</script>
```
