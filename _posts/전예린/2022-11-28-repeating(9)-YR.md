---
title: "복습일지(9)"   
layout: post    
comments: true  
categories: [전예린/복습일지]
tags: Hook, useMemo, useCallback
---

### Hook

클래스 컴포넌트는 복잡해질수록 이해하기 어려워졌고, 컴포넌트 사이에서 상태 로직을 재사용하기 어렵다는 단점이 있었다.
또한 React의 클래스 컴포넌트를 사용하기 위해서는 JavaScript의 this 키워드가 어떤 방식으로 동작하는지 알아야 하는데, 
이는 문법을 정확히 알지 못하면 동작 방식 자체를 정확히 이해하기 어렵게 만들곤 했다.

그래서 React는 점진적으로 클래스 컴포넌트에서 함수 컴포넌트로 넘어갔다.
다만 이전까지의 함수 컴포넌트는 클래스 컴포넌트와는 다르게 상태 값을 사용하거나 최적화할 수 있는 기능들이 조금 미진했는데, 그 부분들을 보완하기 위해 Hook이라는 개념을 도입하였다.

 
Hook은 리액트 16.8 버전 이후 추가된 기능이며, Hook이 등장하면서 더 이상 상태를 관리하기 위해 Class를 쓸 필요가 없어졌다. 
기존에는 Class형 컴포넌트에서만 상태를 관리 할 수 있었고 함수형 컴포넌트에서는 상태를 관리할 수 없었지만, 
Hook을 통해 상태 관리를 할 수 있게 되었고 상태 관리 뿐만 아니라 기존 클래스형 컴포넌트에서만 가능하던 여러 기능을 사용할 수 있게 되었다.

Hook은 간단히 말하면 함수 컴포넌트에서 React state와 생명주기 기능(lifecycle features)을 “연동(hook into)“할 수 있게 해주는 함수다. 
Hook을 통해 class 없이 리액트를 사용할 수 있다. Hook은 class가 아닌 function으로만 React를 사용할 수 있게 해주는 것이기 때문에 클래스형 컴포넌트에서는 동작하지 않는다.

Hook은 리액트 함수의 최상위에서만 호출해야 하고, 오직 리액트 함수 내에서만 사용되어야 한다.

<br/>

### useMemo

useMemo는 특정 값(value)를 재사용하고자 할 때 사용하는 Hook이다. 메모이제이션된 값을 반환한다.

`useMemo(() => fn, deps)`

useMemo는 deps가 변한다면, () => fn이라는 함수를 실행하고, 그 함수의 반환 값을 반환한다. 
deps는 dependency이며, useMemo가 이 deps라는 것에 '의존'한다는 뜻이다.

예시를 살펴보자.

```js
import React, { useState, useCallback, useMemo } from "react";

export default function App() {
  const [x, setX] = useState(0);
  const [y, setY] = useState(0);

  // useMemo
  useMemo(() => {console.log(x)}, [x]);

  // 두 개의 버튼이 있다. X버튼을 누르는 것만이 x를 변화시킨다.
  return (
    <>
      <button onClick={() => setX((curr) => (curr + 1))}>X</button>
      <button onClick={() => setY((curr2) => (curr2 + 1))}>Y</button>
    </>
  );
}
```
위의 코드에서 deps는 [x]이므로, x 가 변할 때에만 () => {console.log(ex)} 이 실행된다. 따라서 X 버튼을 누를 때에만 콘솔창에 ex 값이 출력된다.
Y 버튼을 누르더라도 APP이라는 함수 컴포넌트가 전부 재실행(리렌더링) 되지만, x 값은 변하지 않았기 때문에 useMem에는 아무런 변화가 없다.

<br/>

### useCallback

 useMemo는 함수를 실행해버리는 반면, useCallback은 함수를 반환한다. 
 useMemo는 값의 재사용을 위해 사용하는 Hook이라면, useCallback은 함수의 재사용을 위해 사용하는 Hook이다.

`useCallback(fn, deps)`

useCallback은 deps가 변한다면 fn이라는 새로운 함수를 반환한다. 함수가 의존하는 값들이 바뀌지 않는 한 기존 함수를 계속해서 반환하다.

React는 리렌더링 시 함수를 새로이 만들어서 호출한다. 
새로이 만들어 호출된 함수는 기존의 함수와 같은 함수가 아니다. 
그러나 useCallback을 이용해 함수 자체를 저장해서 다시 사용하면 '함수의 메모리 주소 값을 저장했다가 다시 사용한다는 것'과 같다고 볼 수 있다. 
따라서 React 컴포넌트 함수 내에서 다른 함수의 인자로 넘기거나 자식 컴포넌트의 prop으로 넘길 때 예상치 못한 성능 문제를 막을 수 있다.

예시를 살펴보자.

```js
import React, { useState, useCallback, useMemo } from "react";

export default function App() {
  const [x, setX] = useState(0);
  const [y, setY] = useState(0);

  // useCallback이 () => {console.log(y)} 라는 함수를 반환한다.
  const useCallbackReturn = useCallback(() => {console.log(y)}, [x]);

  // useCallback 이 담겨있는 함수를 실행
  useCallbackReturn();

  return (
    <>
      <button onClick={() => setX((curr) => (curr + 1))}>X</button>
      <button onClick={() => setY((curr2) => (curr2 + 1))}>Y</button>
    </>
  );
}
```

위의 useCallback 은 () => {console.log(y)}라는 함수를 반환해주고 있다. 위의 useCallback 은 다음의 순서로 진행된다.

1. 처음 컴포넌트가 시작될 때 실행 () => {console.log(0)}
2. x 가 변할 때까지 함수는 () => {console.log(0)} 실행
3. x 가 변한다면 그제서야 y 의 값을 가져와서 () => {console.log(새로운 값)} 실행

useCallback은 함수와는 상관없는 상태 값이 변할 때, 함수 컴포넌트에서 불필요하게 함수를 업데이트하는 것을 방지해준다. 
다만, deps를 잘못 설정하면 아무리 함수 컴포넌트를 재실행해도 함수가 변하지 않으며 원치 않는 상황이 올 수도 있으므로 섬세한 컨트롤이 필요하다. 이는 useMemo에도 동일하게 적용된다.

사실 useCallback만 사용해서는 useMemo에 비해 괄목할 만한 최적화를 느낄 수는 없다. 
왜냐하면 useCallback은 함수를 호출 하지 않는 Hook이 아니라, 그저 메모리 어딘가에 함수를 꺼내서 호출하는 Hook이기 때문이다. 
따라서 단순히 컴포넌트 내에서 함수를 반복해서 생성하지 않기 위해서 useCallback을 사용하는 것은 큰 의미가 없거나 오히려 손해인 경우도 있다.

<br/>

! react 공식 문서에서도 인정하듯이, 아래의 두 식은 같다. !

`useCallback(fn, deps) is equivalent to useMemo(() => fn, deps).`

<br/>


참고 블로그 ) https://whales.tistory.com/117
