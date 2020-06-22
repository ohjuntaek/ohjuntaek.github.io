---
layout: post
title: "jwt 공부"
date: 2019-07-11
categories: "doodling"
tags: [web, network]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---
# JWT란? OAuth란? spring-security? 회원가입?
- JWT : 헤더(header), 내용(payload), 서명(signature)를 통해 뭐 인증?인가를 하는것.. 인가

### 공부 전 내 생각..
- 아오 실컷 뇌피셜 세웠는데 모르것네

### 세션/쿠키 vs jwt 의문점 정리
[의문점많았던링크](https://12bme.tistory.com/130)
1. 세션하이재킹.. http에서 토큰 갈취하면 어차피 정보 날라올텐데 쿠키보내는거랑 보안성에 뭔차이냐?
=> 생각해보면 쿠키를 갈취하거나, 토큰을 갈취하면 내가 나라는 정보가 거기 있으니 둘다 탈취되면 안되겄네 아마도??

2. 세션에 저장할거를 걍 토큰 안에 때려박아서 암호화하는거 아냐?? 이게 좋다고?
- 뭐 마이크로서비스나 rest api monolothic 등 서버를 여러개 둬야하는 경우 세션으로 하면 동기화 등등 그래서 oauth 서버 두거나 jwt로 토큰에 걍 정보 저장.. 방식의 차이인데 머가 좋을라나

### 여튼 안전하고 편리한 회원가입, 로그인을 위해서 어떻게 해야 할까? 단계별로 가자
1. 세션/쿠키는 많이 함
2. 일단 https 부터 해야하는데 나중에 하자
3. oauth가 개념 정립하기 좋네 시큐리티 적용해서 구현하자
4. 시큐리티 롤 적용해서 하자
5. 페이스북, 구글 oauth랑 디비 유저랑 연동하자
6. jwt 구축하자
----
6. 직접 Oauth 서버 구축.. 은 몰겠다 할란가??
[만약 하게 된다면 여기 링크 보고..](https://brunch.co.kr/@sbcoba/6)


# [조대협 블로그](https://bcho.tistory.com/999) 정리
- rest api니깐.. 왜 세션 쿠키 안쓰고 이런거 쓰냐? [여기 정리 잘됨](https://sanghaklee.tistory.com/47)



