## 타입 변환

- 명시적 강제 변환 : 의도적으로 타입을 변환하는 것
- 암시적 강제 변환 : 표현식의 평가 중 타입이 변환되는 것

### 명시적 강제 변환

- 문자열로 변환

### 객체의 원시 타입 변환

**문자열로 변환**

`객체가 문자열로 변환되는 과정`

1. 객체에 정의된 toString() 메서드를 호출한다 (Object.prototype.toString() 메서드 실행
    
    ⇒ `Object object` 문자열 반환
    
2. 1단계의 결과가 원시 타입이라면 문자열로 변환하여 반환, 아니라면 valueOf 메서드를 호출
3. valueOf() 메서드가 원시 타입이라면 문자열로 반환 아니라면 TypeError 발생

```jsx
console.log(String({})); // '[Object object]'
```

**숫자로 변환**

1. valueOf() 메서드 호출
2. 원시 타입이라면 숫자로 변환하여 반환, 아니라면 toString() 호출
3. 원시 타입이라면 숫자로 변환하여 반환, 아니라면 TypeError 발생

```jsx
console.log(Number({})); // NaN
```

### 암시적 강제 변환

- 연산 중 내부적으로 타입을 변환하는 것을 뜻하기 때문에 타입이 아닌 연산자를 기준으로 알아본다.

**덧셈 연산자**

1. 피연산자 중 하나가 문자열 타입인 경우 나머지 타입도 문자열로 변환하여 병합한다.
    
    ```jsx
    console.log(1 + ''); //1
    ```
    
2. 피연산자 중 하나가 객체이며 문자열로 변환 가능한 경우 문자열로 변환하여 연산한다.
    
    ```jsx
    // 빈 객체는 '[object Object]' 문자열로 변환이 가능하므로 숫자 1을 문자열로 변환하여 두 문자열을 병합한다.
    console.log(1 + {}); // '1[object Object]'
    ```
    
3. 피연산자가 모두 문자열과 객체가 아닌 경우 숫자로 변환하여 연산한다. 만약 변환 결과의 타입이 각각 다른 경우 TypeError가 발생한다.
    
    ```jsx
    // 피연산자 중 객체나 문자열이 없기 때문에 true를 숫자로 변환하여 연산한다.
    console.log(1 + true); //2
    ```
    

**동등 연산자**

1. 피연산자 중 하나는 문자열, 하나는 숫자인 경우 문자열을 숫자로 변환하여 동등함을 비교한다.
    
    ```jsx
    // 문자열 '1'을 숫자로 변환하여 동등함을 판단한다.
    console.log(1 == '1'); //true
    ```
    
2. 피연산자 중 하나는 문자열, 하나는 BigInt인 경우 문자열을 BigInt로 변환하여 동등함을 비교한다.
    
    ```jsx
    console.log(1n == '1') // true
    ```
    
3. 피연산자 중 하나는 null, 다른 하나는 undefined인 경우 동등하게 판단한다.
    
    ```jsx
    console.log(null == undefined); // true
    console.log(undefined == null) // true
    ```
    
4. 피연산자 중 하나가 불리언일 경우 불리언을 숫자로 변환하여 동등함을 비교한다.
    
    ```jsx
    console.log(true == 1); // true
    ```
    
5. 피연산자 중 하나는 객체, 다른 하나가 문자열, 숫자, BigInt, 심볼 중 하나일 경우 객체를 원시 타입으로 변환하여 동등함을 비교한다.
    
    ```jsx
    console.log('[object Object]' == {}); // true
    ```
    
6. 피연산자 중 하나는 숫자, 다른 하나는 BigInt인 경우 내부적인 숫자 비교 알고리즘에 따라 비교한 결과를 반환한다.
    
    ```jsx
    console.log(1 == 1n); // true
    ```
    

암시적 타입변환 규칙은 어렵지 않지만 가독성이 떨어진다는 단점이 있다.

**예시 )**

- ture는 숫자 1로 변환되어 문자열 ‘1’과 비교하게 되고, 이 과정에서 문자열은 ‘1’과 비교하게 되고, 이 과정에서 문자열 ‘1’은 숫자 1로 변환되어 최종적으로 숫자 1과 1을 비교하게 된다.
    
    ```jsx
    console.log(true == '1'); // true
    ```
    

> 동등 연산자 코드는 코드 가독성을 위해 팀원들과 컨벤션을 정하여 사용하는 것이 좋다.
만약 팀 내에서 암시적 강제 변환을 사용하지 않겠다고 결정하게 되면 엄격한 동등 연산자면 사용하면 된다.
> 

**비교 연산자**

- 문자열 비교 : 알파벳 순서로 비교
    
    ```jsx
    console.log('a' < 'b') // true
    
    // 왼쪽부터 문자 단위로 비교함
    console.log('1' < '04') //false
    ```
    

**논리 연산자 (&&, ||)**

- 자바스크립트에서 **논리 연산자의 결과 값은 불리언이 아닐 수 있다.**
- `&& 논리 연산자` : 첫번째 피연산자의 값이 true로 평가되는 경우 두번째 피연산자의 값을 반환하고, false로 평가되면 첫번째 피연산자의 값을 반환한다
- `|| 논리 연산자` : 첫번째 피연산자의 값이 true로 평가되는 경우 첫번째 피연산자의 값을 반환하고, false로 평가되면 두번째 피연산자의 값을 반환한다.

```jsx
const a = null;
const b = 'javascript';
const c = 1;
// 1. a 평가
// 2. a는 불리언 타입이 아니기 때문에 암시적 타입 변환을 통해 불리언으로 값을 변환. 
//    null은 falsy 값이기 때문에 false로 변환
// 3. a의 평가 결과가 false이기 때문에 b는 평가하지 않음
// 4. a 값 반환

console.log(a && b); // null
console.log(b || c); // 'javascript'
// 1. b 평가
// 2. b는 불리언 값이 아니기 때문에 암시적 타입 변환 -> true
// 3. b가 true이기 때문에 c 평가하지 않음
```

**논리 연산자의 활용**

- 디폴트 값을 설정하거나 조건에 따라 함수를 실행할 때 유용하다.

```jsx
// a의 값이 falsy일 경우 'default string'이라는 문자열을 디폴트로 설정
function setDefault(a) {
  return a || 'default string';
}
```

```jsx
const a = 'javascript';

function doSomething() {
  // ...
}
// a의 값이 true인 경우에만 doSomething() 함수를 실행 
**// 리액트의 컴포넌트를 조건에 따라 렌더링 할 때 자주 사용하는 표현식**
**a && doSomething();**
```

falsy 값이 아닌 null, undefined처럼 값이 비어 있는 경우에 디폴트 값을 설정하고 싶을 때는
|| 연산자가 아닌 ES2020에서 등장한 null 병합(nullish coalescing) 연산자를 사용할 수도 있다.

```jsx
const a = '';
// a가 null, undefined인 경우에만 'default' 문자열이 b의 값으로 할당된다.
const b = a ?? 'default';
```