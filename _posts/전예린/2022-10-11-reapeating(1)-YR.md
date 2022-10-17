---
layout: single
title: "전예린 복습일지(1)"
categories: 복습일지
tags: 전예린
---

### 221011 

#### 리차드슨의 REST 성숙도 모델

레너드 리처드슨(Leonard Richardson)의 성숙도 모델은 REST의 성숙도를 총 4단계로 구분하는 유용한 모델이며, 2단계까지만 지켜져도 좋은 API 디자인이라고 한다.

0단계에서는 server와 client간의 상호작용에 있어서 HTTP 프로토콜을 사용해야 한다.
<br/>
1단계에서는 개별 리소스(Resource)와의 통신을 준수해야 한다. 모든 자원은 개별 리소스에 맞는 엔드포인트(Endpoint)를 사용해야 하며, 요청하고 받는 자원에 대한 정보를 응답으로 전달해야 한다는 것이 핵심이다.
<br/>
2단계에서는 CRUD에 맞게 적절한 HTTP 메서드를 사용해야 한다. HTTP 메서드에는 GET, POST, PUT, PATCH, DELETE가 있다.
<br/>
마지막으로 3단계에서는 응답에 리소스의 URI를 포함한 링크 요소를 삽입하여 작성해야 한다.
