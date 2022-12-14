---
title: "윤태연 복습일지(1): 웹의 구조"
layout: post
comments: true
categories: [윤태연/복습일지]
tags: 윤태연
---

# 웹의 구조: 2022 / 10 / 11

---

## 사례

윤태연은 코트를 사고싶어 크롬 주소창에 무신사 URL인 https://www.musinsa.com/app/ 를 쳐서 무신사로 접속하였고, 검색해 코트의 카테고리를 본다음 이것 저것 장바구니에 담지만 결국 사지는 못하고 짠한 잔고만 본다.

## 설명

여기서 윤태연과 윤태연이 쓰는 크롬브라우저는 사용자, 즉 클라이언트를 의미한다. 그러면서 원하는 사이트의 URL(도메인)을 입력하여 서버에게 요청한다. 서버는 이를 승인하고 윤태연이 봐야하는 홈페이지의 데이터를 클라이언트에게 보낸다. 그 뿐만 아니라 클라이언트가 접속한 후 요청하는 명령이 쏟아지기 시작한다?!

1. 검색 요청
2. 카테고리의 아이템 목록 불러오기 요청
3. 전체보기 시 추가 아이템 요청
4. 장바구니 담기 요청 등등

이처럼 1. 클라이언트가 요청을 보내고, 2. 서버가 받는 구조는 웹이 기본적으로 통신하는 구조다. 이를 이중으로 상호작용하는 구조라 해서 '2티어 아키텍쳐'라고 명명한다.

또한 '무신사'는 트래픽이 다량 발생하는 큰 웹이기때문에 2티어 아키텍쳐로는 서버만으로는 로딩 속도도 떨어지고, 불러올 수 있는 리소스양이 적는 등 규모가 커질 시 생기는 문제가 발생할 것이다. 그래서 서버가 바로바로 가져올 수 있는 'DB'라는 것을 따로 둬 요청시 서버가 바로바로 요구에 맞는 리소스를 빼와서 넣을 수 있게 만든다. 이것이 '3티어 아키텍쳐'다!
