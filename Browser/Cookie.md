## 쿠키란?

- 서버가 사용자의 웹 브라우저에 전송하는 작은 기록 정보 파일
- 상태가 없는(stateless) HTTP에서 브라우저는 쿠키 정보를 기록하여, 동일 서버 요청 시 쿠키 정보 같이 전송
- 이름-값 쌍으로 정보 기록
  - 쿠키 만료 기간, 도메인, Secure, HttpOnly 등 정보 기록

## 쿠키 만들기

- 서버에서 전송된 HTTP 응답의 Set-Cookie 헤더로 쿠키 생성

```jsx
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: cookie1=value1
Set-Cookie: cookie2=value2

// ...page content
```

- 응답을 통해 쿠키 속성 생성하여 저장

```jsx
GET /page.html HTTP1.1
Host: ...
Cookie: cookie=value; cookie2=value
```

- 이후 전송되는 요청에 쿠키 값 회신

## 쿠키 속성

### 쿠키 삭제

- 세션 종료 시 제거 됨
  - Expires 속성에 날짜가 있거나, Max-age 속성에 기간 명시되어 있을 시 이후 삭제
    (둘 다 있을 경우 Max-age 우선 적용)

### 쿠키 스코프

- Domain과 Path 속성은 쿠키에서 접근할 수 있는 도메인과 URL 경로를 설정할 수 있음
- Domain : 작성 시 서브 도메인을 포함함
  - Domain=google.com일 경우 dev.google.com 같은 서브 도메인도 접근 가능
- Path : 서브 디렉토리 경로에서 쿠키 접근 가능
  - Path=/docs 인 경우, /docs/tutorial, /docs/tutorial/1 URL에서 접근 가능

### Secure와 HttpOnly

- Secure: HTTPS로 통신하는 경우에만 쿠키 전송
- HttpOnly: 클라이언트 측에서 쿠키를 사용할 수 없게 함

## document.cookie

- 접속한 사이트에 관련된 쿠키 정보를 알아냄

```jsx
document.cookie = "name=hwang; age=17; path=/;";
```

- 쿠키 하나가 차지하는 용량은 4KB
- 브라우저에 따라 허용하는 쿠키는 약 20개 내외

## 동일 출처(Same Origin Policy, SOP)

- 동일 출처란? 두 URL의 프로토콜, 포트, 호스트가 모두 같을 때 동일 출처라고 함.
  ```jsx
  // 비교 기준
  https://dev.test.com:80/docs/1
  // 예시
  https://dev.test.com:80/tutorial // 동일출처
  http://dev.test.com:80/docs/1 // 프로토콜 불일치.
  https://dive.com:80/tutorial // 호스트 다름.
  https://dev.test.com:8080/tutorial // 포트 다름.
  ```
- 동일 출처 정책은 브라우저 보안을 위해 잠재적으로 위험이 있는 문서의 접근을 허용하지 않음.
  - 하지만 웹 사이트 기능이 많아지며 외부 API를 사용하거나, 클라이언트와 API 서버를 분리하여 만들게 되며 문제가 발생
- 허용된 출처를 알려줄 수 있는 **HTTP 헤더**가 추가 됨
- **교차 출처 리소스 공유(Cross Origin Resource Sharing)는** HTTP 헤더를 이용해 애플리케이션에서 다른 출처의 리소스에 접근할 수 있도록 권한을 줄 수 있음.

## 교차 출처 리소스 공유 (Cross Origin Resource Sharing, CORS)

- 요청을 보낸 헤더의 Origin 필드와 서버가 보내준 Access-Control-Allow-Origin을 비교해 응답의 유효성을 판단
- CORS 시나리오는 여러가지가 있지만 일반적으로 사전 요청 방식(preflighted)이 가장 많이 사용된다

### 사전 요청(preflighted) 방식

- **OPTIONS HTTP 메서드**를 이용하여 요청을 전송하기 전 안전한 요청인지 허가를 받은 뒤 요청을 보내는 방식
  - 사전 요청은 브라우저에서 자동으로 전송하기 때문에 클라이언트 개발자가 작성하지 않음
- ex ) DELETE 요청을 다른 도메인에서 보낸다고 했을 때,
  ```jsx
  const xhr = new XMLHttpRequest();
  xhr.open("DELETE", "https://other-example.com/docs/1");
  xhr.send();
  ```
  - 브라우저는 사전 요청을 보내 해당 도메인의 DELETE 메서드를 허용하는지 묻는다.
  - Access-Control-Allow-Origin은 서버에서 허용하는 출처가 무엇인지 알려준다.
  - 브라우저는 이를 통해 리소스에 접근할 수 있는지 판단한다.
  ```jsx
  OPTIONS /docs/1
  Access-Control-Request-Method: DELETE
  Access-Control-Request-Header: origin, x-request-with
  Origin: https://www.example.com
  ```
  - 서버가 요청을 허용하면 Access-Control-Request-Method 필드에 허용하는 메서드가 나타나고,
    Access-Control-Allow-Origin 필드에 허용하는 출처가 나타난다.
  ```jsx
  HTTP/1.1 204 No Content
  Connection: keep-alive
  Access-Control-Allow-Origin: https://www.example.com
  Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE
  ```
  - 사전 요청이 마무리되면 실제 DELETE 요청을 보낸다.

### credentials

- 브라우저에서 제공하는 XMLHttpRequest나 fetch()는 출처가 다를 경우 브라우저의 쿠키, 인증 정보를 헤더에 담지 않는다.
- 만약 요청에 담아 보내고 싶다면 credentials 옵션을 변경해야 한다.
  - credentials의 값 :
  - same-origin: 동일 출처라면 보낸다. (디폴트 값)
  - omit: 동일 출처 여부 상관없이 쿠키 전송하지 않음
  - include: 교차 출처라도 보낸다.
- 단, credentials이 include일 때 서버에서 Access-Control-Allow-Origin을 와일드 카드로 설정하면 요청 실패하는 예외적인 상황이 있다.
