---
layout: single
title: "복습일지(4)"
categories: 복습일지
tags: 전예린
---

### 221027 || Json.stringfy & JSON.parse

JSON은 JavaScript Object Notation의 줄임말로, 데이터 교환을 위해 만들어진 객체 형태의 포맷이다. JSON은 특히 네트워크를 통해 서로 다른 시스템들이 데이터를 주고 받을 때 많이 사용된다.


JSON 내장 객체는 JavaScript 객체와 JSON 문자열 간의 상호 변환을 수행해주는 두 개의 메서드인 Json.stringfy 와 JSON.parse를 제공한다. 


#### Json.parse

Json.parse는 JSON 문자열을 JavaScript 객체로 변환할 때 사용한다. parse() 메서드는 JSON 문자열을 인자로 받고 결과값으로 JavaScript 객체를 반환한다.


예를 들어, 아래와 같은 JSON 문자열을 Json.parse()의 파라미터로 전달해준 결과값을 obj라는 변수에 할당해주면,
```js
const str = `{
  "name": "홍길동",
  "age": 25,
  "married": false,
  "family": { "father": "홍판서", "mother": "춘섬" },
  "hobbies": ["독서", "도술"],
  "jobs": null
}`;
```

```js
const obj = JSON.parse(str);
```

obj는 아래와 같은 JavaScript 객체 형태의 값을 가지게 된다.
```js
{
    name: "홍길동",
    age: 25,
    married: false,
    family: {
        father: "홍판서",
        mother: "춘섬"
    },
    hobbies: [
        "독서",
        "도술"
    ],
    jobs: null
}
```
출력 결과를 자세히 관찰해보면 JavaScript 객체와 JSON 문자열 간에는 아주 미묘한 차이가 있는 것을 알 수 있다.
JSON 문자열에서는 키(key)를 나타낼 때 반드시 쌍따옴표로 감싸줘야 하는 반면, JavaScript 객체에서는 꼭 쌍따옴표를 사용할 필요는 없다. 


이렇게 외부에서 문자열의 형태로 주어진 데이터를 해당 언어에서 다루기 용이하도록 내장 데이터 타입으로 변환하는 과정을 역직렬화(deserialization)이라고 부른다.


#### Json.stringify

역으로 JavaScript 객체를 JSON 문자열로 변환할 때는 JSON 객체의 stringify() 메서드를 사용한다. stringify() 메서드는 JavaScript 객체를 인자로 받고 JSON 문자열을 반환한다.

예를 들어, 아래와 같은 JavaScript 객체를 Json.parse()의 파라미터로 전달해준 결과값을 str이라는 변수에 할당해주면,

```js
const obj = {
  name: "홍길동",
  age: 25,
  married: false,
  family: {
    father: "홍판서",
    mother: "춘섬",
  },
  hobbies: ["독서", "도술"],
  jobs: null,
};
```
```js
const str = JSON.stringify(obj);
```
str은 아래와 같은 문자열 형태의 값을 가지게 된다.
```js
{"name":"홍길동","age":25,"married":false,"family":{"father":"홍판서","mother":"춘섬"},"hobbies":["독서","도술"],"jobs":null}
```

변환해야하는 JavaScript 객체가 많은 양의 데이터를 담고 있는 경우에는 이렇게 한 줄의 문자열로 변환되면 알아보기 힘들어지는데,
이럴 때는 stringify() 메서드의 3번째 인자로 들여쓰기할 공백의 크기를 지정해줄 수 있다.

JSON.stringify(obj)의 결과값을 2개의 공백으로 들여쓰기가 된 형태로 볼 수 있도록 설정해주면,

```js
const str2 = JSON.stringify(obj, null, 2);
```

데이터를 한 눈에 알아보기 쉽게 만들어줄 수 있다.

```js
{
  "name": "홍길동",
  "age": 25,
  "married": false,
  "family": {
    "father": "홍판서",
    "mother": "춘섬"
  },
  "hobbies": [
    "독서",
    "도술"
  ],
  "jobs": null
}
```

이렇게 특정 언어의 내장 타입의 데이터를 외부에 전송하기 용이하도록 문자열로 변환하는 과정을 직렬화(serialization)이라고 부른다.
