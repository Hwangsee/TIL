## useMemo란?

- useMemo는 연산된 값을 재사용하는 방식으로 함수 컴포넌트 내부에서 발생하는 연산을 최적화하는 Hook이다.
- 아래와 같이 같이 사용자가 입력한 값으로 합계를 내는 함수를 작성했을 때,
- `코드보기`
    
    ```jsx
    import React, { useCallback, useMemo, useRef, useState } from "react";
    
    /* eslint-disable */
    const getSum = (numbers) => {
      if (numbers.length === 0) return 0;
      console.log("값을 더하는 중..");
      return numbers.reduce((prev, current) => prev + current);
    };
    
    const Sum = () => {
      const [list, setList] = useState([0]);
      const [number, setNumber] = useState();
      const inputElem = useRef();
    
      const onChange = (e) => {
        setNumber(e.target.value);
      };
    
      const onCreate = (e) => {
        const prevList = list.concat(parseInt(number));
        setList(prevList);
        setNumber("");
        inputElem.current.focus();
      };
    
      const sumVal = getSum(list);
    
      return (
        <div>
          <input value={number || ""} onChange={onChange} ref={inputElem} />
          <button onClick={onCreate}>등록</button>
          <ul>
            {list.map((value, index) => (
              <li key={index}>{value}</li>
            ))}
          </ul>
          <div>
            <b>합계:</b> {sumVal}
          </div>
        </div>
      );
    };
    
    export default Sum;
    ```
    

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/025df466-6c99-4643-8482-d84d1eb4132f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221008T132646Z&X-Amz-Expires=86400&X-Amz-Signature=0f2fdc05e95d6334d2aa5b573d32d4113a9b16b92ebf7980a68a99c719710648&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

**왜 값을 입력할 때마다 getSum() 함수가 호출 되나요?**

- input 컴포넌트에 change 이벤트가 발생할 때마다 컴포넌트가 리렌더링 된다.
- 리렌더링 될 때마다 getSum() 함수가 호출되어 불필요하게 자원을 낭비하고 있다.
- 이럴 때 `useMemo` 를 사용하여 성능 최적화를 할 수 있다.
    - memo는 memoized를 의미하는데 이는 이전에 계산한 값을 재사용한다는 의미를 가진다.

## useMemo 사용방법

- 첫번째 파라미터에는 어떻게 연산할 함수(`getSum`)를 넣어주면 된다.
- 두번째 파라미터에는 deps 배열(`list`)을 넣어주면 되는데, 이 배열 안의 내용이 바뀌면 연산할 함수를 호출하고, 내용이 바뀌지 않으면 이전에 연산한 값을 재사용하게 된다.
- `useMemo 적용 코드보기`
    
    ```jsx
    import React, { useCallback, useMemo, useRef, useState } from "react";
    
    /* eslint-disable */
    const getSum = (numbers) => {
      if (numbers.length === 0) return 0;
      console.log("값을 더하는 중..");
      return numbers.reduce((prev, current) => prev + current);
    };
    
    const Sum = () => {
      const [list, setList] = useState([0]);
      const [number, setNumber] = useState();
      const inputElem = useRef();
    
      const onChange = (e) => {
        setNumber(e.target.value);
      };
    
      const onCreate = (e) => {
        const prevList = list.concat(parseInt(number));
        setList(prevList);
        setNumber("");
        inputElem.current.focus();
      };
    
      const sumVal = useMemo(() => getSum(list), [list]);
    
      return (
        <div>
          <input value={number || ""} onChange={onChange} ref={inputElem} />
          <button onClick={onCreate}>등록</button>
          <ul>
            {list.map((value, index) => (
              <li key={index}>{value}</li>
            ))}
          </ul>
          <div>
            <b>합계:</b> {sumVal}
          </div>
        </div>
      );
    };
    
    export default Sum;
    ```

    ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9f5e2fbf-ee37-41b8-837f-714ffe205b73/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221008T132629Z&X-Amz-Expires=86400&X-Amz-Signature=6d84aecbd74c9af1afc79f2e787701f3bd8d75e193d2e305567d41d341ac77c4&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

컴포넌트가 최초 렌더링 됐을 때 1번, 함수 값을 입력한 5번만 호출이 되었다.