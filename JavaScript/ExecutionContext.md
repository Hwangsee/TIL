## 실행 컨텍스트(ExecutionContext)란?

---

자바스크립트 코드를 실행할 때 필요한 정보들을 저장하고 제공하는 환경을 뜻한다.

## 실행 컨텍스트의 구성 요소

---

- 실행 컨텍스트는 `Lexical Enviroment(LE)`와 `Variable Environment(VE)` 두 가지 컴포넌트로 구성된다.
- LE, BE는 `Environment Records(ER)` 이라고 불리는 형태로 구성된다.

## Environment Records와 스코프 체인

---

- ER은 렉시컬 스코프를 기반으로 `특정 변수와 함수에 대한 식별자 연결 정보를 저장`한다.
- ER이 생성되는 예시
  - 함수 선언, 블록문, try, catch절과 같은 구문이 평가될 때 식별자 바인딩을 위해 ER이 생성된다.
- 모든 ER에는 OuterEnv 필드가 있으며, 이 필드는 `상위 렉시컬 스코프에 대한 ER을 참조`한다.
  - 전역 컨텍스트에서는 상위 컨텍스트가 없기 때문에 필드 값이 null이다.

```jsx
function foo() {
  var a = 1;
  function bar() {
    console.log(a); // 1
  }
  bar();
}
foo();
```

## Lexical Environment와 Varibale Environment

---

- LE와 VE는 모두 ER에 바인딩된 정보를 찾을 수 있는 컴포넌트이다.
- var 키워드는 VE, 이외는 LE를 통해 찾는다.

## 실행 컨텍스트의 생성 과정

---

```jsx
console.log("global context");

function foo() {
  console.log("foo context");
}

function bar() {
  foo();
  console.log("bar context");
}
bar();

// 결과
// global context
// foo context
// bar context
```

1. 전역 실행 컨텍스트가 생성되어 제어권을 가져간다.
2. ‘global context’가 출력된다.
3. 전역 실행 컨텍스트에서 bar() 함수가 호출되어 새로운 실행 컨텍스트가 생성되어 스택에 쌓이고 bar 컨텍스트가 제어권을 가져간다.
4. 'bar context'가 출력된다.
5. foo() 함수의 실행 컨텍스트가 생성되고, 제어권을 가지며 'foo context'가 출력된다.
6. foo() 함수 안의 코드 실행 후 실행 컨텍스트가 완료되어 소멸된다.
7. bar() 실행 컨텍스트가 차례대로 소멸된다.
8. 전역 실행 컨텍스트까지 실행이 완료되면 모든 실행이 완료된다.
