## Ajax

- Ajax(Asynchronous JavaScript And XML) : 전체 페이지가 아닌 페이지의 일부분만 갱신하여 응답성을 향상시키는 기술
- 브라우저에서는 `XMLHttpRequest`라는 API로 Ajax를 사용할 수 있도록 제공

## Ajax의 기반이 되는 기술

### 1. XMLHttpRequest

- 브라우저와 웹 서버 간에 통신을 하여 리소스를 가져올 수 있는 API
- 이름과 상관없이 XML 외 모든 종류의 데이터를 받아올 수 있다.
  - http, ftp, file 같은 프로토콜 또한 사용 가능
- 옵션을 통해 비동기, 동기 방식 모두 처리 가능

```jsx
const xhr = new XMLHttpRequest();
const method = "POST"; // GET, DELETE 또한 사용 가능
const url = "/posts";
xhr.open(method, url);
```

- 인스턴스를 생성 후 open() 메서드를 통해 요청 환경을 구성한다.
- open() 메서드는 필수 매개변수로 http 메서드, 요청 url을 가진다.
- 선택 매개변수로는 요청이 동기인지 비동기인지 확인하는 async와 username, password 같은 매개변수를 넘겨줄 수 있다.

```jsx
const body = JSON.stringify({ userId: "test" });

xhr.send(body); // 요청 보냄
xhr.abort(); // 요청 중지
```

- send() 메서드를 이용해 서버에 요청을 보낸다.
- POST 요청처럼 Request body에 데이터를 담아 전송할 때 send() 메서드를 사용한다.
- abort() 메서드를 이용해 요청을 중지한다.

### 2. 헤더 다루기

- 요청에 헤더를 지정하고 싶다면 `setRequestHeader()`메서드에 이름과 값을 인자로 넘긴다.

```jsx
xhr.setRequestHeader("X-Test", "han");
xhr.setRequestHeader("X-Test", "lee");

// X-Test: han, lee
```

- 단 보안에 문제가 될 수 있는 필드를 수정하는 것은 불가능하다. ex ) Cookie, Host, Connection 등
- 추가할 수는 있지만, 제거하는 것은 불가능하다.

```jsx
xhr.setRequestHeader("Content-Type", "application/json");
```

- HTTP POST 메서드로 통신할 경우 body 데이터 타입은 Content-type 필드를 통해 표현한다.
- Connect-type에 지정한 미디어 타입으로 body 데이터 타입이 결정된다. ex ) application/json, multipart/form-data

```jsx
xhr.getResponseHeader("Content-Type");
xhr.getAllResponseHeaders();
```

- 응답으로 온 헤더 정보가 필요할 경우 `getResponseHeader()`, `getAllResponseHeaders()` 메서드를 사용한다.
- getResponseHeader() : 원하는 헤더 필드의 이름을 인자로 넘겨 값을 가져온다.
- getAllResponseHeaders() : 모든 헤더의 이름과 값을 개행으로 구분한 문자열로 반환한다.

### 3. 이벤트

- 응답을 다루는 이벤트에는
  - loadstate : 요청이 시작될 때 발생
  - progress : 요청한 데이터를 받는 동안 주기적으로 발생
  - load : 요청이 성공적으로 종료되었을 때
  - loadend : 성공 여부와 관계없이 요청 처리 과정이 완료되었을 때
  - 요청 처리에 문제가 발생했을 때 timeout, abort, error 이벤트로 예외를 처리할 수 있다.
  - 단 error는 HTTP 상태 코드가 4xx, 5xx일 때 발생하는 것이 아니라 네트워크 장애 요인으로 요청이 완료되지 못 했을 때 발생

```jsx
xhr.addEventListener("progress", () => {
  console.log();
});

xhr.addEventListender("load", () => {
  console.log(xhr.status); // 상태 코드
  console.log(xhr.response); // 응답 body
});
xhr.addEventListener("error", () => {
  console.log("error");
});
```

- `readyState` 프로퍼티를 통해 현재 요청 상태를 알 수 있다.
  - 객체의 상수와 숫자로 비교할 수 있다.
  - UNSENT(0), OPENED(1), HEADERS_RECEIVED(2), LOADING(3), DONE(4)

```jsx
// readystatechange event : readyState 속성 값이 변할 때 발생
xhr.addEventListener('readystatechange', () -> {
	if(xhr.readyState === xhr.OPENED) {
		// 상수로 readyState 비교
	} else if (xhr.readyState === 3) {
		// 숫자로 비교
	}
});
```

## Promise

```jsx
pourWater(() => {
	bringToBoil() => {
		putRamenAndSoupBase(() => {
			setTimeout(() => {
				turnOffGasFire();
				eatRamen();
			}, 3 * 60 * 1000);
		})
	}
});
```

- 비동기로 코드를 작성하다 보면 예제처럼 중첩된 콜백 함수를 매개변수로 사용하는 경우가 발생..
  - 콜백 지옥(Callback Hell) 혹은 파멸의 피라미드(Pyramid of Doom)이라고 한다.
  - 코드의 들여쓰기가 깊어질수록 이해하기 힘들고, 유지보수가 어렵다.
- ES2015의 `Promise` 문법을 사용하여, 비동기 처리 시점을 명확히 표현할 수 있다.
- **Promise란?** 비동기 작업이 끝난 뒤 결괏값(성공, 실패)를 처리할 수 있는 객체이다.
- Promise 생성자 함수를 통해 인스턴스화 한다.
- 생성자 함수에 전달되는 함수를 실행자(executor)라고 한다.
- 전달된 실행자는 인스턴스가 생성될 때 비동기 작업을 수행한다.

```jsx
const promise = new Promise((resolve, reject) => {
  // 실행자(executor)
  // 비동기 작업 수행. 객체가 만들어질 때 실행.
});
```

- 실행자 함수는 매개변수로 **자바스크립트 함수인 resolve()와 reject()**를 가진다.
- resolve() : 성공했을 때 결괏값과 함께 해당 함수 호출
- reject() : 실패했을 때 에러 객체와 함께 해당 함수 호출
- 실행자에서는 둘 중 하나를 반드시 호출해야 하며, 한 실행자 내에서 중복 호출 시 무시된다.

```jsx
const promise = new Promise((resolve, reject) => {
  resolve(100);
  resolve(200); // 무시
});
```
