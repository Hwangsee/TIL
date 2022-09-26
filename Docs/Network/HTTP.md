## HTTP란?

- HTML과 같은 **하이퍼미디어 문서를 전송하기 위한 프로토콜**
  - 프로토콜은 데이터가 전송되는 방식을 결정하는 규약
- 주로 TCP 사용 (HTTP3는 UDP)
- 기본 포트는 80
- http:로 시작하는 URL로 송수신하는 데이터에 접근 가능
- HTTP 통신은 상태(state) 개념이 없다
  - 요청(request)과 응답(response)는 각각 독립적으로 처리된다.

### HTTP 통신의 장점

- 서버 디자인의 단순화
- 송수신되는 정보 처리를 위한 공간을 동적 할당 (제거할 필요가 없음)

### HTTP 통신의 단점

- 요청마다 필요한 정보를 모두 담아 보내야 함

### HTTPS(HyperText Transfer Protocol over SSL)

- HTTP보다 보안이 강화된 통신 방법
- 기본 포트 443

## HTTP 응답 상태 코드

- 1xx(Infomational): 서버에서 요청 수신. 처리 중 혹은 정보를 알려줄 필요가 없을 때
- 2xx(Successful): 요청이 성공적으로 완료
- 3xx(Redirection): 리다이렉션 같이 추가 동작 취할 때
- 4xx(Client Error): 요청에 잘못된 구문이 있거나 수행할 수 없을 경우
  - 400(Bad Request): 요청 오류. 잘못된 문법으로 서버가 응답을 이해하지 못함
  - 401(Unauthorized): 권한 없음. 요청 받기 위해 인증 받아야 함
  - 403(Forbidden): 접근 금지
  - 404(Not Found): 서버가 요청받은 리소스가 없을 때 반환 (잘못된 URL 접근, 리소스가 없을 때)
- 5xx(Server Error): 서버가 유효한 요청을 수행하지 못했을 때 반환
  - 500(Internal Server Error): 서버가 처리 방법을 모를 경우
  - 502(Bad Gateway): 게이트웨이나 프록시 작업 시 잘못된 응답을 수신했을 때
  - 503(Service Unabailable): 서버가 요청을 처리할 준비가 되지 않은 경우 (서버 과부화, 배포 도중 서비스 중단 등)

## HTTP 메서드가 갖는 특징

- 요청 메서드를 통해 웹 서버에 전송할 HTTP 요청의 목적, 종류를 나타낼 수 있음.
- 총 9가지의 메서드가 있다.

### safe

- **서버의 상태를 변경하지 않는 메서드의 경우 “safe”** 하다고 함.
- 대표적으로 GET, HEAD, OPTIONS 메서드가 있다.

### 멱등성(Idempotent)

- **동일한 요청을 한 번 보낼 때와 여러 번 연속으로 보낼 때 같은 동작을 하고, 서버의 상태 또한 동일할 때 멱등성을 갖는다**고 한다.
- 모든 safe한 메서드는 멱등성을 갖는다
- 하지만 멱등성을 가진 메서드가 모두 safe하지는 않다.
- 대표적으로 GET, HEAD, PUT, DELETE 메서드가 있다.
  - PUT과 DELETE는 서버의 상태를 변경하므로 멱등성은 갖지만 safe하지는 않다

```jsx
GET /tutorial HTTP 1.1
GET /tutorial HTTP 1.1
-> 같은 응답을 받음. "멱등성" 을 가진다.

DELETE /delete/1 HTTP 1.1
-> 존재 시 제거. 200 응답
DELETE /delete/1 HTTP 1.1
-> 제거되었으므로 404. 상태 코드는 달라졌지만 "멱등성"을 가진다.

POST /write HTTP 1.1 -> 작성
POST /write HTTP 1.1 -> 추가로 호출할수록 새로운 글이 작성. "멱등성"을 갖지 않는다
```

## Cacheable

응답을 저장해 향후 재사용을 할 수 있는 것

## 많이 사용하는 6가지 HTTP 메서드

1. GET : 특정 리소스를 가져올 때 사용
2. POST : 서버로 데이터를 전송할 때 사용. 이를 통해 서버에서 변경이 일어남
3. PUT : 요청 payload를 사용해 새로운 리소스 생성, 혹은 타깃을 payload로 대체.
   **멱등성을 갖는다**
4. PATCH: PUT 메서드는 payload로 리소스를 대체하지만, PATCH는 리소스의 특정 부분을 수정할 때 사용
5. DELETE: 특정 리소스를 제거할 때 사용
6. OPTIONS: 주어진 URL 또는 서버에서 허용된 통신 옵션을 확인할 때 사용.
   서버에서 지원하는 메서드를 확인하거나 사전 요청(prefilghted request)을 보내는 용도로 사용

## 헤더

- 서버와 클라이언트 사이에서 body 외에 정보를 교환하는 방식
- HTTP 메세지 제일 앞에 삽입. 수십 개의 종류를 갖는다.
- 헤더는 대소문자를 구분하지 않고 **이름:값** 형태로 이루어진다.
- 사용되는 문맥(context)을 기준으로 Entity 헤더, General 헤더, Request 헤더, Response 헤더로 분류

### Request와 Response 헤더

- Request 헤더
  - 요청하는 자원이나 클라이언트 측 정보 포함
  - 클라이언트가 이해할 수 있는 정보를 나타내는 Accept-\*
  - 요청에 조건을 부여하는 if-\*
  - User-Agent, Cookie 포함
- Response 헤더
  - 서버의 위치나 응답에 대한 부가 정보
  - Request 정보에 대한 답
  - Server, Location 포함

### Entity 헤더

- 본문에 대한 메타 정보 또는 본문이 없는 경우 요청으로 식별된 자원에 대한 메타 정보를 포함
- Content-Type (Content-Length, text/html 같은 리소스 타입을 나타냄)

### General 헤더

- Request 및 Response에서 모두 사용되는 헤더, 문맥에 따라 리퀘스트가 되기도 리스폰스가 되기도 함
- body와는 직접적으로 관련이 없는 정보를 가짐
- 통신이 만들어진 날짜를 의미하는 Date
- 캐싱 매커니즘을 나타내는 Cache-Control
  - 캐싱 헤더를 사용하여 서버와 클라이언트 간의 통신 시 성능을 높일 수 있음
