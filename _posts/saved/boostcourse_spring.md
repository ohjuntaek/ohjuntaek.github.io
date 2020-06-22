---
layout: post
title: "부스트코스_2_스프링 프로젝트 만들기"
date: 2019-06-04
categories: "doodling"
tags: [web, spring]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---

# Boostcourse Spring

### 1.5.2 서블릿 작성 방법
[부스트코스-서블릿 설정 방법 강좌](https://www.edwith.org/boostcourse-web/lecture/16687)

> web.xml의 <servlet-mapping>, <servlet>으로 url 컨트롤러 해줄 수 있다~ 이런게 있구나... 

### 2.9.2 Maven을 이용한 웹 어플리케이션 실습
[부스트코스-메이븐webapp강좌](https://www.edwith.org/boostcourse-web/lecture/16724/)
- 세팅 하는거 많음.. 첨하면 다봐야함 얼마안됨


### 2.11.3 Web API 실습
[부스트코스-웹API실습 강좌](https://www.edwith.org/boostcourse-web/lecture/20654/)
- pom.xml 설정 부분만 보면 됨


## Spring MVC 작성
[부스트코스 강좌-Spring MVC 구성요소](https://www.edwith.org/boostcourse-web/lecture/16763/)

* 뭐 DispatcherServlet 내부 동작 흐름에 대해 상세히 설명하는 편. __매우 중요한 부분!__.. ~~인데 백기선에서 더 자세히 할듯..잘 듣고끝내기~~
* 참고링크도 엄청 뭐 많네..

[부스트코스 강좌-Spring MVC 실습](https://www.edwith.org/boostcourse-web/lecture/16764/)

* DispatcherServlet : FrontController 역할을 하도록 설정
    - 프론트 컨트롤러는 mvc 1강에 설명 나오는데, 약간 내가 처음 서블릿 했을때 action.java 부분/싸피에서 필터로 구현한 부분인거 같다.(url 요청받아서 메소드로 넘겨주기)
    * 여튼 설정하는 방법
        1. web.xml 파일에 Spring이 제공하는 DispatcherServlet을 프론트컨트롤러로 설정할 수 있단다(1.5.2 참조)
        2. 서블릿 3.0 이상에서는(애노테이션 쓰니깐) javax.servlet.ServletContainerInitializer라는 클래스를 사용해서 할 수 있단다..
        3. 스프링이 제공하는 머시기.WebApplicationInitializer 인터페이스를 구현해서 설정할 수 있단다
    > 제일 많이 쓰는건 1,3번이란다.. 그래서 2번은 강의에 없단다. 3번은 걍 소개만 한단다.. 즉 1번
    > 걍 이클립스 legacy 프로젝트 만드는게 젤 편한데.. 만들어봐야 뭔지 알지
    
* WebMvcContextConfiguration.java
- 디스페쳐 서블릿이 해당 설정파일을 읽고 내부적으로 ApplicationContext를 생성한단다.(WebMVCContextConfig.xml에 보면 나옴..)
    - @Configuration : Bean 등록
    - @EnableWebMvc : 웹에 필요한 빈들을 대부분 자동으로 설정해준다(mvc 2강에서 했던 뭐 리퀘스트 익셉션 메세지컨버터 뭐시기..)
    - xml 설정의 <mvc:annotation-driven/>과 동일하단다
    - 기본 설정 이외의 설정이 필요하면 WebMvcConfigurerAdapter를 상속받은 자바 파일을 쓴다.. -> WebMvcConfiguratioinSupport.. 8~9분대 구현 되있는거 링크 줌
    - @ComponentScan : 빈 붙은거 찾아서 스프링 컨테이너가 관리한다
    - 와 .do가 사라진 이유, 대체한거 다 가르쳐주네 굳
> 여튼 이 실습 잘 따라하면 메이븐webapp에서 mvc로 간다.. 힘들긴 한데 딱 한번 쯤은 해보면 좋음

# 레이어드 아키텍처 실습

- 스프링 mvc 실습에서 진행
- web.xml에서 
    - WebMvcContextConfiguration.java로 DispatcherServlet 설정
    - ApplicationConfig.java로 ApplicationConfigListener
    - > 뭐 비즈니스랑 딴거를 분리해서 설정한다는데 mvc 설정이랑 컨트롤러, 빈 설정을 분리하는 건가? 
    
- 위에꺼 한거 다 종합 + spring-JDBC

    
    
    
    
### 20190714 프로젝트 3 하면서..
- 레이어드 아키텍처 실습때는 별 생각 없었는데, ApplicationConfig.java에는 dao와 service만 컴포넌트스캔하고, WebMvcContextConfiguration.java에는 (즉 DispatcherServlet의 파라미터..) contoller를 컴포넌트 스캔하는데, 
이게 컴포넌트 스캔 대상을 두개 바꾸면 안되네.. 희한하네 뭔가 먼저 실행되는거지??

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
	<display-name>Spring JavaConfig Sample</display-name>
	<!-- 비즈니스 로직 읽어들이기 -->
	<context-param>
		<param-name>contextClass</param-name>
		<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
	</context-param>
	<context-param>
		<param-name>contextConfigLocation</param-name> <!-- ContextLoaderListenr가 이 파일을 참조?해서 리스너를 등록한다 -->
		<param-value>com.boostcourse.reservation.config.ApplicationConfig</param-value>
	</context-param>

	<listener> <!-- 어떤 특정한 이벤트가 일어났을 때 동작하는 리스너 등록. 즉 여기선 context 로딩, 즉 서버가 켜질 때  이 클래스를 실행시킨다-->
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	
	
	<servlet>
		<servlet-name>mvc</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextClass</param-name>
			<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
		</init-param>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>com.boostcourse.reservation.config.WebMvcContextConfiguration</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>mvc</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
</web-app>
```

- context-param : 뭐 public 같은 개념이란다.. 그럼 이게 먼저 될려나
- init-param : 서블릿 클래스가 생성될 때 init 해줄 때 넣어주는 파라미터.. 인듯

그럼 논리적으로 context 가 먼저 실행되지 않냐?? => 테스트 해보니 그런거 같음..
- 왜냐.. dao랑 controller를 ApplicationConfig.java에 넣으면 오류는 안나는데 url 접속이 안됨 WebMvc 설정을 안해줘서 그런듯
- 그리고 dao랑 컨트롤러 서로 바꾸면 오류남 dao를 controller가 의존하는데 context에서 dao 생성 안되고 init에서 컨트롤러 생성해서 그런듯
- dao랑 컨트롤러를 init-param인 WebMvcContextConfiguration.java에 넣으면 실행 잘됨...
힘들었다.. 아오


> 아니 이거 이제보니깐 레이어드 아키텍쳐 강좌 들을 때 다 적어놨었네 context가 부모고 밑에 init-param의 Mvc콘피그 머시기가 자식이고.. 여기에 프레젠테이션 단 넣고 아놔... [여기봐라-설정의 분리](https://ohjuntaek.github.io/doodling/2019/05/30/boostcourse_javascript/#%EC%84%A4%EC%A0%95%EC%9D%98-%EB%B6%84%EB%A6%AC)








    
    
    
    



