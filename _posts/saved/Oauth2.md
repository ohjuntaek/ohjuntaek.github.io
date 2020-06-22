---
layout: post
title: "oauth 공부"
date: 2019-07-12
categories: "doodling"
tags: [web, network]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---

# Spring Securit Oauth2 개념 정리 및 구현

- [참고 블로그 링크](
https://minwan1.github.io/2018/02/24/2018-03-11-Spring-OAuth%EA%B5%AC%ED%98%84/)
-[참고 깃헙 링크](https://github.com/minwan1/spring-security-oauth2-example/)

## Oauth란?

- Open Authorization, Open Authentication, 어또리제이션(인증), 어뗸티케이션(인가)를 줄인거..
- 자신의 앱 서버의 데이터로 다른 서드 파티에 자원을 공유하거나 대신 유저 인증을 처리해 줄 수 있는 오픈 표준 프로토콜
- 말이 어렵다.. 예시 보고 정의하자

### OAuth 실 사용 예

- SNS 로그인
- 유저가 인증됨(아디 비번이 맞다) -> 액세스 토큰 받는다 -> 서드파티 앱에 유저를 인증시킨다
  - 자꾸 서드파티 앱이라고 나오네.. 서드파티가 따로 있어야 하는건가?
- 인증뿐 아니라 인가 기능도 있다. 인가 기능을 이용하면 써드파티 앱들은 유저가 선택한 로그인 방식에 애플리케이션의 유저 정보 등을 얻을 수 있다. 이 정보를 바탕으로 써드파티 앱들은 유저를 자신의 앱에 가입시킨다. 뿐만 아니라 서드파티 앱들은 유저가 로그인한 애플리케이션의 기능 등을 이용할 수 있다
  - 로켓펀치의 다양한 로그인 방식 예시가 나온다. 서드 파티가 내가 운영하는 클라이언트 앱이라 생각하고(로켓펀치), 로그인한 애플리케이션이 OAuth 서버(페이스북)라 생각하면 되겠네..
  - 예시는 로켓펀치에서 페북 로그인 하면 친구 정보 가져올 수 있는 것
- 또한 MSA에 인증 방식으로도 사용된다

### Auth2의 역할

- Resource Owner(유저), Authorization Server(인증 서버), Resource Server(자원이 있는 서버), Client(써드파티 앱)

### Resource Owner Password Credentials Grant 동작 방식

1. AuthorizationServer에 Client-id와 Secret을 등록한다(그림에는 안나와있음)
2. User가 Id와 Password를 입력한다
3. 클라이언트는 유저의 id와 password와 클라이언트 정보를 넘긴다.
4. Authorization sever는 Access token을 넘긴다.
5. Client는 token로 자원서버에 접근할 수 있게 된다.

## Spring에서 OAuth2 구현

### OAuth2 in-memory

- OAuth에서는 인증 서버와 자원 서버를 하나의 애플리케이션에서 구현할 수도 있고, 분리할 수도 있다
- 분리한다면 서로간의 네트워크 통신이 필요하겠지?
- 여튼, 여기서는 하나에서 한다. Resource Owner, Client 관리는 in-memory를 이용한다.
- 이건 뭐.. 그냥 /users에 자원/인증서버 설정하고 테스트에 토큰 넣고.. 그냥 테스트 하는거

### OAuth2 JdbcTokenStore등을 이용한 영속화

- 일단 스키마를 만들어야 한다.. 스키마를 보자

#### 스키마 작성

- oauth_client_details : 뭐가 이래 많어?? 여튼 클라이언트 정보.. 뭐 여러 개 있다 테스트 용이란다
- oauth_client_token : 클라이언트 토큰 정보인가?? (클라이언트, 유저)에 대한 토큰이 들어가는 듯..
- oauth_access_token : 이것도 토큰이네?? 이거는 인증 받고나서 받은 토큰.. 위에꺼는 그럼 뭐에쓰는거야?
- oauth_refresh_token : 리프래쉬? 이거 많이 들어봤는데..
- oauth_code : 뭐야이건
- oauth_approvals : 뭐가이래 많어
- ClientDetails : 위에 oauth_client_details를 커스터마이징 한거(컬럼보면 거의 똑같음)

> 보니깐 거의 모르겠다.. spring security oauth에 대한 스키마라는데.. 다 필요한건가? 일단 넘어가자


#### TokenStore의 빈 등록

- TokenStore는 아마 Spring-security-oauth에서 제공하는 빈인거 같은데 
- TokenStore에 빈을 등록하게 되면 Token 관리, Client 관리 등을 디비로 할 수 있게 된단다..

#### 인증, 자원 서버 분리

- 자원에 HttpSecuriry 콘피겨에는 자원 서버의 접근 권한 설정.. 토큰이 필요해 지겠지?
- 인증에는 인증서버자체, 클라이언트, 엔드포인트 콘피겨가 잇는데..


> 스프링 1.5.9인가? 라서 좀 안 맞네.. 딴거 본다




