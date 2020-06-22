---
layout: post
title: Daily - Spring Security OAuth 2.0 Roadmap Update 훑어보기
category : daily
tag: [spring, security, oauth]
---

- [원문 링크](https://spring.io/blog/2019/11/14/spring-security-oauth-2-0-roadmap-update)

### NOTE

- [지난 Announcing 링크](https://spring.io/blog/2019/11/14/spring-security-oauth-2-0-roadmap-update)
- [스프링 시큐리티 OAuth2.0 다음 세대 링크](https://spring.io/blog/2018/01/30/next-generation-oauth-2-0-support-with-spring-security)
    - 이 글은 위 링크의 follow-up 포스트이다.

## Current State 

- [Spring-Security_OAuth2.0-Feature-Matrix](https://github.com/spring-projects/spring-security/wiki/OAuth-2.0-Features-Matrix)
    - 자주하는 질문
        1. 스프링 시큐리티가 2.0에서 미래에 지원되나요?
        - 시큐리티 5 전체에 다음 세대의 OAuth2.0이 지원됩니다.
        - 시큐리티 5.2 부터는 OAuth 2.0 Login, Client, Resource Server가 내장되어 있습니다.
        2. 생략
        3. Spring Security 5 OAuth 2.0 샘플 어디서 찾습니까?
        - [프로젝트](https://github.com/spring-projects/spring-security/tree/5.2.0.RELEASE/samples/boot/oauth2login) - 봐도 모르겠다.
        - [DOCS](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2login) - 매우 좋음
        4. Spring Security OAuth 2.3+에 새로운 기능이 구현되있습니까?
        - minor enhancements(bug/security fixes)를 더하는 것을 계속 제공중이다. 우리는 오어쓰랑 시큐리티 5.x랑 기능 동일성(feature parity)을 목표로 가고 있다. 시큐리티랑 오어쓰랑 기능이 같아지고 나면, 우리는 최소 1년동안 버그/보안 업데이트를 계속할 것이다.
        5. 스프링부트 2.0이 Spring Security OAuth2.0을 지원하나요?
        - 스프링부트 2.0은 Spring Securty OAuth 지원을 드랍중이다. 그러나 OAuth2.0 로그인, 클라이언트, Resource Serer는 시큐리티 5에서 지원한다.
        6. 스프링부트 2.0이랑 Spring Security OAuth 2.0을 통합하는 방법은?
        - > 링크에 답변 보면 잘 나와있음... 생략
- 위와 같이 Feature Parity가 5.2에서 거의 다 됐고(Client, Resource Server), 남은건 별로 없는데 5.3 릴리즈 때 끝내길 기대한다.

## No Authorization Server Support

- RFC 6749라는 OAuth 2.0에 대한 문서가 2012년에 나왔다. 
- 그후 2014년에 스프링-시큐리티-오어쓰 2.0.0 버전이 나왔다.
- 시큐리티의 인증 서버는 적합하지 않다.
- 인증 서버는 제품을 빌드할때 라이브러리를 필요로한다.
- 시큐리티는 프레임워크다.
- 예시로, 우리는 jwt 라이브러리가 없고, Nimbus로 쉽게 사용할 수 있게 해준다.
- 2019년에, 인증 서버는 상업/오픈소스가 넘쳐난다.
- > 그러니깐 인증서버는 있는거 갖다써라
> 하지만 커뮤니티에 피드백 보니깐 한번 더 검토해야겠다는 생각이 들어서 검토 중인 부분

## Support Lifetime for Spring Security OAuth 2.x
- 위에 적은대로 끝나간다... 시큐리티 5.2로 갈아타라
- 자세한건 링크 참조

## 리뷰

- 시큐리티 꾸준히 주시하자. 버전 몇 써야할지 감이 안 온다.. 
- 인증 서버 지원 여부는 참 재미있는 주제다.
- 공식 샘플 프로젝트 보기가 왜이렇게 어렵지? 한번 날잡고 파야겠다.


    