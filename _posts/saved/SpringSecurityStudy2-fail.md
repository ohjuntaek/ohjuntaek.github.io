---
layout: post
title: "스프링 시큐리티 공부_2"
date: 2019-05-15
categories: "doodling"
tags: [spring]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---

# 스프링 시큐리티 공부 두 번째

~~이래갖고 jwt, oauth 언제하나...~~

http://progtrend.blogspot.com/2018/07/spring-boot-security.html

## WebSecurityConfig.java 설정

```java

@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
 
    @Autowired
    private UserAuthenticationProvider authenticationProvider;
 
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/css/**", "/js/**", "/img/**").permitAll()
                .antMatchers("/auth/admin/**").hasRole("ROLE_ADMIN")
                .antMatchers("/auth/**").hasAnyRole("ROLE_ADMIN", "ROLE_USER")
                .anyRequest().authenticated();
 
        http.formLogin()
                .loginPage("/login") // default
                .loginProcessingUrl("/authenticate")
                .failureUrl("/login?error") // default
                .defaultSuccessUrl("/home")
                .usernameParameter("email")
                .passwordParameter("password")
                .permitAll();
 
        http.logout()
                .logoutUrl("/logout") // default
                .logoutSuccessUrl("/login")
                .permitAll();
    }
 
    @Override
    protected void configure(AuthenticationManagerBuilder auth) {
        auth.authenticationProvider(authenticationProvider);
    }
}

```


- 링크에 잘 설명 되있다..
- UserAuthenticationProvider : 커스텀 인증을 구현하는 클래스란다 뒤에 다시 설명
- authorizeRequests(), formLogin(), logout() : 이거는 아까거랑 짬뽕해서 쓰기
- configure(AuthenticationManagerBuilder auth) : 32 ~ 34 : 커스텀 인증을 구현한 (6 라인에서 Autowired로 주입된) authenticationProvider를 AuthenticationManagerBuilder에 AuthenticationProvider로서 추가한다.


## UserAutenticationProvider.class 
### 커스텀 인증을 구현하는 클래스


```java

@Component
public class UserAuthenticationProvider implements AuthenticationProvider {
 
    @Autowired
    UserService userService;
 
    @Override
    public Authentication authenticate(Authentication authentication) 
            throws AuthenticationException {
        String email = authentication.getName();
        String password = (String) authentication.getCredentials();
 
        UserVO userVO = userService.authenticate(email, password);
        if (userVO == null)
            throw new BadCredentialsException("Login Error !!");
        userVO.setPassword(null);
 
        ArrayList<SimpleGrantedAuthority> authorities = new ArrayList<>();
        authorities.add(new SimpleGrantedAuthority("ROLE_USER"));
        return new UsernamePasswordAuthenticationToken(userVO, null, authorities);
    }
 
    @Override
    public boolean supports(Class authentication) {
        return authentication.equals(UsernamePasswordAuthenticationToken.class);
    }
}

```

- userService 주입하고, athenticate를 호출하는데, 왜 getName을 먼저하지??? 이거에서 입력된 email(id)랑 패스워드 가져오는 거인갑네 
- 그래서 가져온 email과 password에서 userService.authenticate? 아오 난 이런거 한적 없는데... 이거 로직 직접 구현할거면 세션해서 암호화 하는거랑 뭐가달러? => 뭔가 다르단다 일단 암호화 뭐 리소스 관리 등등
- 여튼 authenticate 구현해서 해보자..
- csrf 사용 안하게 하는거 주의하라


# 아니 이건왜 UserDetails가 없냐 로그인하는데...;;; 걍 다시





