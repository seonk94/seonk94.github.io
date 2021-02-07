---
title: "코드 스플리팅(Code Splitting) 1부"
search: true
categories:
  - WEB
tag:
  - WEB
last_modified_at: 2021-01-02T00:00:00+08:00
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

<b>이 글에서는 `Entry Points`, `Prevent Duplication`을 알아보려고 한다.</b>

## Entry Points

이것은 코드를 분할하는 가장 쉽고 직관적인 방법입니다.
하지만, 더 수동적이고, 우리가 검토할 함정이 있다.
이제 메인 번들에서 다른 모듈을 어떻게 분할할 수 있는지 살펴보자.

project

```
webpack-demo
|- package.json
|- webpack.config.js
|- /dist
|- /src
  |- index.js
+ |- another-module.js
|- /node_modules
```

another-module.js

```js
import _ from "lodash";

console.log(_.join(["Another", "module", "loaded!"], " "));
```

webpack.config.js

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    index: "./src/index.js",
    another: "./src/another-module.js",
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
};
```

이렇게 하면 다음과 같은 빌드 결과가 나타납니다.

```cmd
...
[webpack-cli] Compilation finished
asset index.bundle.js 553 KiB [emitted] (name: index)
asset another.bundle.js 553 KiB [emitted] (name: another)
runtime modules 2.49 KiB 12 modules
cacheable modules 530 KiB
  ./src/index.js 257 bytes [built] [code generated]
  ./src/another-module.js 84 bytes [built] [code generated]
  ./node_modules/lodash/lodash.js 530 KiB [built] [code generated]
webpack 5.4.0 compiled successfully in 245 ms
```

앞서 언급한 바와 같이 이 접근방식에는 몇가지 함정이 있습니다.

항목 chunk 사이에 중복된 모듈이 있는 경우 두 번들에 모두 포함됩니다.

이 기능은 유연성이 떨어지며 핵심 어플리케이션 로직으로 코드를 동적으로 분할하는데 사용할 수 없습니다.
lodash도 ./src/index.js 내에서 가져오므로 두 가지 사항 중 첫 번째 사항이 두 번들에서 모두 중복되기 때문에 이 예에서는 분명 문제가 됩니다. 다음 섹션에서 중복을 제거하겠습니다.

## Prevent Duplication

### Entry dependencies

[dependOn 옵션](https://webpack.js.org/configuration/entry-context/#dependencies)을 사용하면 chunk 간에 모듈을 공유할 수 있습니다.

webpack.config.js

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    index: {
      import: "./src/index.js",
      dependOn: "shared",
    },
    another: {
      import: "./src/another-module.js",
      dependOn: "shared",
    },
    shared: "lodash",
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
};
```

단일 HTML 페이지에서 여러 진입점을 사용하려면 optimization.runtimeChunk: 'single' 도 필요합니다. 그렇지 않으면 [여기](https://bundlers.tooling.report/code-splitting/multi-entry/)에서 문제가 발생할 수 있습니다.

webpack.config.js

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    index: {
      import: "./src/index.js",
      dependOn: "shared",
    },
    another: {
      import: "./src/another-module.js",
      dependOn: "shared",
    },
    shared: "lodash",
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  /* optimization runtimeChunk 추가 */
  optimization: {
    runtimeChunk: "single",
  },
};
```

빌드결과는 다음과 같습니다.

```cmd
...
[webpack-cli] Compilation finished
asset shared.bundle.js 549 KiB [compared for emit] (name: shared)
asset runtime.bundle.js 7.79 KiB [compared for emit] (name: runtime)
asset index.bundle.js 1.77 KiB [compared for emit] (name: index)
asset another.bundle.js 1.65 KiB [compared for emit] (name: another)
Entrypoint index 1.77 KiB = index.bundle.js
Entrypoint another 1.65 KiB = another.bundle.js
Entrypoint shared 557 KiB = runtime.bundle.js 7.79 KiB shared.bundle.js 549 KiB
runtime modules 3.76 KiB 7 modules
cacheable modules 530 KiB
  ./node_modules/lodash/lodash.js 530 KiB [built] [code generated]
  ./src/another-module.js 84 bytes [built] [code generated]
  ./src/index.js 257 bytes [built] [code generated]
webpack 5.4.0 compiled successfully in 249 ms
```

보시다시피 또 공유된 파일 이외에 또 다른 파일이 생성됩니다. 'shared.bundle.js', 'runtime.bundle.js'.

웹팩에서는 페이지당 여러 진입점을 사용할 수 있지만, 가능하면 여러 imports `entry: { page: ['./analytics', './app'] }` 가 있는 진입점을 사용하지 않도록 해야합니다.

따라서 비동기 스크립트 태그를 사용할 때 더 나은 최적화 및 일관된 실행 순서가 생성됩니다.

### SplitChunksPlugin

SplitChunksPlugin을 사용하면 공통 dependency 을 기본 항목 chunk 또는 새로운 chunk로 추출할 수 있습니다. 이를 사용하여 이전 예시에서 lodash dependency 을 중복 제거해보겠습니다.

webpack.config.js

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    index: "./src/index.js",
    another: "./src/another-module.js",
  },
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
  /* optimization 추가 */
  optimization: {
    splitChunks: {
      chunks: "all",
    },
  },
};
```

빌드 결과

```cmd
...
[webpack-cli] Compilation finished
asset vendors-node_modules_lodash_lodash_js.bundle.js 549 KiB [compared for emit] (id hint: vendors)
asset index.bundle.js 8.92 KiB [compared for emit] (name: index)
asset another.bundle.js 8.8 KiB [compared for emit] (name: another)
Entrypoint index 558 KiB = vendors-node_modules_lodash_lodash_js.bundle.js 549 KiB index.bundle.js 8.92 KiB
Entrypoint another 558 KiB = vendors-node_modules_lodash_lodash_js.bundle.js 549 KiB another.bundle.js 8.8 KiB
runtime modules 7.64 KiB 14 modules
cacheable modules 530 KiB
  ./src/index.js 257 bytes [built] [code generated]
  ./src/another-module.js 84 bytes [built] [code generated]
  ./node_modules/lodash/lodash.js 530 KiB [built] [code generated]
webpack 5.4.0 compiled successfully in 241 ms
```

optimization.splitChunks을 사용하면 index.bundle.js, another.bundle.js 에서 중복 dependency가 제거되는것을 볼 수 있습니다.

플러그인은 우리가 lodash를 다른 chunk로 분리한 것을 알아채고 main bundle 에서 제거해줍니다.
