---
title: "복습일지(6)"   
layout: post    
comments: true  
categories: [전예린/복습일지]
tags: Redux
---

### 221107 || Redux

Redux는 상태 관리 라이브러리이다. Redux를 이용할 경우 컴포넌트에서 사용할 '상태'에 관련된 코드만 다른 파일로 분리시켜서 관리할 수 있다. 

Redux를 사용하는 가장 큰 이유는 '공통적으로 이용하는 상태'를 전역에서 관리 하기 위함이다. 
프로젝트가 커지면서 컴포넌트 구조가 복잡해질 경우 Redux를 이용하여 상태를 관리한다면 유지보수 및 코드작성의 효율성을 극대화시킬 수 있다.


Redux는 다음과 같은 순서로 상태를 관리한다.

![image](https://user-images.githubusercontent.com/39157466/200230882-6df94f76-05ce-4719-889e-4818f23e52a3.png)

1. 상태가 변경되어야 하는 이벤트가 발생하면, 변경될 상태에 대한 정보가 담긴 Action 객체가 생성된다.
2. 이 Action 객체는 Dispatch 함수의 인자로 전달된다.
3. Dispatch 함수는 Action 객체를 Reducer 함수로 전달해준다.
4. Reducer 함수는 Action 객체의 타입을 확인하고, 그 타입에 따라 전역 상태 저장소 Store의 상태를 변경한다.
5. 상태가 변경되면, React는 화면을 다시 렌더링 한다.


즉, Redux에서는 Action → Dispatch → Reducer → Store 순서로 데이터가 '단방향'으로 흐르게 된다.


#### Store

컴포넌트와는 별개로 Store라는 공간이 있어서 그 Store 안에 앱에서 필요한 상태를 담는다.
컴포넌트에서 상태 정보가 필요할 때 Store에 접근한다.

```js
import { createStore } from 'redux';

const store = createStore(rootReducer);
```

위 코드와 같이 createStore 메서드를 활용해 Reducer를 연결 Store를 생성할 수 있다.

#### Reducer

Reducer는 Dispatch에게서 전달받은 Action 객체의 type 값에 따라서 상태를 변경시키는 순수 함수이다.

```js
const count = 1

// Reducer를 생성할 때에는 초기 상태를 인자로 요구한다.
const counterReducer = (state = count, action) => {

  // Action 객체의 type 값에 따라 분기한다.
  switch (action.type) {
    case 'INCREASE':
		return state + 1
    
    case 'DECREASE':
		return state - 1

    case 'SET_NUMBER':
		return action.payload

    // 해당 되는 경우가 없을 땐 기존 상태를 그대로 리턴한다.
    default:
      return state;
	}
}
```

만약 여러 개의 Reducer를 사용하는 경우, Redux의 combineReducers 메서드를 사용해서 하나의 Reducer로 합쳐줄 수 있다.

```js
import { combineReducers } from 'redux';

const rootReducer = combineReducers({
  counterReducer,
  anyReducer,
  ...
});
```

Reducer에서 순수함수로 상태 업데이트 하는 방법에는 여러가지가 있다.

첫번째로는 Object.assign()을 이용하는 방법이다.
```js
return Object.assign( {}, state, {새로 업데이트 할 것} );
```

두번째로는 Spread syntax를 이용하는 방법이다.
```js
return { ...state, {새로 업데이트 할 것} };
```

#### Action

Action은 어떤 액션을 취할 것인지 정의해 놓은 객체로, 자바스크립트 객체 형식으로 되어있다.

```js
// payload가 필요 없는 경우
{ type: 'INCREASE' }

// payload가 필요한 경우
{ type: 'SET_NUMBER', payload: 5 }
```
해당 Action 객체가 어떤 동작을 하는지 명시해주는 역할을 하기 때문에 'type' 은 필수로 지정 해 주어야 하며, 대문자와 Snake Case로 작성한다.

보통 Action을 직접 작성하기보다는 Action 객체를 생성하는 함수를 만들어 사용하는 경우가 많다. 이러한 함수를 '액션 생성자(Action Creator)'라고도 한다.
```js
const setNumber = (num) => {
  return {
    type: 'SET_NUMBER', // 필수
    payload: num // 옵션
  }
}
```

#### Dispatch

Dispatch는 Reducer로 Action을 전달해주는 함수이다. ispatch(액션)를 통해 Reducer를 호출하면 Reducer는 Store를 업데이트한다.


```js
// Action 객체를 직접 작성하는 경우
dispatch( { type: 'INCREASE' } );
dispatch( { type: 'SET_NUMBER', payload: 5 } );

// 액션 생성자(Action Creator)를 사용하는 경우
dispatch( increase() );
dispatch( setNumber(5) );
```

이제 Store, Reducer, Action, Dispatch 이 개념들을 연결시켜주어야 할 텐데, 이때는 Redux Hooks가 사용된다.


Redux Hooks는 React-Redux에서 Redux를 사용할 때 활용할 수 있는 Hooks 메서드를 제공한다.


#### useDispatch()

useDispatch() 는 Action 객체를 Reducer로 전달해 주는 'Dispatch 함수를 반환'하는 메서드이다. 

```js
import { useDispatch } from 'react-redux'

const dispatch = useDispatch();
dispatch( increase() );
```

#### useSelector()

useSelector()는 컴포넌트와 state를 연결하여 Redux의 state에 접근할 수 있게 해주는 메서드이다.

```js
import { useSelector } from 'react-redux'
const counter = useSelector(state => state)
```












