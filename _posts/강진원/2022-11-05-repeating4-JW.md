---
title: "useRef"
layout: post
comments: true
categories: [강진원/복습일지]
tags: [useRef, DOM, 상태, react]
---
# useRef
Ref는 'referece'의 약자로 '참조'라는 뜻!   
uesRef는 다음과 같은 경우에 사용할 수 있다.

### 1. DOM요소에 접근 
JavaScript 를 사용 할 때에는, 우리가 특정 DOM 을 선택해야 하는 상황에 getElementById, querySelector 같은 DOM Selector 함수를 사용해서 DOM 을 선택했다.
리엑트 애플리케이션을 만들 때 DOM을 직접 조작하는 것은 지양해야한다. 하지만, 리엑트를 사용하다 보면 가끔씩 DOM을 직접 선택해야 하는 상황이 발생 할 때도 있다.   
(예를 들어서 특정 엘리먼트의 크기를 가져와야 한다던지, 스크롤바 위치를 가져오거나 설정해야된다던지, 또는 포커스를 설정해줘야된다던지, 속성 값을 초기화(clear)할 필요가 있다던지 등이 있다.)   
DOM을 직접 건드려야 할때 사용할 수 있는 것이 바로 useRef라는 Hook 함수이다.    

### 2. 저장공간(변수 관리)
DOM을 직접 건드리는 거 외에도, 리엑트에서는 상태가 변할 때마다 재 렌더링이 되면서 컴포넌트 내부 변수들이 초기화가 된다. 원하지 않는 렌더링이 발생할 때, 만약 다시 랜더링 되어도 동일한 참조값을 유지하고싶다면? useRef 훅 함수를 쓸 수 있다.   
보통은 상태관리 할때 useState를 썼는데, State대신 Ref를 쓴다.
useRef 함수는 current 속성을 가지고 있는 객체를 반환하는데, 인자로 넘어온 초기값을 current 속성에 할당한다. 이 **current 속성은 값을 변경해도 상태를 변경할 때 처럼 React 컴포넌트가 다시 랜더링되지 않는다.** React 컴포넌트가 다시 랜더링될 때도 마찬가지로 이 current 속성의 값이 유실되지 않는다.

### DOM요소 접근 예시
```js
import { useEffect, useRef } from 'react'
import './App.css';

function App() {
  const inputRef = useRef();

  useEffect(() => {
    console.log(inputRef);
    inputRef.current.focus();
  }, [])

  const loginAlert = () => {
    alert(`환영합니다. ${inputRef.current.value}`);
    inputRef.current.focus();
  }
  return (
    <div className="App">
      <header className="App-header">
        <input ref={inputRef} type="text" placeholder="id"/>
        <button onClick={loginAlert}>Login</button>
      </header>
  </div>
  );
}

export default App;
```
### 변수관리 예시
```js
import { useState, useRef } from 'react'
import './App.css';

function App() {
  const [render, setRender] = useState(false);
  const countRef = useRef(0);
  let countVar = 0;

  
  console.log('***** 렌더링 후 Ref:', countRef.current);
  console.log('***** 렌더링 후 Var:', countVar);
  
  const increaseRef = () => {
    countRef.current = countRef.current + 1;
    console.log('Ref Up! --->', countRef.current);
  }

  const increaseVar = () => {
    countVar = countVar + 1;
    console.log('Var Up! --->', countVar);
  }

  const doRender = () => {
    setRender(!render);
  }

  return (
    <div className="App">
      <header className="App-header">
        <p>Ref: {countRef.current}</p>
        <p>Var: {countVar}</p>
        
        <div>
          <button onClick={increaseRef}>Ref Up!</button>
          <button onClick={increaseVar}>Var Up!</button>
          <button onClick={doRender}>Render!</button>
        </div>
      </header>
  </div>
  );
}

export default App;
```




