---
layout: single
title: "전예린 복습일지(3)"
categories: 복습일지
tags: 전예린
---

### 221017 || request 프로퍼티 : req.body() / query() / params()의 차이
<br/>

Express로 서버를 구현 할 때, request의 프로퍼티들을 사용하여 우리가 원하는 값을 불러온다.
<br/>
대표적인 request 프로퍼티인 req.body() / query() / params()의 차이점에 대해 알아보자.
<br/>

- req.params()

라우트 파라미터들을 포함한다.
<br/>
만약 요청온 url이 www.example.com/public/100/jun 이라면, 

```js
router.get('/:id/:name', (req, res, next) => {
  console.log(req.params) // { id: '100', name: 'jun' }
});
```
req.params 값은 { id: '100', name: 'jun' } 이 된다.


- req. query()

url 쿼리 파라미터들을 포함하며, 주로 GET 요청을 처리할 때 사용한다.
<br/>
만약 요청온 url이 www.example.com/public/100/jun?title=hello! 이라면,
```js
router.get('/:id/:name', (req, res, next) => {
  console.log(req.query) // { title : 'hello!' }
});
```
req.query 값은 { title : 'hello!' } 이 된다.


- req.body()

주로 POST/PUT 요청을 처리할 때, 유저 정보 같은 JSON 등의 바디 데이터를 담을 때 사용한다.
<br/>

클라이언트에서 다음과 같이 요청을 보내면,

```js
await axios({
  method: "post",
  url: `www.example.com/post/1/jun`,
  data: { // post 로 보낼 데이터
    name: 'nomad',
    age: 11,
    married: true
  },
})
```

req.body는 { name: 'nomad', age: 11, married: true }가 된다.

```js
router.post('/:id/:name', (req, res, next) => {
// post 요청 시 담은 객체 부분이 담긴다.
  console.log(req.body) // { name: 'nomad', age: 11, married: true }
});
```
