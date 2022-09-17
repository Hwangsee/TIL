## JSON

- 더글라스 크락포드가 처음 정의하고 보급한 문자 기반의 데이터 교환 방식
- 대부분의 프로그래밍 언어는 표준에 맞게 JSON을 파싱하거나 직렬화(serialization)할 수 있다.

### 구조

- 객체 리터럴 작성법을 따른다.
- 자바스크립트 원시 자료형을 가질 수 있다.
- 중첩된 계층 구조를 가질 수 있다.
- 자바스크립트의 객체와 형태가 비슷하지만, JSON은 순수한 데이터 포맷이다.
  - undefined, NaN, INFINITY와 같은 값을 존재하지 않는다.
- 프로퍼티와 값은 큰 따옴표만 사용이 가능하다.

```jsx
{
	"name" : "hwang",
	"age" : 30,
	"langauge" : [
		{
			"name" : "JavaScript",
			"creater" : "Brendan Eich"
		},
		{
			"name" : "Java",
			"creater" : "James Arthur Gosling"
		}
	]
}
```

### 메서드

- JSON은 문자열 형태이기 때문에 자바스크립트 객체를 데이터로 전송하거나 전송 받을 때 파싱이 필요하다.

**stringify()**

- 값이나 객체를 JSON 문자열로로 변환
- 네트워크를 통해 네이티브 객체를 문자열로 변환할 때 주로 사용한다.
- 자바스크립트에 존재하는 타입이지만 JSON으로 존재하지 않을 경우 몇 가지 규칙에 맞춰 변환된다.
  - Boolean, Number, String 타입은 기본형 값으로 변환된다.
  - 문자열화 될 때 객체의 속성은 순서가 보장되지 않는다. (배열은 보장됨)
  - undefined, Symbol 타입은 객체 안에 있을 시 생략된다. 배열 안에 있을 시 null로 변환
  - NaN, Infinity의 경우 null로 표현
  - 함수는 객체 값으로 허용되지 않는다.

```jsx
JSON.stringify({}); // "{}"
JSON.stringify(false); // "false"
JSON.stringify(100); // "100"
JSON.stringify([
  1,
  NaN,
  () => {
    console.log("hi");
  },
]); // "[1,null,null]"
JSON.stringify({
  a: 1,
  b: NaN,
  c: () => {
    console.log("hi");
  },
}); // "{"a":1, "b":null}"
```

**parse()**

- JSON 문자열을 구문 분석하고 ECMAScript 값을 생성, ECMAScript 객체로 표현된다.
- 타입이 일치하는 배열, 문자열, 숫자, 불리언, null은 ECMAScript 타입으로 변환된다.

```jsx
const json = '{"a": 100, "b": null, "c": true}';
const obj = JSON.parse(json);

JSON.parse(obj);
console.log(obj.a, obj.b, obj, c); // 100, null, true
```
