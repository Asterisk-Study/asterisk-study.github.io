---
title: "복습일지(5)"   
layout: post    
comments: true  
categories: [전예린/복습일지]
tags: Json메소드
---

### 221031 || CSS-in-JS와 Styled components


애플리케이션으로 개발 방향이 진화하면서 컴포넌트 단위의 개발은 캡슐화의 중요성을 불러왔다. 
하지만 CSS는 컴포넌트 기반의 방식을 위해 만들어진 적이 한 번도 없어, CSS도 컴포넌트 영역으로 불러들이기 위해 'CSS-in-JS'가 만들어지게 되었다.


CSS-in-JS 라이브러리를 사용하면 CSS도 쉽게 Javascript 안에 넣어줄 수 있으므로, HTML, JS, CSS를 묶어 하나의 JS파일 안에서 컴포넌트 단위로 개발할 수 있게 된다.


대표적인 CSS-in-JS에는 'Styled-Component'가 있다.


Styled-Component는 컴포넌트 기반으로 CSS를 작성할 수 있게 해주는 라이브러리이다. CSS를 컴포넌트 안으로 캡슐화하였고, 네이밍이나 최적화를 신경쓰지 않아도 된다는 장점이 있다.


Styled-Component의 사용법은 이렇다.

npm install을 통해 설치 후,

```js
npm install --save styled-components
```

Styled Components를 사용할 파일로 불러와주면 된다.
```js
import styled from "styled-components"
```

이제 Styled-Component의 문법에 대해 알아보자.


Styled-Component는 ES6의 Templete Literals 문법을 사용한다. 즉, 따옴표가 아닌 백틱(`)을 사용한다.

컴포넌트를 선언한 후 styled.태그종류를 할당하고, 백틱 안에 기존에 CSS를 작성하던 문법과 똑같이 스타일 속성을 작성해주면 된다.


이렇게 만든 컴포넌트를 React 컴포넌트를 사용하듯 리턴문 안에 작성해주면 스타일이 적용된 컴포넌트가 렌더되는 것을 확인할 수 있다.
```js
import styled from "styled-components";

const BlueButton = styled.button`
  background-color: blue;
  color: white;
`;

export default function App() {
  return <BlueButton>Blue Button</BlueButton>;
}
```

<img width="188" alt="스크린샷 2022-10-31 오후 2 33 29" src="https://user-images.githubusercontent.com/39157466/198938454-300a775b-d9a7-49b3-b0fd-7fbd60a30a9a.png">


이미 만들어진 컴포넌트를 재활용해서 새로운 컴포넌트를 만들 수도 있다. 


컴포넌트를 선언하고 styled() 에 재활용할 컴포넌트를 전달해준 다음, 추가하고 싶은 스타일 속성을 작성해주면 된다.
```js
import styled from "styled-components";

const BlueButton = styled.button`
  background-color: blue;
  color: white;
`;

const BigBlueButton = styled(BlueButton)`
  padding: 10px;
  margin-top: 10px;
`;

export default function App() {
  return (
    <>
      <BlueButton>Blue Button</BlueButton>
      <br />
      <BigBlueButton>Big Blue Button</BigBlueButton>
    </>
  );
}
```
<img width="255" alt="스크린샷 2022-10-31 오후 2 33 07" src="https://user-images.githubusercontent.com/39157466/198938397-1a240435-eea8-42f1-a0a1-195ec75f53d7.png">


Styled Component로 만든 컴포넌트도 React 컴포넌트처럼 props를 내려줄 수 있다. 


받아온 props로 조건부 렌더링을 해줄 수도 있고,
```js
import styled from "styled-components";
import GlobalStyle from "./GlobalStyle";
//받아온 prop에 따라 조건부 렌더링이 가능합니다.
const Button1 = styled.button`
  background: ${(props) => (props.skyblue ? "skyblue" : "white")};
`;

export default function App() {
  return (
    <>
      <GlobalStyle />
      <Button1>Button1</Button1>
      <Button1 skyblue>Button1</Button1>
    </>
  );
}
```

<img width="267" alt="스크린샷 2022-10-31 오후 2 35 11" src="https://user-images.githubusercontent.com/39157466/198938619-943185ea-ce4f-453f-81be-676bfb69c558.png">


props의 값을 통째로 활용해서 컴포넌트 렌더링에 활용할 수 있다.
```js
import styled from "styled-components";
import GlobalStyle from "./GlobalStyle";

// 방법 1
const Button1 = styled.button`
  background: ${(props) => (props.color ? props.color : "white")};
`;
// 방법 2
const Button2 = styled.button`
  background: ${(props) => props.color || "white"};
`;

export default function App() {
  return (
    <>
      <GlobalStyle />
      <Button1>Button1</Button1>
      <Button1 color="orange">Button1</Button1>
      <Button1 color="tomato">Button1</Button1>
      <br />
      <Button2>Button2</Button2>
      <Button2 color="pink">Button2</Button2>
      <Button2 color="turquoise">Button2</Button2>
    </>
  );
}
```
<img width="402" alt="스크린샷 2022-10-31 오후 2 40 34" src="https://user-images.githubusercontent.com/39157466/198939296-16f5680b-babd-4589-bf0a-f6133e6d17fe.png">


전역에 스타일을 설정하는 것도 가능하다.


전역 스타일을 설정하기 위해 Styled Components에서 createGlobalStyle 함수를 불러온 뒤, 이 함수를 사용하여 설정해주고 싶은 스타일을 작성한다.
```js
import { createGlobalStyle } from "styled-components";

const GlobalStyle = createGlobalStyle`
	button {
	padding : 5px;
	margin : 2px;
	border-radius : 5px;
	}
 ```
 
 작성을 마치고 <GlobalStyle> 컴포넌트를 최상위 컴포넌트에서 사용해주면 원하는대로 전역에 <GlobalStyle> 컴포넌트의 스타일이 적용된다.
  
```js
function App() {
	return (
		<>
		<GlobalStyle />
		<Button>전역 스타일 적용하기</Button>
		</>
	);
}
```
