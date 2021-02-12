# React.memo ?

> [공식홈페이지](https://ko.reactjs.org/docs/react-api.html#reactmemo)

```ts
const MyComponent = React.memo(function MyComponent(props) {
  /* props를 사용하여 렌더링 */
});
```

React.memo는 [고차 컴포넌트(Higher Order Component)](https://ko.reactjs.org/docs/higher-order-components.html)입니다.

당신의 컴포넌트가 동일한 props로 동일한 결과를 렌더링해낸다면, React.memo를 호출하고 결과를 메모이징(Memoizing)하도록 래핑하여 경우에 따라 성능 향상을 누릴 수 있습니다. 즉, React는 컴포넌트를 렌더링하지 않고 마지막으로 렌더링된 결과를 재사용합니다.

React.memo는 `props 변화`에만 영향을 줍니다. React.memo로 감싸진 함수 컴포넌트 구현에 useState 또는 useContext 훅을 사용한다면, 여전히 state나 context가 변할 때 다시 렌더링됩니다.

props가 갖는 복잡한 객체에 대하여 얕은 비교만을 수행하는 것이 기본 동작입니다. 다른 비교 동작을 원한다면, 두 번째 인자로 별도의 비교 함수를 제공하면 됩니다.

```ts
function MyComponent(props) {
  /* props를 사용하여 렌더링 */
}
function areEqual(prevProps, nextProps) {
  /*
  nextProp가 prevProps와 동일한 값을 가지면 true를 반환하고, 그렇지 않다면 false를 반환
  */
}
export default React.memo(MyComponent, areEqual);
```

이 메서드는 오직 성능 최적화를 위하여 사용됩니다. 렌더링을 “방지”하기 위하여 사용하지 마세요. 버그를 만들 수 있습니다.

{% hint style="working" %}
주의
class 컴포넌트의 shouldComponentUpdate() 메서드와 달리, areEqual 함수는 props들이 서로 같으면 true를 반환하고, props들이 서로 다르면 false를 반환합니다. 이것은 shouldComponentUpdate와 정반대의 동작입니다.
{% endhint %}

---
