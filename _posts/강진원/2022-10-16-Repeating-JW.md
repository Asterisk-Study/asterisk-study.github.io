---
layout: single
title: "강진원 복습일지"
categories: 복습일지
tags: 강진원
jinwon: 복습일지
---
# 2022-10-16 SUN
## AJAX 
JavaScript, DOM, Fetch, XMLHttpRequest, HTML 등의 다양한 기술을 사용하는 웹 개발 기법

AJAX (Asynchronous JavaScript And XMLHttpRequest)란 비동기 자바스크립트와 XML을 말한다. 서버와 통신하기 위해 XMLHttpRequest(https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest) 객체를 사용하는 것을 말하며, <span style="color: #2D3748; background-color:#fff5b1;">**서버와 비동기적으로 데이터를 주고받는 자바스크립트 기술을 의미**한다.</span>

AJAX의 강력한 특징은 페이지 전체를 refresh 하지 않고서도 수행 되는 <span style = "color: red;">“비 동기성”</span>이다.
쉽게 말해서 클라이언트가 서버로부터 GET요청을 하고 응답을 받을 때, 새로 고침 없이 수행 될 수 있게 해준다.

### AJAX의 주요 두가지 특징
- 페이지 새로고침 없이 서버에 요청
- 서버로부터 데이터를 받고 작업을 수행

### AJAX의 두가지 핵심 기술
- JavaScript와 DOM
- Fetch    
Fetch라는 내장 함수를 사용하면, 페이지를 이동하지 않아도 서버로부터 필요한 데이터를 받아 올 수 있다. Fetch는 사용자가 현재 페이지에서 작업을 하는 동안 서버와 통신할 수 있도록 한다.
즉, 브라우저는 Fetch가 서버에 요청을 보내고 응답을 받을 때까지 모든 동작을 멈추는 것이 아니라 계속해서 페이지를 사용할 수 있게 하는 비동기적인 방식을 사용한다.
Fetch를 통해 전체 페이지가 아닌 필요한 데이터만 가져와 DOM에 적용 시켜 새로운 페이지로 이동하지 않고 기존 페이지에서 필요한 부분만 변경할 수 있다.


### AJAX의 장점
- 서버에서 HTML을 완성하여 보내주지 않아도 웹페이지를 만들 수 있다.
- 표준화된 방법 (XHR이 표준화 되면서부터 브라우저에 상관없이 AJAX를 사용할 수 있게 되었다.
- 유저 중심 애플리케이션 개발
- 더 작은 대역폭(대역폭 : 네트워크 통신 한 번에 보낼 수 있는 데이터의 크기)
### AJAX의 단점
- Search Engine Optimization(SEO)에 불리 (AJAX 방식의 웹 애플리케이션의 HTML파일은 뼈대만 있고, 데이터는 없기 때문에 사이트 정보를 긁어가기 어렵다. => 서버로부터 비동기적으로 필요한 데이터를 가져 오기때문에 처음 받는 HTML 파일에는 데이터를 채우기 위한 틀만 작성되어 있는 경우가 많음)
- 뒤로가기 버튼 문제 (AJAX에서는 이전 상태를 기억하지 않는다)
