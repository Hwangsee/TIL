> 컴포넌트의 리턴 타입이 `JSX.Element`인 것이 있고 `React.ReactNode`인 것도 있어서 이 두 타입의 차이가 무엇인지 찾아 보았다.  
  
> 추측건대 JSX.Element를 사용했을 때 type 오류가 발생하면 React.ReactNode를 사용한 것 같았다.

## React.ReactNode와 JSX.Element의 관계

- `React.ReactNode` > `ReactElement` > `JSX.Element` 의 포함 관계에 있다.
- JSX.Element의 제네릭 인터페이스가 ReactElement이기 때문에 둘은 묶어서 생각해도 된다.
    - `ReactElement` , `JSX.Element` 유형은 모두 `React.createElement()`/ `jsx()` 함수 호출의 결과이다.

## React.ReactNode

- ReactNode는 ReactElement, ReactFragment, string, number, ReactNodes 타입의  array, null, undefined, boolean이 될 수 있다.
    - Javascritp의 원시 타입 등 React가 렌더링할 수 있는 모든 것을 나타낸다.
- 구체적으로 어떤 타입이 올지 알 수 없거나 어떤 타입도 모두 받고 싶다면 해당 타입으로 지정한다.

```tsx
type ReactText = string | number;
type ReactChild = ReactElement | ReactText;

interface ReactNodeArray extends Array<ReactNode> {}
type ReactFragment = {} | ReactNodeArray;

type ReactNode = ReactChild | ReactFragment | ReactPortal | boolean | null | undefined;
```

## ReactElement

- 가장 기본적인 것이다.
- ReactElement는 type이 지정된 object와 props를 가지고 있다.
    - JavaScript의 원시 타입은 지정할 수 없다.

```tsx
interface ReactElement<P = any, T extends string | JSXElementConstructor<any> = string | JSXElementConstructor<any>> {
    type: T;
    props: P;
    key: Key | null;
}
```

## JSX.Element

- 더 일반적인 것이다. TypeScript의 내부 Hook이다.
- JSX.Element는 ReactElement와 동일하게 설정되며, props 및 type에 대한 유형이 any인 부분에서 차이점이 있다.
    - 다양한 라이브러리에서 JSX를 구현할 수 있도록 더욱 유연성을 제공한다.
- 다양한 라이브러리에서 자체 방식으로 JSX를 구현할 수 있으므로, JSX는 라이브러리에 설정되는 전역 네임스페이스며 다음과 같이 설정한다.

```tsx
declare global {
  namespace JSX {
    interface Element extends React.ReactElement<any, any> { }
  }
}
```

```tsx
	<p> // <- ReactElement = JSX.Element
   <Custom> // <- ReactElement = JSX.Element
     {true && "test"} // <- ReactNode
  </Custom>
 </p>
```

출처 : 

- [https://stackoverflow.com/questions/58123398/when-to-use-jsx-element-vs-reactnode-vs-reactelement](https://stackoverflow.com/questions/58123398/when-to-use-jsx-element-vs-reactnode-vs-reactelement)
- [https://github.com/facebook/react/blob/v17.0.0/packages/shared/ReactElementType.js#L15](https://github.com/facebook/react/blob/v17.0.0/packages/shared/ReactElementType.js#L15)
- [https://dev.to/fromaline/jsxelement-vs-reactelement-vs-reactnode-2mh2](https://dev.to/fromaline/jsxelement-vs-reactelement-vs-reactnode-2mh2)