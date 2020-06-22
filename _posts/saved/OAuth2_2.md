---
layout: post
title: "OAuth2 공부_2"
category: "doodling"
tags: ["spring", "network"]
---


# OAuth

[딴거 블로그 링크](https://cheese10yun.github.io/spring-oauth2-jdbc/)
[딴거 깃허브 링크](https://github.com/cheese10yun/springboot-oauth2)


## OAuth2 승인 방식의 종류

- Authorization Code Grant Type: 권한 부여 코드 승인 타입
  - 클라이언트가 다른 사용자 대신 특정 리소스에 접근을 요청할 때 사용됨
  - 리소스 접근을 위한 사용자명, 비밀번호, 권한 서버에 요청해서 받은 권한 코드를 활용해 리소스에 대한 액세스 토큰을 받는 방식

- Implicit Grant Type : 암시적 승인
  - 권한 부여 코드 승인 타입(위에꺼)랑 다르게 권한 코드 교환 단계 없이 액세스 토큰을 즉시 반환받아 이를 인증에 이용

- Resource Owner Password Credential Grant Type : 리소스 소유자 암호 자격 증명 타입
  - 전에 글에서 공부한 방식
  - 클라이언트가 암호를 사용해 액세스 토큰에 사용자의 자격 증명을 교환하는 방식

- Client Credentials Grant Type: 클라이언트 자격 증명 타입
  - 클라이언트가 컨텍스트 외부에서 액세스 토큰을 얻어 특정 리소스에 접근 요청
  

> 1, 2 는 귀찮다.. 3, 4만 실습 해보자
=> 나와있는대로 하면 아주 잘 됨 포트번호랑 승인코드만 좀 바꿔주면 된다

#### 이제 3번으로 회원가입 및 로그인 구현...

