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
