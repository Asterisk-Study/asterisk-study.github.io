---
title: "복습일지(7)"   
layout: post    
comments: true  
categories: [전예린/복습일지]
tags: Redux
---

### 221115 || 인증 & 보안

#### 쿠키

쿠키는 서버에서 클라이언트에 영속성있는 데이터를 저장하는 방법이다. 서버가 클라이언트에게 보내는 4kB이하의 작은 데이터로 http의 stateless로 인한 불편함을 해소하기 위해 만들어졌다.

보안 대책 없이 사용할 경우 중간에 탈취당해 내용이 알려질 위험성이 있기에 세션이나 토큰등 보안 대책이 필요하다. 쿠키는 서버에서 MaxAge, HttpOnly 등과 같은 속성을 설정할 수 있다.

서버에서 속성들을 지정한 다음, 서버에서 클라이언트로 쿠키를 처음 전송하게 된다면 헤더에 Set-Cookie라는 프로퍼티로 쿠키를 담아 전송한다.
이후 클라이언트에서 서버에게 쿠키를 전송해야 한다면 클라이언트는 헤더에 Cookie라는 프로퍼티에 쿠키를 담아 서버에 쿠키를 전송하게 된다.

이러한 쿠키의 특성을 이용하여 서버는 클라이언트에 인증정보를 담은 쿠키를 전송하고, 클라이언트는 전달받은 쿠키를 서버에 요청과 함께 전송하여 Stateless한 인터넷 연결을 Stateful하게 유지할 수 있다.
하지만 기본적으로 쿠키는 오랜 시간 동안 유지될 수 있고, HttpOnly 옵션을 사용하지 않았다면 자바스크립트를 이용해서 쿠키에 접근할 수 있기 때문에 쿠키에 민감한 정보를 담는 것은 위험하다.
<br/>
<br/>

#### 세션

세션은 쿠키에 정보를 담는 것보다 안전하게 서버에 정보를 저장하는 방법이다.

서버는 사용자가 인증에 성공하면 로그인 상태를 유지하기 위해 해당 사용자의 정보를 담은 세션 객체를 생성한다. 
그리고 이에 접근할 수 있는 id를 암호화하여 쿠키로 사용자에게 전달한다.
이후에 사용자는 로그인 할 필요없이 쿠키 속 세션ID를 통해 편리하게 활동 할 수 있다. 

만일 사용자가 로그아웃한다면 임의의 의미없는 값을 가진 세션ID를 포함한 쿠키를 전송하고 서버의 세션과 관련한 정보를 삭제해 주면 된다. 

세션은 신뢰할 수 있는 유저인지 서버에서 추가로 확인 가능하다는 장점이 있지만, 하나의 서버에서만 접속 상태를 가지므로 분산에 불리하다는 단점이 있다.
만약 애플리케이션을 확장하고 싶을 경우 세션은 적절하지 않는 선택이 될 수 있다.

Node.js에는 이런 세션을 대신 관리해 주는 'express-session' 이라는 모듈이 존재한다.
express-session은 세션을 위한 미들웨어로, express 서버에서 쉽게 세션을 위한 공간을 다룰 수 있도록 만들어준다.
<br/>
<br/>

#### 토큰

세션 기반 인증은 서버에 유저정보를 담는 인증 방식이었다.
서버에서는 제한된 정보를 유저가 요청할 때마다 정보를 줘도 되는지 확인하기 위해서 서버가 가지고 있는 세션 값과 일치하는지 확인해야 했다. 이 확인 작업을 덜어 줄 수 있는 것이 토큰이다.
토큰 기반 인증 중 대표적인 방법으로는 JWT (JSON Web Token)가 있다.

클라이언트는 XSS, CSRF 공격에 노출이 될 위험이 있음에도 불구하고 인증에 사용되는 토큰을 클라이언트에 담아도 되는 이유는 토큰에 담긴 유저 정보가 암호화된 상태이기 때문이다.

세션에서 사용자 인증을 위해 세션ID와 같은 특별한 값을 서버와 클라이언트 모두에게 저장했던 것과 달리 
토큰에서는 암호화 키의 알고리즘을 통해 토큰이 유효한지 검사 할 수 있기 때문에 클라이언트에만 인증정보를 보관하면 된다. 즉 암호화 키는 서버에, 토큰은 클라이언트에 저장되는 것이다.

JWT에는 두가지의 토큰이 존재한다.

1. Access Token
2. Refresh Token

Access token은 보호된 정보(유저의 이메일, 연락처, 사진 등)에 접근할 수 있는 권한 부여에 사용한다.
클라이언트가 처음 인증을 받게 될 때(로그인 시) access, refresh token 두 가지를 다 받지만, 실제로 권한을 얻는 데 사용하는 토큰은 access token이다.<br/>


![image](https://user-images.githubusercontent.com/39157466/201910025-74649ddc-2dae-480c-9af7-75d311948885.png)

JWT는 위 그림과 같이 . 으로 나누어진 세 부분이 존재한다.

header는 이용하는 hashing 알고리즘과 토큰의 종류가 적혀 있다.
payload에는 클라이언트에 제공하고자 하는 정보가 담겨 있다.
signature은 base64로 인코딩된 header와 payload사이에 salt를 삽입한 후 한번 더 암호화한 것이다. 
<br/>
<br/>

발급 및 토큰 사용 순서는 다음과 같다.

1. 유저가 로그인을 한다.
2. 서버는 암호화 키를 통해 사용자 정보 등 보내고 싶은 내용을 암호화 하여 보낸다. 예를 틀어 JWT(json web token)를 이용한다면 'header.payload.signature'로 보낼 수 있다.
3. access token, refresh token이 발급된다.
4. access token을 이용하여 서버의 api를 이용한다.
5. access token이 만료시 refresh token을 이용하여 다시 access token을 발급 받을 수 있다.
6. 둘 다 만료시 다시 로그인하여 위 과정을 반복한다.

토큰 기반 인증은 무상태성 & 확장성, 안전함, 권한 부여가 쉬움 등과 같은 장점이 있지만 용량이 크다는 단점이 있다.
<br/>
<br/>

#### OAuth

전통적으로 직접 작성한 서버에서 인증을 처리해주는 것과는 달리, OAuth는 인증을 중개해주는 메커니즘이다. 
보안된 리소스에 액세스하기 위해 클라이언트에게 권한을 제공하는 프로세스를 단순화하는 프로토콜이다.

즉, 이미 사용자 정보를 가지고 있는 웹 서비스(GitHub, Google, Facebook 등)에서 사용자의 인증을 대신해주고, 접근 권한에 대한 토큰이 발급되면 이를 이용해 내 서버에서 인증이 가능해진다.
<br/>
<br/>

OAuth에서 자주사용되는 용어들은 다음과 같다.

- Resource Owner: 사용자이며 정보 제공자이기도 하기 때문에 Resource Owner라고 한다.
- Client: Resource Owner를 대신하여 보호된 리소스에 액세스하는 애플리케이션이다.
- Local Server: Client의 요청을 수락하고 응답할 수 있는 서버이다.
- Resource Server: 사용자의 정보를 저장하고 있는 서버이다.
- Authorization Server: 인증을 담당하고 있는 서버이다. Access Token을 발급하는 인증 서버이다.
- Authorization Grant: Client가 Access Token을 얻는 방법을 의미한다. 다음과 같은 방법들이 주로 사용된다.
  1. Authorization Code Grant Type
  2. Refresh Token Grant Type
- Authorization Code: Authorization Grant의 한 타입으로 Access Token을 발급받기 위한 Code를 의미한다.
- Access Token: 보호된 리소스에 액세스하는 데 사용되는 인증 토큰이다. 이 Access Token으로 이제 Resource Server에 접근할 수 있다.
- Refresh Token: 발급받은 Access Token이 만료될 시 Refresh Token을 통해 새로운 Access Token을 발급받을 수 있다.
<br/>

OAuth 인증 흐름 두 가지를 살펴보자.

첫 번째, Authorization Code Grant Type은 Authorization Code를 받아 그것을 통해 Access Token을 받는 방식을 말한다. 

이 유형은 Access Token이 사용자나 브라우저에 표시되지 않는다는 것을 의미하므로 Access Token이 다른 사람에게 누출될 위험이 줄어든다.

![image](https://user-images.githubusercontent.com/39157466/201911243-78f1fbde-0292-4c55-8346-19d991b78934.png)

두 번째, Refresh Token Grant Type은 Access Token을 발급받은 후 Access Token이 만료된 경우 Refresh Token을 활용해 새로운 Access Token으로 교환하는 데 사용된다. 

이를 통해 사용자와의 추가 상호 작용 없이 계속 유효한 액세스 토큰을 가질 수 있다.

![image](https://user-images.githubusercontent.com/39157466/201911494-406a4850-2c58-459b-9a06-bfa873c0ab76.png)







