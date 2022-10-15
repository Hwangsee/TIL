2019년 React v16.8부터 Hook이 등장하였고 이후 함수형 컴포넌트에서 state를 다루거나 컴포넌트가 마운트, 언마운트 될 때 추가 작업을 수행할 수 있게 되었다. 리액트 공식문서에서도 클래스형 컴포넌트보다 함수형 컴포넌트를 사용할 것을 권장하고 있다.

하지만 아직 클래스형 컴포넌트를 사용하는 곳이 있기 때문에 유지보수를 하거나 클래스형 컴포넌트를 지원하는 라이브러리를 위해서라도 두 가지 방식을 모두 알고 있어야 한다.

### 함수형 컴포넌트

```tsx
import React from 'react';

function Hello({ color, name, isSpecial }) {
  return (
    <div style={{ color }}>
      {isSpecial && <b>*</b>}
      안녕하세요 {name}
    </div>
  );
}

Hello.defaultProps = {
  name: '이름없음'
};

export default Hello;
```

### 클래스형 컴포넌트

- `class` 키워드를 사용한다.
- `Component`로 상속을 받아야 한다.
- `render()` 메서드가 꼭 있어야 한다.
- 메서드에서 렌더링하고 싶은 JSX를 반환하고, props를 조회할 때는 this.props를 조회하면 된다.

```tsx
import React, { Component } from 'react';

class Hello extends Component {
  render() {
    const { color, name, isSpecial } = this.props;
    return (
      <div style={{ color }}>
        {isSpecial && <b>*</b>}
        안녕하세요 {name}
      </div>
    );
  }
}

// static 키워드와 함께 선언할 수도 있다.
Hello.defaultProps = {
  name: '이름없음'
};

export default Hello;
```

### 클래스형 함수형의 차이점은?

- 클래스형
    - React에서 확장이 필요하며, 컴포넌트를 만들고 React 요소를 반환하는 렌더 함수를 만든다.
    - React 수명 주기 메서드(ex : componentDidMount)는 기능 구성 요소 내에서 사용할 수 있다.
    - 메모리 자원을 함수형 컴포넌트보다 더 사용한다.
    - 상태를 저장해야 하므로 생성자가 사용된다.

- 함수형
    - props를 인자로 받으며 React 요소(JSX)를 반환하는 일반 JavaScript 순수 함수이다.
    - React 수명 주기 메서드는 기능 구성 요소 내에서 사용할 수 없다. (Hook을 통해 해결)
    - 메모리 자원을 클래스형보다 덜 사용한다.
    - 생성자가 사용되지 않는다.

### state 사용 차이

- 클래스형
    
    ```tsx
    import React, { Component } from "react";
    
    class App extends Component {
      constructor(props) {
        super(props);
        this.state = {
          name: "choi",
        };
      }
    
      render() {
        return (
          <>
            <div>{this.state.name}</div>
            <button onClick={()=>this.setState({ name: "kim" })}>버튼</button>
          </>
        );
      }
    }
    
    export default App;
    ```
    
    - constructor 안에서 this.state 초기화 값을 설정한다.
    - state는 객체 형식이다
    - this.setState()를 사용하여 state 값을 변경한다.
    
    > **super(props)는 어디서 등장한 건가요?**
    React.Component를 상속한 컴포넌트의 생성자를 구현할 때는 다른 구문에 앞서 super(props)를 호출해야 한다.
    this.props가 정의되지 않아 버그로 이어질 수 있다.
    > 
- 함수형
    - useState Hook으로 state를 사용한다.

### props 사용 차이

- 클래스형
    - this.prop로 값을 불러올 수 있다.
- 함수형
    - props를 바로 호출할 수 있다.

### LifeCycle 차이

- 클래스형
    - React 수명주기 메서드를 사용하여 LifeCycle을 구현한다.
- 함수형
    - useEffect Hook을 사용하여 LifeCycle을 별도로 구현한다.