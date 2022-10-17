---
layout: single
title: "전예린 복습일지"
categories: 복습일지
tags: 전예린
---

### 221017 || side Effect(부수 효과)와 useEffect

함수 내의 어떤 구현이 함수 외부에 영향을 끼치는 경우 해당 함수는 Side Effect가 있다고 이야기한다. 
보통 React 애플리케이션을 작성할 때 AJAX 요청, LocalStorage 또는 타이머와 같이 React와 상관없는 API를 사용하며 많은 Side Effect가 발생하게 된다.
React는 이러한 Side Effect를 다루기 위한 Hook인 Effect Hook <b>useEffect</b>함수를 제공한다.
<br/>
useEffect() 함수는 React component가 렌더링 될 때마다 특정 작업(Side effect)을 실행할 수 있도록 하는 리액트 Hook이다. <br/>useEffect의 기본형태를 살펴보자.

```js
useEffect(function, deps)
```

function은 실행하고자 하는 함수이며, 해당 함수 내에서 side Effect를 실행하면 된다. deps는 배열 형태이며 function을 실행시킬 조건이다. 
deps 배열 내의 어떤 값이 변할 때에만, function이 실행된다.
<br/>

이제 useEffect의 조건부 실행에 대해 알아보자.

1. useEffect(function,[])
2. useEffect(funtion)
3. useEffect(function,[name])

1번처럼 deps 부분에 빈 배열을 넣게되면, 맨 처음 렌더링 될 때 한 번만 실행된다. 
2번처럼 deps 부분을 생략한다면, 해당 컴포넌트가 렌더링 될 때마다 useEffect가 실행된다.
3번처럼 deps 부분에 특정 값을 넣게되면, 그 특정 값인 'name'이 업데이트 될 때마다 실행된다.


