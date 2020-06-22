# Spring-Security-OAuth2 정리

## 저번에 했던 Spring Security + OAuth2로 회원가입, 로그인(인증, 인가) 구현

[여기 복습](https://ohjuntaek.github.io/doodling/2019/05/15/SpringSecurityStudy3/)

### 시큐리티 구성 코드
1. Member

- 내 DB의 기본적인 유저 id 비번 정보 등등과 authorities 필드를 가짐

2. SecurityUser (extends User)

- Userdetail의 User를 상속! 안 헷갈리게 주의..
- 시큐리티는 Userdetails.User를 사용하는데, 이를 매핑해 주기 위해 별도로 만든 클래스(즉 시큐리티에서 사용하는 멤버 클래스)
- 여기에 Ip가 진짜 유저의 ip 주소네.. 이게 왜 필요하지? ip 별로 인증을 별도 처리할 게 필요할려나


3. @Service CustomUserDetailsService (implements UserDetailsService)

- @Override loadUserByUsername(username) 에서 username으로 디비의 Member를 찾아서 SecurityUser로 반환한다.
- 로그인시 유저네임과 비밀번호가 같으면 인증해주는 로직은 따로 없는데,
책 같은거 좀 볼 때 여기 집중해서 봐야할 듯..
아마 밑의 클래스에서 configure(auth)에서 하는거 같은데..
- 그외 권한 설정, 패스워드 암호화해서 저장 등 서비스 계층!
- 회원가입 하기(save), 탈퇴, 변경 여기에 넣으면 될 듯


4. WebSecurityConfig (extends WebSecurityConfigurerAdapter)

- 실질적 시큐리티 설정을 담당하는 곳, 빈 등록 및 요청에 대한 보안 수준 세부 설정..
- 1. configure(http)


```java
http.authorizeRequest() : 인증 요청에 대해
  .antMathers(url).permitAll() : url에 대해 다 허락해라
  .anyRequest().authenticated() : 나머지는 다 인증 필요하다
// 뭐 이런식으로..
```

- 2. configure(auth) : userDetailsService를 실행 시키나? 
  - auth.userDetailsService(customUserDetailsService)
			.passwordEncoder(passwordEncoder()); => 여기서 아마 패스워드 검사.. 하나??

- 3. 빈 등록, 서비스 의존
  - PasswordEncoder, AuthenticationSuccessHandler.. 등 빈등록하고 유저디테일 서비스 의존하기

5. CustomLoginSuccessHandler extends SavedRequestAwareAuthenticationSuccessHandler

- 인가가 성공하면? 인증이 성공해야 하는거 아닌가.. 어쨌든 로그인 성공하면 실행하는 핸들러
- @Override onAuthenticationSuccess() 에서 principal 이란 객체를 받아서 여기에 유저 정보가 다 담겨있다..



> 이렇게 구성해서 전에 한 OAuth2와 결합할려면 Security에 석세스핸들러에서 세션대신 http 통신을 구현해
응답으로 클라이언트에 토큰을 날려주는 코드를 구현해야 겠군...

시작

> ==> 미친소리였다. OAuth2 서버 구현 다 해놓으면 클라이언트에서 토큰을 받아와야지.. 토큰은 자동으로 날려주네 시큐리티오어쓰에서 신기
> 직접 구현 vs 라이브러리 사용.. 뭐가 나을라나








