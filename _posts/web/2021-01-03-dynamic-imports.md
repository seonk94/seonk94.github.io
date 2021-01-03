---
title: "코드 스플리팅(Code Splitting) 2부 Dynamic Imports"
search: true
categories:
  - WEB
tag:
  - WEB
last_modified_at: 2021-01-03T00:00:00+08:00
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

<b>이 글에서는 `Dynamic Imports`을 알아보려고 한다.</b>

## Dynamic Imports

동적 코드 분할과 관련되어 두 가지 유사한 기술이 웹팩에 의해 지원된다.

첫뻔째 권장되는 방법은 `dynamic imports`에 대한 [`ECMAScript` 제안](https://github.com/tc39/proposal-dynamic-import)에 따른 [import() syntax](https://webpack.js.org/api/module-methods/#import-1)를 사용하는것이다.

레거시 웹팩 접근 관련방식은 [require.ensure](https://webpack.js.org/api/module-methods/#requireensure) 을 사용하는것입니다.

> import() 는 내부적으로는 promises를 사용합니다.
> 이전 브라우저에서 import()를 사용한다면, [es6-promise](https://github.com/stefanpenner/es6-promise) 또는 [promise-polyfill](https://github.com/taylorhakes/promise-polyfill) 과 같은 polyfill을 사용해야합니다.

webpack.config.js

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    index: "./src/index.js",
    // another: './src/another-module.js',
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  /* 아래 코드 제거 */
  optimization: {
    splitChunks: {
      chunks: "all",
    },
  },
};
```

project

```
webpack-demo
|- package.json
|- webpack.config.js
|- /dist
|- /src
  |- index.js
- |- another-module.js
|- /node_modules
```

이제 정적으로 가져오는 대신 lodash를 동적 가져오기하여 chunk를 분리합니다.

src/index.js

```js
//import _ from 'lodash';

//function component() {
function getComponent() {
  const element = document.createElement("div");

  // element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  return import("lodash")
    .then(({ default: _ }) => {
      const element = document.createElement("div");

      element.innerHTML = _.join(["Hello", "webpack"], " ");

      // return element;
      return element;
    })
    .catch((error) => "An error occurred while loading the component");
}

// document.body.appendChild(component());
getComponent().then((component) => {
  document.body.appendChild(component);
});
```

default가 필요한 이유는 웹팩 4 이후로 CommonJS 모듈이 module.export의 값으로 더이상 해결되지 않고, 대신 CommonJS 모듈에 대한 인공 네임스페이스 개체를 생성합니다. 자세한 내용은 다음을 참고하세요. [webpack 4: import() and CommonJs.](https://medium.com/webpack/webpack-4-import-and-commonjs-d619d626b655)

웹팩을 실행하여 Lodash를 별도의 번들로 분리해보겠습니다.

```cmd
...
[webpack-cli] Compilation finished
asset vendors-node_modules_lodash_lodash_js.bundle.js 549 KiB [compared for emit] (id hint: vendors)
asset index.bundle.js 13.5 KiB [compared for emit] (name: index)
runtime modules 7.37 KiB 11 modules
cacheable modules 530 KiB
  ./src/index.js 434 bytes [built] [code generated]
  ./node_modules/lodash/lodash.js 530 KiB [built] [code generated]
webpack 5.4.0 compiled successfully in 268 ms
```

import()가 promise를 반환하므로 비동기 함수와 함께 사용할 수 있습니다.
코드를 단순화하는 방법은 다음과 같습니다.

src/index.js

```js
// function getComponent() {
async function getComponent() {
  const element = document.createElement("div");
  const { default: _ } = await import("lodash");

  // return import('lodash')
  //   .then(({ default: _ }) => {
  //     const element = document.createElement('div');
  element.innerHTML = _.join(["Hello", "webpack"], " ");

  //   element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  //   return element;
  // })
  // .catch((error) => 'An error occurred while loading the component');
  return element;
}

getComponent().then((component) => {
  document.body.appendChild(component);
});
```

> 나중에 계산 변수를 기반으로 특정 모듈을 가져와야할 때 가져오기 위한 [`dynamic expression`](https://webpack.js.org/api/module-methods/#dynamic-expressions-in-import)을 제공할 수 있습니다.

## Prefetching/Preloading modules

webpack 4.6.0+ 는 prefetching, preloading 에 대한 지원글 추가합니다.

imports를 선언하는 동안 이러한 인라인 지시문을 사용하면 웹팩이 다음 사항을 브라우저에 알리는 "Resource Hint"를 출력할 수 있습니다.

- prefetch : 향후 일부 탐색에 리소스가 필요할 수 있습니다.

- preloading : 현재 탐색 중에 리소스도 필요합니다.

간단한 prefetch 예는 `Hompage` 구성 요소를 포함 할 수 있습니다.
구성 요소를 렌더링 한 `LoginPage`다음 요청시 `LoginModal`클릭한 후 구성요소를 로드합니다.

```js
//...
import(/* webpackPrefetch: true */ "./path/to/LoginModal.js");
```

이렇게 하면 `<link rel="prefetch" href="login-modal-chunk.js">` 페이지 헤드에 추가되어 브라우저가 유휴시간에 login-modal-chunk.js파일을 미리 가져오도록 지시합니다.

Preloading 지시문은 prefetch 와 비교하여 여러가지 차이점이 있습니다.

- preloaded chunk는 parent chunk와 병렬로 로드되기 시작합니다. prefetched chunk는 부모 chunk로드가 완료된 후 시작합니다.

- preloaded chunk는 중간 우선 순위를 가지며 즉시 다운로드 됩니다. prefetched chunk는 브라우저가 idle 상태일때 다운로드 됩니다.

- preloaded chunk 는 부모 chunk에 의해 즉시 요청되어야합니다. prefetched chunk는 나중에 언제든지 사용할 수 있습니다.

- 브라우저 지원이 다릅니다.

간단한 preload 예로는 항상 별도의 chunk에 있어야하는 큰 라이브러리에 종속되는 구성요소를 들 수 있습니다.

대형 차트 라이브러리가 필요한 차트 구성 요소를 생각해보면, 로딩화면이 나타나고 즉시 차트 라이브러리 import를 실행합니다.

ChartComponent.js

```js
//...
import(/* webpackPreload: true */ "ChartingLibrary");
```

ChartComponent를 사용하는 페이지가 요청되면 `<link rel="preload">`를 통해서도 차트 라이브러리 chunk 요청이 이루어집니다.

page-chunk가 더 빨리 끝난다고 가정하면 loading 화면이 차트 라이브러리 chunk로드가 완료될때까지 나타날것입니다.

이렇게하면 두번이아니라 한번 왕복하면 되므로 부하 시간이 약간 늘어납니다. 특히 대기시간이 긴 환경에서는 더욱 그렇습니다.

> preload를 잘못 사용하면 실제로 성능이 저하될 수 있으므로 주의하여야합니다.
