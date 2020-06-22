---
layout: post
title: "스프링 시큐리티 공부_3"
date: 2019-05-15
categories: "doodling"
tags: [spring]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---
# 스프링 시큐리티 적용하기 3번째 도전

> 뭘 짬뽕했는지 기억이 안난다

https://4urdev.tistory.com/53

어캐한거지?? 정리..

<script src="https://gist.github.com/ohjuntaek/1f83fd393b294ca8ac92ce1f7b9cdf06.js"></script>


1. WebSecurityConfig.java
- configure(HttpSecurity http)
    - 스태틱 같은건 permitAll() 해주고 나머지 anyRequest().authenticated()..
    - loginPage() 에서 successHandler(successHandler()) 사용!
- configure(AuthenticationManagerBuilder auth)
    - 객체 주입해 둔 CustomUserDetailsService를 이용해 로그인 인증 작업을 하는 듯...
- 그외 PasswordEncoder, AuthenticationSuccessHandler 빈 생성해서 사용

2. CustomUserDetailsService.java
- 롤은 다 ROLE_USER로 통일
- 원래 매퍼를 사용해야 한다는데 걍 DAO 때려박어
- loadUserByUsername(String username)에서 userVO가 null 이면 오류 떠서 error로 가는가?
    - userVO가 있으면 권한을 주는데(권한은 ROLE_USER로 통일, 한개만 준다) 권한 주고 new SecurityUser(userVO)로 시큐리티의 User를 생성
- save(userVO, role) : 패스워드 인코딩해서 집어넣는다.
> 좀 생각해봐야할게 이러면 패킷에서 어차피 패스워드를 다 볼 수 있는데 의미가 있나?    
> 설령 패킷에서 암호화를 했더라도 컨트롤러에서 이 서비스로 올 때 패스워드를 빼내 갈수 있지 않을까? 바로 암호화 해줘야하는거아닌가? 아닌가?

3. SecurityUser.java
- ip가 뭘까..

4. CustomLoginSuccessHandler.java 
- 여기서 세션말고 뭔가 등록해줘야하는데 뭔지를 모르겠다.

5. UserVO.java
- 뭐 봐야할건 
private Collection<? extends GrantedAuthority> authorities;    
이거 ? 뭐 GrantedAuthrity를 상속한 어떠한 클래스 라는 말인가? 그러면 왜 이거쓰냐 뭐 List 이런거 쓰면 될텐데..



# 여튼 뭔가 되긴 된다
