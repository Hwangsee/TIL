## fetch()

- XMLHttpRequest로 작성된 코드는 조금만 복잡해져도 가독성이 떨어진다.

**XMLHttpRequest 사용 예제 ex )**

```jsx
function get(url, done) {
  const xhr = new XMLHttpRequest();
  // 요청 환경 구성
  xhr.open("GET", url);

  // 응답 완료 시
  xhr.addEventListener("load", () => {
    done(xhr.responseText);
  });
  // 전송
  xhr.send();
}

get("https://test.com/profile", function (res) {
  const { id } = JSON.parse(res);
  get(`https://test.com/member/${id}`, function (secondRes) {
    const { name } = JSON.parse(secondRes);
    // 추가 요청 처리
  });
});
```

- 이런 단점을 해결하기 위해 나온 것이 `fetch()`라는 표준이다.

**fetch()로 리팩터링을 하게될 경우 ex )**

```jsx
fetch('https://test.com/profile')
	.then((res) => res.json());
	.then(({id}) => fetch(`https://test.com/member/${res.id}`))
	.then(({name}) => {
		// 추가 요청 처리
	});
```

### fetch()의 특징

- fetch()는 매개변수로 접근하고자 하는 url과 옵션 객체를 갖는다.
- fetch() 호출 시 브라우저는 요청을 보내고 Promise를 반환한다.
- **Promise는 Response의 인스턴스와 함께 fullfilled 상태가 된다.**
- Promise를 반환하기 때문에 Promise 체이닝과 await 문을 통해 다룰 수 있다.

```jsx
fetch(url, options)
	.then((response) => {
		// 응답 처리
	})
	.catch(error) {
		// 네트워크 에러 처리
	});

const response = await fetch(url, options);
```

- `options`에는 HTTP 메서드나 헤더, body 등을 넣을 수 있다.
- XMLHttpRequest와 마찬가지로 Cookie나 Host, Connection 등 보안 상으로 문제가 될 수 있는 헤더는 넣을 수 없다.

```jsx
const user = {
  // user 정보
};
const url = "https://naver.com";

fetch(url, {
  method: "POST",
  headers: {
    "Content-Type": "application/json;charset=utf-8",
  },
  body: JSON.stringify(user),
});
```

- 응답에 대한 특정 헤더 값은 response.headers 프로퍼티의 get() 메서드로 얻을 수 있다.

```jsx
fetch(url).then((res) => {
  const contentType = res.headers.get("Content-Type"); // 일부

  for (let [name, value] of response.headers) {
    // 전체 (배열 형태의 쌍)
    console.log(name, value); // key, value 출력
  }
});
```

- 반환되는 Promise 객체는 HTTP 상태 코드가 404나 500이 되어도 fullfilled 된다.
  - 네트워크 장애 같은 요인으로 요청이 완료되지 못 했을 때만 rejected 된다.
- HTTP 상태 코드에 따라 응답을 처리하기 위해서는 response 객체의 ok 혹은 status 프로퍼티를 사용해야 한다.

```jsx
fetch(url)
  .then((res) => {
    if (res.ok) {
      return res;
    }

    const error = new Error(res.statusText); // 상태 메세지 출력
    error.response = res;
    throw errl;
  })
  .catch(
    (err = {
      // handle error
    })
  );
```

response 객체는 응답 데이터를 원하는대로 가공할 수 있는 Promise 기반 여러 메서드를 제공한다.

- text() : 응답 결과를 텍스트로 반환
- json() : 응답 결과를 JSON 형태로 반환
- formData() : 응답 결과를 formData 객체 형태로 반환
- blob() : 응답 결과를 Blob으로 반환 (이미지, 사운드, 비디오 등)
- arrayBuffer() : 응답 결과를 ArrayBuffer로 반환
