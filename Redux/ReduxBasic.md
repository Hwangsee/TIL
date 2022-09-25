### 들어가며...

소프트웨어 개발을 할 때 가장 큰 위협은 복잡성이다.
코드가 복잡해지면 유지보수나 복잡한 기능을 추가할 때, 어떤 결과를 낳을지 예측하기 어려워진다.

Redux는 어떤 결과를 가져올 지 예측가능하게 만들어주는 도구이다.

### 리덕스의 특징

- **Single Source of Trust (하나의 객체를 갖는다.)**
- 리덕스는 애플리케이션의 객체를 한 곳에 중앙집중적으로 관리한다.
- 상태는 Web으로부터 철저히 차단시켜야하는 중요한 데이터이다.
- dispatch, 리덕서로만 state를 변경할 수 있다.
- state의 값이 바뀔 때마다 지령을 내리기 때문에 리덕스를 사용하면 부품에 대한 작업만 신경쓰면 된다.

### 편안하게 UNDO, REDO

- 리덕스는 원본 복제 → 복제한 데이터를 사용한다.
- 각자의 상태 데이터가 독립된 객체를 가지고 있기 때문에 UNDO와 REDO를 편하게 할 수 있다.
- 리덕스 이용 시 현재 상태 뿐만 아니라, 과거 상태를 볼 수 있기 때문에 디버깅에 용이하다.
- 모듈 리로딩을 할 수 있다.
  - 모듈 리로딩? 작성한 코드가 모듈에 바로 반영된다는 뜻.
- 데이터가 그대로 남아 있는 **핫 모듈 리로딩** 기능을 이용할 수 있다.

---

## Redux의 핵심 store

- store에는 모든 것이 저장되며, 실제 값은 state에 저장된다.
- `하지만 state에는 직접 접근이 불가능하다.`
- store를 만들기 위해서는 reducer()라는 함수를 공급해줘야 하는데
  - redux를 만들다 = reducer 함수를 생성하다 라고 봐도 된다.
  ```jsx
  **function reducer(oldState, action) {**
  	// ....
  }
  var store = Redux.createStore(**reducer**);
  ```

## 또다른 함수 render()

- render()는 **현재 state를 반영한 UI를 만들어주는 역할**을 하는, 개발자가 짜야 할 코드이기도 하다.
  ```jsx
  function render() {
    var state = store.getState();
    document.querySelector("#app").innerHTML = `
  <h1>WEB</h1>
  	`;
  }
  ```

### state가 바뀔 때마다 호출하고 싶다면 subscribe()

- 그렇다면 state가 변경될 때마다 render가 변경된다면 얼마나 좋을까?
- subscribe()가 바로 그 역할을 한다.

```jsx
store.subscribe(render);
```

---

- 사용자가 submit 버튼을 클릭 시 dispatch 함수에 객체를 전달하도록 한다.
  - `전달되는 객체를 액션이라고 한다.`
- type이 create 인 액션을 전달한다.
- 액션은 dispatch에 전달되고,

- dispatch는 reducer를 호출하여 state 값을 바꾸는 역할을 한다.
- state가 변경되면 subscribe를 이용해서 render 함수가 호출되면 화면이 갱신된다.

### dispatch가 reducer에 전달하는 값

1. 현재의 state 값
2. action 데이터

reducer는 state를 입력 값으로 받고, action을 참고해서, 새로운 state를 만들어서서 return하는 가공자 역할을 한다.

### 🥕 요약

> Redux는 하나의 객체를 갖는다.
> Redux의 store에는 모든 것이 저장되며, 실제 값은 state에 저장된다.

> store에 있는 state는 직접 접근하는 것이 불가하기 때문에
> `getState()`로 값을 가져오고,
> `dispatch()`로 값을 변경(가공)시키고,
> `subscrbe()`를 이용하여 값이 변경되었을 때 구동될 함수를 등록할 수 있다.

출처 : [생활코딩 - Redux](./https://www.inflearn.com/course/redux-%EC%83%9D%ED%99%9C%EC%BD%94%EB%94%A9/dashboard)
