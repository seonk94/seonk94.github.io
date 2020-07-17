---
title:  "Webpack 알아보기 - Concept"
search: true
categories: 
  - Webpack
tag:
  - Webpack
last_modified_at: 2020-07-17T00:00:00+08:00
toc: true
toc_sticky: true
toc_label: 목차
article_tag1: Webpack
article_section: Webpack 알아보기
meta_keywords: Webpack,webpack
---
# Webpack 이란?

핵심적으로 webpack 은 최신 JavaScript 응용 프로그램을위한 정적 모듈 번들러 입니다 . 

웹팩은 애플리케이션을 처리 할 때 프로젝트에 필요한 모든 모듈을 매핑하고 하나 이상의 번들을 생성 하는 종속성 그래프를 내부적으로 빌드합니다 .

---

# Core Concepts 알아보기 

---

## Entry

진입점은 내부 종속성 그래프를 구축하기 위해 어떤 모듈 웹 팩을 사용해야 하는지를 나타낸다. 웹 팩은 진입점이 어떤 다른 모듈과 라이브러리에 의존하는지 알아낼 것이다(직접 및 간접).

기본적으로 이 값은 `./src/index.js` 이지만 웹 팩 구성에서 항목 속성을 설정하여 다른(또는 여러 개의 진입점)을 지정할 수 있다. 예를 들면 다음과 같다.

<b>webpack.config.js</b>
```js
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```

## Output

출력 속성은 웹 팩에 자신이 생성하는 번들을 내보내는 위치와 이러한 파일의 이름을 지정하는 방법을 알려준다. 기본값은 주 출력 파일의 경우 `./dist/main.js`로, 생성된 다른 파일의 경우 `./dist` 폴더로 기본 설정된다.

구성에서 출력 필드를 지정하여 프로세스의 이 부분을 구성할 수 있다.

<b>webpack.config.js<b/>
```js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

위의 예에서 우리는 출력을 사용한다.웹 팩에 당사 번들의 이름과 원하는 위치를 알려주는 파일 이름 및 `output.path` 속성. 상단에서 가져오는 경로 모듈이 궁금할 경우, 파일 경로 조작에 사용되는 핵심 `Node.js` 모듈이다.


## Loaders

웹팩은 즉석에서 자바스크립트와 JSON 파일만 이해한다. 

로더는 웹 팩이 다른 유형의 파일을 처리하고 응용 프로그램에서 사용하고 종속성 그래프에 추가할 수 있는 유효한 모듈로 변환할 수 있도록 허용한다.

높은 수준에서 로더는 웹 팩 구성에서 두 가지 속성을 가진다.

1. 테스트 속성은 변환해야 하는 파일 또는 파일을 식별한다.
2. 사용 속성은 변환을 위해 사용해야 하는 로더를 나타낸다.

<b>webpack.config.js</b>
```js
const path = require('path');

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  }
};
```

위의 구성은 테스트와 사용이라는 두 가지 필수 속성을 가진 단일 모듈에 대한 규칙 속성을 정의했다. 이것은 웹팩의 컴파일러에게 다음을 말해준다.


## Plugins

로더가 특정 유형의 모듈을 변환하는 데 사용되는 반면, 플러그인을 활용하여 번들 최적화, 자산 관리 및 환경 변수 주입과 같은 광범위한 작업을 수행할 수 있다.


플러그인을 사용하려면 `require()`를 사용하여 플러그인 `Array` 에 추가해야 한다. 대부분의 플러그인은 옵션을 통해 사용자 정의할 수 있다. 

플러그인은 다른 용도로 구성에서 여러 번 사용할 수 있으므로 새 운영자와 함께 호출하여 인스턴스를 생성하십시오.

<b>webpack.config.js<b/>
```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins

module.exports = {
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};
```

위의 예에서 html-webpack-plugin은 생성된 모든 번들을 자동으로 주입하여 응용 프로그램에 대한 HTML 파일을 생성한다.

## Mode

모드 매개 변수를 개발, 생산 또는 없음으로 설정하여 각 환경에 해당하는 웹 팩의 내장 최적화를 활성화할 수 있다. 

기본값은 `production`이다.

```js
module.exports = {
  mode: 'production'
};
```

## Browser Compatibility

웹 팩은 ES5 호환 브라우저를 모두 지원한다(IE8 이하에서는 지원되지 않음). 웹팩은 `import()` 와 `require.ensure()` 에 대한 `Promise()`를 필요로 한다. 

이전 브라우저를 지원하려면 이러한 식을 사용하기 전에 폴리필을 로드해야 한다.


---

[출처 Webpack doucment](https://webpack.js.org/concepts/)

[번역 Papago](https://papago.naver.com/)