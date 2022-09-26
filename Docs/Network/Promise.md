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

- Promise 객체는 상태를 나타내는 **state 프로퍼티**를 갖는다.
- `이행(fulfilled)` : 작업이 성공적으로 종료됨
- `거부(reject)` : 작업 실패
- `보류(pending)` : 작업이 이행되거나 거부되지 않은 초기 단계

### then(), catch() 그리고 finally()

- Promise 객체는 사용하는 측에서 then(), catch(), finally() 메서드를 통해 연결해서 사용할 수 있다.
- `then()` : Promise 객체를 리턴하고 두개의 콜백 함수를 인자로 받는다.
  - 첫번째는 fullfield일 때 결과, 두번째는 reject 되었을 때 에러
  - 성공만 처리하고 싶다면? → 두번째 콜백 생략, 실패만 처리하고 싶다면? → 첫번째 인자 null

```jsx
const promise = new Promise((resolve, reject) => {
  // ...
});

promise.then(
  (result) => {
    // 결과를 다루는 콜백
  },
  (error) => {
    // 에러를 다루는 콜백
  }
);

// 혹은

promise.then((result) => {
  // 결과만 다루는 콜백
});

promise.then(null, (error) => {
  // 에러만 다루는 콜백
});
```

- `catch()` : 에러를 처리하는 메서드. promise.then(null, callback)과 동일한 동작을 하지만
  - 더 명시적이고 간결한 에러 처리 수행

```jsx
promise.catch((error) => {
  // 에러만을 다루는 콜백
});
```

- `finally()` : fulfilled, rejected에 상관없이 무조건 실행되는 메서드
  - ES2018에 처음 등작
  - 작업이 종료되었을 때 필요한 공통적인 처리를 담당

```jsx
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("done"), 500);
});

promise
  .finally(() => {
    // fulfilled, rejected 상관없이 작업 수행
  })
  .then((result) => {
    // 결과는 finally를 통과해 전달 됨
    console.log(result); // done
  });
```

### Promise 체이닝

- then() 메서드는 Promise 결괏값을 기준으로 fulfilled 된다.
- 이 특성을 이용하여 Promise 객체의 결괏값을 연속으로 이어받아 순차적으로 비동기 작업을 할 수 있다.

```jsx
a((resultFromA) => {
  b(resultFromA, (resultFromB) => {
    c(resultFromB, (resultFromC) => {
      console.log(resultFromC);
    });
  });
});
```

- 이러한 형태를 `메서드 체이닝`이라고 한다.
  - 메서드의 반환 값을 이용해 또 다른 함수를 연속적으로 호출하는 패턴을 뜻한다.
- 콜백 함수를 이용해 비동기 작업 처리 시 콜백 지옥👿 구조가 된다.
  → Promise 체이닝을 통해 알아보기 쉽게 리팩터링 할 수 있다.

```jsx
a()
  .then((resultFromA) => b(resultFromB))
  .then((resultFromB) => c(resultFromC))
  .then((resultFromC) => console.log(resultFromC));
```

### Promise 정적 메서드

### `resolve()와 reject()`

- 항상 Promise를 반환하는 함수를 작성하거나, 호환성을 유지해야 할 때 편리하게 사용할 수 있다.

### `all(), allSettled() 그리고 race()`

- `all()` : **여러 개의 Promise를 동시에 실행시키고 모든 Promise가 완료되면 결괏값을 배열로 반환한다.**
  - 각 Promise의 시작, 종료 순서는 알 수 없다.
  - 반환되는 결괏값은 인자로 넘긴 Promise의 순서에 맞춰 반환된다.
  - 배열처럼 순회 가능한(iterable) Promise 객체를 인자로 받는다.
  🥚  ex ) all() 메서드 사용 전
  ```jsx
  let values = [];

  function doA() {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(1);
      }, 500);
    });
  }

  function doB() {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(2);
      }, 500);
    });
  }

  function doC() {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(3);
      }, 500);
    });
  }
  function sum() {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        // reduce() : 배열을 순회하여 하나의 값을 반환
        const value = values.reduce((total, value) => total + value, 0);
        resolve(value);
      }, 500);
    });
  }

  doA()
    .then((resultA) => {
      values.push(resultA);
    })
    .then((resultB) => {
      values.push(resultB);
      return doC();
    })
    .then((resultC) => {
      values.push(resultC);
      return sum();
    })
    .then((total) => {
      console.log(total);
    });
  ```
  🐣  ex ) all() 메서드를 사용 후
  ```jsx
  Promise.all([doA(), doB(), doC()])
    .then((result) => {
      values = result;
      return result; // [1, 2, 3] 순서대로 반환
    })
    .then(() => {
      return sum();
    })
    .then((total) => {
      console.log(total);
    });
  ```
  - 하지만 all() 메서드는 배열에 담긴 Promise 중 하나라도 실패하면 즉시 거부되며, 첫번째로 발생 실패한 이유를 반환한다.
  - **이전에 성공한 Promise가 존재하더라도 완전히 무시된다.**
  ```jsx
  Promise.all([Promise.resolve(3), Promise.reject(4), Promise.resolve(5)])
    .then((result) => {
      console.log("result : ", result);
    })
    .catch((err) => {
      console.log("error:", err); // 'error: 4'
    });
  ```
- 각각 Promise를 별도로 처리하기 위해 allSettled() 메서드를 사용할 수 있다.
- `allSettled()` : all()과 기능상 동일하지만 반환 값이 다르다.
  - 각 Promise가 성공했을 때 **{status: “fulfilled”, value: result}**
  - 실패했을 때 **{status: “rejected”, reason: error}**
  - 형태의 객체가 배열에 담겨 반환된다.
  - allSettled()는 _지원하지 않는 브라우저가 많기 때문에 지원 범위를 확인하고 써야 한다._
  🐣  ex )
  ```jsx
  Promise.all([Promise.resolve(3), Promise.reject(4), Promise.resolve(5)]).then(
    (result) => {
      console.log(result);
      // [{status : "fullfilled" result: 3},
      // {status : "rejected" result: 4},
      // {status : "fullfilled" result: 5}]
    }
  );
  ```
- `rece()` : 가장 먼저 처리되는 Promise의 결과(또는 에러)를 반환
  - 가장 먼저 처리되는 Promise가 있을 경우 다른 결과는 모두 무시
  - 세번째 Promise가 먼저 이행되며 3을 반환하며 실행이 종료된다.
  ```jsx
  Promise.race([
    new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
    new Promise((resolve, reject) =>
      setTimeout(() => reject(new Error("2")), 1500)
    ),
    new Promise((resolve, reject) => setTimeout(() => resolve(3), 500)),
  ])
    .then((result) => {
      console.log(result); // 3
    })
    .catch((err) => {
      console.log(err);
    });
  ```
