---
title: "Redux"
layout: post
comments: true
categories: [강진원/복습일지]
tags: [상태, react, redux]
---
# Redux
React에서 상태관리를 위한 라이브러리이다.   

### 등장 배경
프론트엔드 개발에서의 상태는 "동적으로 표현되는 데이터"를 뜻한다.   
로컬 상태는 특정 컴포넌트 안에서만 관리되는 상태이며, 전역 상태는 프로덕트 전체 혹은 여러 가지 컴포넌트가 동시에 관리하는 상태를 말한다.

React에서 상태를 관리 할 때, 최상위 컴포넌트의 state를 props를 통해 전달하여 사용했다. 이러한 방법은 간단한 구조의 react 애플리케이션에서는 큰 문제가 되지 않지 구조가 복잡해질수록 이는 문제가 된다.    
예를 들어 A->B->C->D 식으로 A가 최상위 컴포넌트이고 D가 최하위 컴포넌트일 때, B와 C는 A의 state를 props로 받을 필요 없는 컴포넌트라 가정을 하고, D는 A의 state를 props를 받아야 하는 컴포넌트라고 가정을 하면 D가 A의 상태를 받기 위해 어쩔 수 없이 B,C컴 포넌트도 A로 부터 props를 전달 받아 D에게 전달해줘야한다. 이와 같이 props를 전달하는 용도로만 쓰이는 컴포넌트들을 거치면서 데이터를 전달하는 현상을 <span style='background-color: #fff5b1'>**props drilling**</span>이라고 한다. props의 전달 횟수가 많아져 props drilling 이 발생하면 다음과 같은 문제가 발생한다.
* 코드의 가독성이 나빠진다
* 코드의 유지 보수가 힘들어진다.
* state 변경시 Props 전달 과정에서 불필요하게 관여된 컴포넌트들 또한 리렌더링이 발생한다. 따라서, 웹 성능에 악영향을 줄 수 있다.     


props drilling 이외에 컴포넌트 내부에서 상태를 관리하며 발생하는 여러 복잡한 문제들을 단번에 해결 할 수 있는 방법으로 Redux라는 상태관리 라이브러리가 있다.
Redux는 전역 상태를 관리할 수 있는 저장소엔 Store를 제공함으로써 문제를 해결한다. Store에서 필요한 상태만 꺼내쓰는 그런 느낌

### Redux 상태 관리 순서
Redux에서는 Action → Dispatch → Reducer → Store 순서로 데이터가 단방향으로 흐른다.
* **Action**   
어떤 액션을 취할 것인지 정해 놓은 **객체**   
type은 필수로 지정해 줘야 한다. 해당 Action 객체가 어떤 동작을 명시해주는 역할을 하며 대문자,Snake_Case로 작성한다. 이후 Reducer 함수를 실행 할 때 switch 조건문에서 type으로 어떤 Action을 취할지 결정된다.

* **Dispatch**   
Reducer로 Action을 전달해주는 함수 Dispatch의 전달 인자로 Action 객체가 전달된다.
* **Reducer**    
Dispatch에서 전달받은 Action 객체의 type 값에 따라 상태를 변경시키는 함수. 이 때, Reducer는 순수함수여야 한다. 외부 요인으로 인해 기대한 값이 아닌 엉뚱한 값으로 상태가 변경되는 일이 없어야하기 때문이다. 
* **Store**   
상태가 관리되는 오직 하나뿐인 저장소 역할을 한다. Redux의 state가 저장되어 있는 공간이다.

### Redux Hooks
* useSelector()   
컴포넌트와 state를 연결하여 Redux의 state에 접근 할 수 있게 해준다.   
useSelector에 매개변수에 state => state.모듈명  형식으로 상태값을 반환할 수 있다.   
ruit 상태 안에 name, price 등의 요소를 바로 가져오고 싶을 경우, state => state.fruit.name   state => state.fruit.price 와 같이 요소요소들을 마침표로 구분하여 상태값을 가져올 수도 있다.

```js
import { useSelector } from 'react-redux';

const fruitList = useSelector(state => state.fruit);
// useSelector의 사용법
const fruitList = useSelector(state => state.fruit.name);
```

* useDispatch()   
Action 객체를 Reducer로 전달해 주는 Dispatch 함수를 반환하는 메서드이다.    
useDispatch객체를 dispatch로 재선언한 후, dispatch 변수를 활용하여 액션을 호출할 수 있다.   
```js
import { useDispatch } from 'react-redux';

cosnt dispatch = useDispatch(); // dispatch로 재선언하여 사용한다.

// 그러면 Action객체를 Reducer에 전달 할 때, 다음과 같이 쓸 수 있다.
dispatch({type : INCREASE })
```
