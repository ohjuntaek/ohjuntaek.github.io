---
layout: post
title: "스프링 시큐리티 공부_1"
date: 2019-05-15
categories: "doodling"
tags: [spring]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---


# 스프링 시큐리티 공부

## 1. 인증(Authentication)과 권한(Authorization)

- 인증 : 사용자가 맞는지 아닌지 확인(하는 과정)
- 권한 : 사용자가 맞다면, 그 사용자가 어떤 기능을 이용할 수 있는지 확인(하는 과정)

## 2. 사용해보기
[Getting Started Spring Security](https://spring.io/guides/gs/securing-web/)


<script src="https://gist.github.com/ohjuntaek/4dadb1c56bd4d6818cc1ecab795767d9.js"></script>

* @EnableWebSecurity : to enable Spring Security's web security support and provide the Spring MVC integration.
  - being able to extends WebSecurityConfigurerAdapter
  

  
* configure(HttpSecurity http) method : URL이 secure 적용할건지 아닌지 정의.
    - http.authorizeRequests() : 요청에 대한 권한을 지정한다나.. 뭔소리야
        - "/" 이랑 "/home"은 .permitAll(), 즉 허락(인증) 한다는 거인거 같은데
    	- 그리고 나머지 request(anyRequest())는 인증(authenticated()) 해야한다(맞나?)
    	- and()
  	- .formLogin()
    	- .loginPage("/login") 여기에 커스텀한 로그인 페이지를 넣는갑다 물론 컨트롤러와 페이지에서 파라미터키는 어딘가에 설정해주겠지
    	- 여기서도 로그인 페이지는 permitAll, 로그아웃도 permitAll.. 즉 누구나 볼수있다 이런거인듯
    - .logout().permitAll() : 로그아웃하면 다 인증 필요없이 허락한다 이말인가??

* public UserDetailsService userDetailsService()
    ~~여기부분이 애매한데? username이 "user", password가 "password"말고 DB와 연동, 더 나아가서 jwt와 연동이 되야 할텐데... 롤은 여기서 정해져있나? 일단 넘어가자~~

* login.html 페이지 보면.. 파라미터값으로 error와 logout값이 넘어오는 갑다 이거 설정 잘 해서 하면 될 듯

* hello.html : [[${#httpServletRequest.remoteUser}]]
=> We display the username by using Spring Security’s integration with  HttpServletRequest#getRemoteUser() 
~~스프링 시큐리티 깔면 사용할 수 있는 문법이라는 건가? 좀 더 봐야할듯~~


## 3. 디비 연동해서 사용해보기
[스프링시큐리티 설명2](https://xmfpes.github.io/spring/spring-security/)

### 1. 프로젝트의 구조
걍설명
### 2. 프로젝트에 스프링시큐리티 적용하기
@EnableWebSecurity까지

### 3. 적용된 Security 기반 회원가입 로직 추가하기

뭐 Member엔터티 구성해서 컨트롤러에 BCryptPasswordEncoder 해서 signUp.html에서 보내서 save하기


* MemberController.java
```java

@Controller
@RequestMapping("/member")
public class MemberController {
	
	@Autowired
	MemberRepository memberRepository;
	
	@PostMapping("")
	public String create(Member member) {
		MemberRole role = new MemberRole();
		BCryptPasswordEncoder passwordEncoder = new BCryptPasswordEncoder();
		member.setUpw(passwordEncoder.encode(member.getUpw()));
		role.setRoleName("BASIC");
		member.setRoles(Arrays.asList(role));
		memberRepository.save(member);
		return "redirect:/";
	}
}

```


- BCryptPasswordEncoder() : 스프링 시큐리티에서 지원, 회원 비밀번호를 암호화한다
- 앞에서 만든 MemberRole 클래스를 이용하는데.. 롤이 굳이 필요한가? 걍 다 USER 아니면 ADMIN으로 떄려박어
- 이제보니깐 앞 프로젝트에 WebSecurityConfig.java의 userDetailsService() 보니깐 withDefaultPasswordEncoder()가 있네 이거 써도 될거같기도 하고..


* signUp.html
```html

<form class="signup-form" action="/member" method="POST">
  <input type="hidden" th:name="${_csrf.parameterName}" th:value="${_csrf.token}" />
  <div class="row">
    <div class="input-field col s12">
      <input id="user_name" name="uid" type="text" class="validate"/>
      <label for="user_name">Username</label>
    </div>
  </div>
  <div class="row">
    <div class="input-field col s12">
      <input id="email" name="uemail" type="email" class="validate"/>
      <label for="email">Email</label>
    </div>
  </div>
  <div class="row">
    <div class="input-field col s12">
      <input id="password" name="upw" type="password" class="validate"/>
      <label for="password">Password</label>
    </div>
  </div>
  <input class="signup-btn waves-effect waves-light btn" type="submit" value="가입하기" />
</form>


```

- 뭐 어떻게 한거야? 특히 ${_csrf.parameterName} .. ?? thymeleaf에만 있는건가 뭔가 감싸주는 느낌인데.. jsp에서 어케쓰지?
- 인풋의 클래스가 죄다 validate인데 뭔가 validator 쓴거같은 불길한 기분..모르는데...
- 뭐 잘 찾아서 해보자 jsp로 정 안되면 타임리프 쓰던가..

> 여튼 여기까지 하면 비밀번호가 잘 암호화되서 정상적으로 로그인이 된단다..


### 4. 스프링 시큐리티 로그인 기능 구현하기


- 멤버를 얻어와서, UserDetailsService를 임플리먼트 하는건가? 여튼 기본 예시(gist에 있는) 것처럼 UserDetails를 걍 memberRepo에서 받아와서 기본예제처럼 해주면 되는거 아냐??

해보자..

- 아니네... loadUserByUsername을 통해 UserDetails을 얻어와서 userDetailsService() 에서 써먹어야하네..
- 그러기 위해 나의 UserVO랑 스프링 시큐리티의 User랑 타입이 불일치한다

- 그러므로 Spring Security의 User를 상속하는 커스텀 유저 클래스를 만들어줘야한다
(롤은 USER? ADMIN? 으로 통일 일단..)



> 데모랑 짬뽕해서 해볼려고 했는데.. 데모의 userDetailsService() 의 User.withDefaultPasswordEncoder()가 deprecated 되있어서 어떻게 할지 모르겠고, loadUserByUsername도 어디서 실행되는지 모르겠고 이상한 곳이 너무 많아서 버리기로 결정

```java

안되는 코드!!

package com.ssafy.safefood.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;

import com.ssafy.safefood.model.repository.UserDAO;
import com.ssafy.safefood.model.vo.UserVO;

@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter implements UserDetailsService {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable();
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .anyRequest().authenticated()
                .and()
            .formLogin()
                .loginPage("/login").permitAll().and().logout().permitAll();
    }

    @Autowired
    UserDAO userDAO;

    User users = null;


    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        UserVO userVO = userDAO.findById(username);
        users = new User(userVO.getId(), userVO.getPass(), null);
        return users;
    }

    @Bean
    @Override
    public UserDetailsService userDetailsService() {
        UserDetails user =
             User.withDefaultPasswordEncoder()
            .username(users.getUsername())
            .password(users.getPassword())
            .roles("USER")
            .build();

        return new InMemoryUserDetailsManager(user);
    }

}


```


# 다시하기!!






















    
   





