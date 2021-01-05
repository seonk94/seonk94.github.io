---
title: "코드 스플리팅(Code Splitting) 4부 React"
search: true
categories:
  - WEB
tag:
  - WEB
last_modified_at: 2021-01-05T00:00:00+08:00
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

<b>이 글에서는 React에서 활용하는 방법을 알아보려고 한다.</b>

## Dynamic import()

Before

```js
import { add } from "./math";

console.log(add(16, 26));
```

After

```js
import("./math").then((math) => {
  console.log(math.add(16, 26));
});
```

## React.lazy()

> React.lazy와 Suspense는 아직 서버 사이드 렌더링을 할 수 없습니다. 서버에서 렌더링 된 앱에서 코드분할을 하기 원한다면 [Loadable Components](https://github.com/gregberge/loadable-components를 추천합니다. 이는 [서버 사이드 렌더링과 번들 스플리팅에 대한 좋은 가이드](https://loadable-components.com/docs/server-side-rendering/)입니다.

React.lazy() 함수를 사용하면 동적 import를 사용해서 컴포넌트를 렌더링 할 수 있습니다.

Before

```js
import OtherComponent from "./OtherComponent";
```

After

```js
// in MyComponent
const OtherComponent = React.lazy(() => import("./OtherComponent"));
```

MyComponent가 처음 렌더링 될 때 OtherComponent를 포함한 번들을 자동으로 불러옵니다.

React.lazy는 동적 import()를 호출하는 함수를 인자로 가집니다. 이 함수는 React컴포넌트를 포함하며 default export를 가진 모듈로 결정되는 Promise를 반환해야 합니다.

lazy컴포넌트는 Suspense 컴포넌트 하위에서 렌더링 되어야 하며, Suspense는 lazy컴포넌트가 로드되길 기다리는 동안 로딩화면과 같은 UI를 보여줄수 있게 해야합니다.

```js
import React, { Suspense } from "react";

const OtherComponent = React.lazy(() => import("./OtherComponent"));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

fallback prop은 컴포넌트가 로드될 때까지 기다리는 동안 렌더링하려는 React Element를 받아들입니다. Suspense 컴포넌트는 lazy컴포넌트를 감쌉니다. 하나의 Suspense 컴포넌트로 여러 lazy컴포넌트를 감쌀 수도 있습니다.

```js
import React, { Suspense } from "react";

const OtherComponent = React.lazy(() => import("./OtherComponent"));
const AnotherComponent = React.lazy(() => import("./AnotherComponent"));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```

네트워크 장애 같은 이유로 다른 모듈을 로드에 실패할 경우 에러를 발생시킬 수 있습니다.

이때 [Error Boundaries](https://ko.reactjs.org/docs/error-boundaries.html)를 이용하여 사용자의 경험과 복구 관리를 처리할 수 있습니다. Error Boundary를 만들고 lazy 컴포넌트를 감싸면 네트워크 장애가 발생했을 때 에러를 표시할 수 있습니다.

```js
import React, { Suspense } from "react";
import MyErrorBoundary from "./MyErrorBoundary";

const OtherComponent = React.lazy(() => import("./OtherComponent"));
const AnotherComponent = React.lazy(() => import("./AnotherComponent"));

const MyComponent = () => (
  <div>
    <MyErrorBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </MyErrorBoundary>
  </div>
);
```

## Route based

앱에 코드 분할을 어느 곳에 도입할지 결정하는 것은 조금 까다롭습니다. 여러분은 사용자의 경험을 헤치지 않으면서 번들을 균등하게 분배할 곳을 찾고자 합니다.

이를 시작하기 좋은 장소는 라우트입니다. 웹 페이지를 불러오는 시간은 페이지 전환에 어느 정도 발생하며 대부분 페이지를 한번에 렌더링하기 때문에 사용자가 페이지를 렌더링하는 동안 다른 요소와 상호작용하지 않습니다.

React.lazy를 React Router 라이브러리를 사용해서 애플리케이션에 라우트 기반 코드 분할을 설정하는 예시입니다.

```js
import React, { Suspense, lazy } from "react";
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";

const Home = lazy(() => import("./routes/Home"));
const About = lazy(() => import("./routes/About"));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
      </Switch>
    </Suspense>
  </Router>
);
```

## Named Exports

React.lazy는 현재 default exports만 지원합니다. named exports를 사용하고자 한다면 default로 이름을 재정의한 중간 모듈을 생성할 수 있습니다. 이렇게 하면 tree shaking이 계속 동작하고 사용하지 않는 컴포넌트는 가져오지 않습니다.

```js
// ManyComponents.js
export const MyComponent = /* ... */;
export const MyUnusedComponent = /* ... */;
```

```js
// MyComponent.js
export { MyComponent as default } from "./ManyComponents.js";
```

```js
// MyApp.js
import React, { lazy } from "react";
const MyComponent = lazy(() => import("./MyComponent.js"));
```

---

[출처](https://ko.reactjs.org/docs/code-splitting.html)
