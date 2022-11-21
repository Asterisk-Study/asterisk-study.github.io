---
title: "OAuth"
layout: post
comments: true
categories: [강진원/복습일지]
tags: [OAuth, 소셜로그인]
---

# OAuth
OAuth는 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹사이트 상의 자신들의 정보에 대해 웹사이트나 애플리케이션의 접근 권한을 부여할 수 있는 공통적인 수단으로서 사용되는, 접근 위임을 위한 개방형 표준이다. 예를들자면 소셜로그인 같은 것이 해당된다.

OAuth는 서버에서 인증을 처리해주는 것과는 달리, 인증을 중개해주는 매커니즘이다. 

OAuth 프로토콜을 이용함으로 써 서비스를 구현하는 개발자는 신규회원가입이나 회원 관리를 따로 신경쓰지 않아도 된다는 이점이 있다. 또 유저 입장에서는 아이디와 비밀번호를 통일하여 작성하는 유저도 있는 반면, 사이트마다 다르게 하거나 비밀번호를 잊어버려 매번 비밀번호 찾기를 사용하는등 이런 불편함을 하나의 아이디로 통합하여 관리할 수 있다는 장점이 있다. 뿐만아니라 OAuth는 보안상의 이점도 있다. 검증되지 않은 App에서 OAuth를 사용하여 로그인한다면, 직접 유저의 민감한 정보가 APP에 노출될 일이 없고 인증 권한에 대한 허가를 미리 유저에게 구해야 하기 떄문에 더 안전하게 사용할 수 있다.

### OAuth에서 꼭 알아야 할 용어
* Resource Owner : 사용자이며 정보 제공자
* Client : Resource Owner를 대신하여 보호된 리소스에 액세스하는 애플리케이션
* Local Server : Client의 요청을 수락하고 응답할 수 있는 서버
* Resource Server : 사용자의 정보를 저장하고 있는 서버
* Authorization Server : 인증을 담당하고 있는 서버, Access Token을 발급하는 인증 서버
* Authorization Grant : Client가 Access Token을 얻는 방법을 의미
* Authorization Code : Authorization Grant의 한 타입으로 Access Token을 발급받기 위한 code를 의미
* Access Token : 보호된 리소스에 액세스하는데 사용되는 인증 토큰, 이 Access Token으로 이제 Resource Server에 접근
* Refresh Token : 발급받은 Access Token이 만료될 시 Refresh Token을 통해 새로운 Access Token을 발급받을 수 있다.
### OAuth 인증 흐름
Authorization Grant에는 다음과 같은 방법들이 주로 사용된다.

* Authorization Code Grant Type   
Authorization Code를 받아 Authorization Code를 통해 Access Token을 받는 방식을 말한다. 이 유형은 Access Token이 사용자나 브라우저에 표시되지 않는다는 것을 의미하므로 Access Token이 다른사람에게 누출될 위험이 줄어든다. 
* Refresh Token Grant Type   
Authorization Code Grant Type으로 Access Token을 발급 받은 후 Access Token이 만료된 경우 Refresh Token을 활용해 새로운 Access Token으로 교환하는데 사용된다. 이를 통해 사용자와의 추가 상호 작용 없이 계속 유효한 액세스 토큰을 가질 수 있다.
 
래퍼런스
https://ko.wikipedia.org/wiki/OAuth