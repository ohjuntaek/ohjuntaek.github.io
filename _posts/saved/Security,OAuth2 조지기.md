# 제대로 Spring Security, OAuth2 조지기

## 소개

- 인증(Authentication), 인가(Authorization).. 영어 맨날 헷갈리네.. 인증은 로그인 인가는 권한 부여
- 시큐리티 아키텍쳐 이해하는게 중요!
- CSRF, XXS, 세션 변조, Clickjacking 뭐 이런거 언제 아냐
- 폼 인증 기반으로 설명

## 1강. 폼 인증 예제 살펴보기

- Principal : 시큐리티의 인터페이스, 인증된 사용자가 있다면 Spring MVC 핸들러에서 ArgumentResolver가 argument로 쓸 수 있게(구현) 해준다.
  - 세션 비슷하게 if( principal == null) 해서 쓸 수 있네...
- MVC로 model 쓰네
- /info랑 /dashboard가 있음. info는 다 접근, dashboard는 인증 필요
- WebSecurityConfig extends WebSecurityConfigurerAdapter.. 말고 또 뭐있더라(Authorization? Resource? 이건 OAuth니깐.) 좀 외우자
- 폼 인증이니깐 formLogin()
- httpBasic()이란 것도 있네

- 아니 /logout도 있었네

## 2강. 스프링 웹 프로젝트 만들기

- 걍 해라

## 3강. 스프링 시큐리티 연동

- spring-boot-start-security
- 기본 유저 정보가 있단다 user, 콘솔 로그 password.. 몰렀네

## 4강. 스프링 시큐리티 설정하기

- 드디어 SecurityConfig
- configure(http)
- http.authorizeRequest()에서 .mvcMatchers("/", "/info").permitAll().mvcMatchers().hasRole()
- .anyRequest() : 이게 기타 등등 .authenticated() : 인증 하기만 하면 된다
- .and() .formLogin() . and() .httpBasic();
- 일케하면 느낌오는대로 딱 됨
- and() 안하고 걍 끊어도 됨.. 이게 나은거 같은데

## 5강. 스프링 시큐리티 커스터마이징 : 인메모리 유저 추가

- UserDetailsServiceAutoConfiguration.class를 보면 아주 많은게 있다.. 첨에 User 자동 생성해주는 코드 쫙 있음
- 인메모리로 유저 정보를 여러개 설정하기
  - configure(AuthenticationManagerBuilder auth)를 사용해서 원하는 유저 정보를 임의로 설정
  - 코드 함 써볼까
  
```
  auth.inMemoryAuthentication().withUser("keesun").password("{noop}123").role("USER").and()
      .withUser("admin").password("{noop}!@#");
```

- 아니 noop이 기본 패스워드 인코더.. 라는거야 아니면 암호화 안된거라는 거야. 안됐다는거 같은데
- 입력받은 값을 인코딩(noop, bcrypt로)으로 일치하면 인증이 됨


## 6강. 스프링 시큐리티 커스터마이징 : JPA 연동

- UserDetailsService: username을 받아와서 이 유저네임에 해당하는 유저정보를 데이터에서 가져와서 UserDetails라는 타입으로 리턴해줘야지 인증이 된다.(loadUserByUsername())
- @ModelAttribute로 Path에 있는 값을 받아서 모델에 넣네??? 공부 필요.. 

## 7강. 스프링 시큐리티 커스터마이징 : PasswordEncoder

- 패스워드 인코딩 방법
- NoOpPasswordEncoder.getInstance() : deprecated
- 패스워드 기본 전략이 noop 에서 스프링 시큐리티 5 이후 bcrypt로 되었다.
- 헐 바로나오네 PasswordEncoderFactories.createDelegatingPasswordEncoder() : 다양한 패스워드 인코딩을 지원하는 거란다
  - pbkdf2, scrypt, sha256 같은거 다 적용된단다... ㄷㄷㄷ
  - 그래서 됐나보네.. BCrytEncoder였나? 이거 좀 제대로 안먹히는듯??
  
## 8, 9강. 스프링 시큐리티 테스트 1, 2부

------------------

# 스프링 시큐리티 아키텍처

## 10강. SecurityContextHolder와 Authentication

- SecurityContextHolder : ThreadLocal(한 스레드 내에서 공용으로 사용하는 저장소라고 생각)을 사용, SecurityContext를 제공
  - SecurityContextHolder.getContext().getAuthentication().getPrincipal()
  - 

-------------------
# OAuth2 쿡북

- OAuth2의 인가 코드 그랜트 타입을 사용
  - 타입이 4가지 있음...
- OAuth2는 당연히 인가 서버와 리소스 서버로 구성

- @EnableAuthorizationServer
  - ClientDetailsServiceConfigurer : configure(clients), 클라이언트 정보 주입 설정
  - AuthorizationServerEndpointsConfigurer : configure(endpoint), 인가/토큰/토큰 체크 엔드포인트 설정
    - AuthorizationEndpoint
    - TokenEndpoint
    - CheckTokenEndpoint





