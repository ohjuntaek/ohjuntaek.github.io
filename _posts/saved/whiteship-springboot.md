---
layout: post
title: "스프링 핵심 기술_2"
date: 2019-05-20
categories: "doodling"
tags: [spring]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---

# 백기선의 스프링 부트
- 대략 60강..

-----------------------
[스프링부트docs](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)

## 스프링 부트 시작하기

parent에 spring-boot-start-parent
디펜던시에 web starter
빌드에 플러그인에 spring-boot-maven-plugin

메인에서 SpringApplication.run해서 시작


## 스프링 부트 프로젝트 구조
- 메이븐 자바와 동일(src/main/java/패키지, src/main/resources/(클래스패스 루트) -> 스프링 리소스))
- 컴포넌트스캔은 지있는 패키지랑, 그 밑에있는거 다 해주는가? 아마도

## 자동 설정 이해, 자동 설정 만들기 1부

- @SpringBootConfiguration(사실상그냥 Configuration), @ComponentScan, @EnableAutoConfiguration
- 스프링 부트는 빈을 두번 등록하는데 컴포넌트스캔으로 등록하고 그다음 이네이블오토콘피겨레이션으로 등록된 걸 한번 더 읽어온다. ?
- spring.factories

## 자동 설정 만들기 2부
- 덮어쓰기 방지하기 : @ConditionalOnMissingBean
- application.properties 쓰기
1. 디펜던시 추가
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-configuration-processor</artifactId>
        <optional>true</optional>
    </dependency>
2. application.properties에 있는거 써먹기(적용하기)
    1. 빈에다 @ConfigurationProperties("holoman") class HolomanProperties {}
    2. @EnableConfigurationProperties(HolomanProperties.class)
    
쓰는 법(code)


```java

package me.whiteship;

public class Holoman {

    String name;

    int howLong;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getHowLong() {
        return howLong;
    }

    public void setHowLong(int howLong) {
        this.howLong = howLong;
    }

    @Override
    public String toString() {
        return "Holoman{" +
                "name='" + name + '\'' +
                ", howLong=" + howLong +
                '}';
    }
}


package me.whiteship;

import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class HolomanConfiguration {

    @Bean
    @ConditionalOnMissingBean
    public Holoman holoman() {
        Holoman holoman = new Holoman();
        holoman.setHowLong(5);
        holoman.setName("Keesun");
        return holoman;
    }
}


# spring.factories 파일이 서비스 프로바이더.. 같은 거라는데 언급만 함

org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  me.whiteship.HolomanConfiguration


# 설정 파일을 명시적으로 저장해 Configuration의 빈설정을 보고 쓰게 된다.
  
  

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>me.whiteship</groupId>
    <artifactId>keesun-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure-processor</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.0.3.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>



// --------------------------------- parents(자식)..---------------------

package com.example.parents;


import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties("holoman")
public class HolomanProperties {

    private String name;

    private int howLong;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getHowLong() {
        return howLong;
    }

    public void setHowLong(int howLong) {
        this.howLong = howLong;
    }
}


package com.example.parents;

import me.whiteship.Holoman;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

@Component
public class HolomanRunner implements ApplicationRunner {
    @Autowired
    Holoman holoman; // pom에서 의존성 설정했기 때문에 외부에서 불러들일 수 있다.

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println(holoman);
    }
}

package com.example.parents;

import me.whiteship.Holoman;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.WebApplicationType;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;

// parents 가 아니라 자식입니다

@SpringBootApplication
@EnableConfigurationProperties(HolomanProperties.class)
public class ParentsApplication {
    public static void main(String[] args) {
        SpringApplication application = new SpringApplication(ParentsApplication.class);
        application.setWebApplicationType(WebApplicationType.NONE);
        application.run(args);
    }

    // 이제 @CompenetScan하고 @EnableAutoConfiguration 하는 문제
    // 가 뭐냐하면 뒤에거에 의해 빈이 덮여씌여져서 이렇게 등록한 빈이 무용지물이 된단다.
    // 였는데 버전 업때문인가? 덮어쓰면 오류나네.. 다행히 로그에 고치는 방법 나옴
    // 그래서 외부의 라이브러리에 @ConditionalOnMissingBean 사용한다!
    // 약간 기본값 같이 사용되네..

    // 근데 생각해보면 빈이 있는데 또 빈을 등록해?
    // 그래서 application.properties로 바꾸자!
    // @CconfigurationProperties, @EnableConfiguratinProperties해서 프로퍼티로 set..
    @Bean
    public Holoman holoman(HolomanProperties holomanProperties) {
        Holoman holoman = new Holoman();
        holoman.setName(holomanProperties.getName());
        holoman.setHowLong(holomanProperties.getHowLong());
        return holoman;
    }
}


spring.main.allow-bean-definition-overriding=true

holoman.name=왜케기냔냐냐
holoman.how-long=555
# 캐멀로해도 되고 스네이크로 해도 되고.. 이게 스네이크 아니지싶은데 => 케밥 케이스! 스네이크는 _이거

```



여튼 이렇게 외부 의존성, 자동 설정을 설정할 수 있다!


## 내장 웹 서버 이해

교재 복붙
* 스프링 부트는 서버가 아니다.
    * 서버 만들기
        - 톰캣 객체 생성
        - 포트 설정
        - 톰캣에 컨텍스트 추가
        - 서블릿 만들기
        - 톰캣에 서블릿 추가
        - 컨텍스트에 서블릿 맵핑
        - 톰캣 실행 및 대기
이 모든 과정을 보다 상세히 또 유연하고 설정하고 실행해주는게 바로 스프링 부트의 자동 설정.
- ServletWebServerFactoryAutoConfiguration (서블릿 웹 서버 생성)
    - TomcatServletWebServerFactoryCustomizer (서버 커스터마이징)
- DispatcherServletAutoConfiguration : 서블릿 만들고 등록
> autoconfigure에 spring.factories 타고 들어가서 봐라..  
> 뭔진 잘 모르겠지만 궁금하면 봐라 궁금하긴하네

## 내장 웹서버 응용 1부 : 컨테이너와 포트


```
#spring.main.web-application-type=none
#server.port=0

@Component
public class PortListener implements ApplicationListener<ServletWebServerInitializedEvent> {
    @Override
    public void onApplicationEvent(ServletWebServerInitializedEvent servletWebServerInitializedEvent) {
        ServletWebServerApplicationContext applicationContext =  servletWebServerInitializedEvent.getApplicationContext();
        System.out.println(applicationContext.getWebServer().getPort());
    }
}

```



## 내장 웹 서버 응용 2부 : HTTPS와 HTTP2

### HTTPS

- 키 만들기
https://opentutorials.org/course/228/4894


keytool -genkey 
  -alias tomcat 
  -storetype PKCS12 
  -keyalg RSA 
  -keysize 2048 
  -keystore keystore.p12 
  -validity 4000
  



```
# application.properties
server.ssl.key-store: keystore.p12
server.ssl.key-store-password: 123456
server.ssl.keyStoreType: PKCS12
server.ssl.keyAlias: tomcat
```



- HTTP랑 HTTPS 같이 쓰기
https://github.com/spring-projects/spring-boot/blob/v2.0.3.RELEASE/spring-boot-samples/spring-boot-sample-tomcat-multi-connectors/src/main/java/sample/tomcat/multiconnector/SampleTomcatTwoConnectorsApplication.java



```

package com.example.demo1;

import org.apache.catalina.connector.Connector;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory;
import org.springframework.boot.web.servlet.server.ServletWebServerFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class Demo1Application {

    @GetMapping("/hello")
    public String hello() {
        return "Hello Spring";
    }

    public static void main(String[] args) {
        SpringApplication.run(Demo1Application.class, args);
    }

    @Bean
    public ServletWebServerFactory serverFactory() {
        TomcatServletWebServerFactory tomcatServletWebServerFactory = new TomcatServletWebServerFactory();
        tomcatServletWebServerFactory.addAdditionalTomcatConnectors(createStandardConnector());
        return tomcatServletWebServerFactory;
    }

    private Connector createStandardConnector() {
        Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
        connector.setPort(8080);
        return connector;
    }
}

```




- 이러면 https, 커넥터에 ssl을 적용해준다.
- https 접속해보면 빨간색이 뜨는데, 왜그런가?  
브라우저가 접속을 요청했을 때 서버가 인증서를 보내는데  
인증서는 내가 만든 keystore 안에 들어있고 그 인증서는 펍키를 모른다.  
여튼 공인된 인증서(뭔 영어..) 가 아니라 내가 그냥 만든 인증서이므로 빨간색이 뜨는데.. 무시하고 걍 고하면 됨


# 독립적으로 실행 가능한 JAR

- 로더의 JarFile, JarLauncher.. 이런걸로 JAR 파일이 동작하는구나~(MANIFEST 참조)
교재  
- spring-maven-plugin이 해주는 일 (패키징)
* 스프링 부트의 전략
    - 내장 JAR : 기본적으로 자바에는 내장 JAR를 로딩하는 표준적인 방법이 없음.
    - 애플리케이션 클래스와 라이브러리 위치 구분
    - org.springframework.boot.loader.jar.JarFile을 사용해서 내장 JAR를 읽는다.
    - org.springframework.boot.loader.Launcher를 사용해서 실행한다.



> 3부 스프링부트 원리 끝
----------------------------------------------------
> 4부 스프링부트 활용


## SpringApplication 1부
- FailureAnalyzers : 실패시 뜨는건데 그냥 이런게 있다 알아만 둬란다
- 배너 : 리소스에다 banner.txt 써도되고, SpringApplication의 setBanner 써도 되고..
- 빌더패턴 : new SpringApplicationBuilder().sources(SpringinitApplication.class).run(args);

## SpringApplication 2부

* ApplicationEvent 등록 : ApplicationListener<ApplicationStartingEvent>
- ApplicationContext를 만들기 전에 사용하는 리스너는 @Bean으로 등록할 수 없다.
- => SpringApplication.addListners(new SampleListener()); 로 리스너 등록
- 또는 ApplicationListener<ApplicationStartedEvent> 로 콘텍스트 생성 후 이벤트 실행

* WebApplicationType.NONE

* 애플리케이션 아규먼트 : 런콘피겨레이션에 프로그램 아규먼트 --bar. vm옵션 -Dfoo 는 안쳐줌
- ApplicationRunner의 메소드에 인자로 args 설정 되있음
- CommandLineRunner의 메소드에 인자로 String... args로 되있다

## 외부 설정 1, 2부
https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-external-config 사용할 수 있는 외부 설정

- application.properties => @Value("${keesun.name}") String name;


```
프로퍼티 우선 순위
1. 유저 홈 디렉토리에 있는 spring-boot-dev-tools.properties
2. 테스트에 있는 @TestPropertySource
3. @SpringBootTest 애노테이션의 properties 애트리뷰트
4. 커맨드 라인 아규먼트
5. SPRING_APPLICATION_JSON (환경 변수 또는 시스템 프로티) 에 들어있는 프로퍼티
6. ServletConfig 파라미터
7. ServletContext 파라미터
8. java:comp/env JNDI 애트리뷰트
9. System.getProperties() 자바 시스템 프로퍼티
10. OS 환경 변수
11. RandomValuePropertySource
12. JAR 밖에 있는 특정 프로파일용 application.properties
13. JAR 안에 있는 특정 프로파일용 application.properties
14. JAR 밖에 있는 application.properties
15. JAR 안에 있는 application.properties
16. @PropertySource
17. 기본 프로퍼티 (SpringApplication.setDefaultProperties)
```


뭐야이게 모르겠다~

그다음은 핵심기술에서 한 ConfigurationProperties 부트에 적용.

## 프로파일
도 마찬가지

## 로깅, 테스트

## MVC

## 데이터

## 시큐리티

## REST


--------------------- 
5부 : 스프링 부트 운영

## 액츄에이터






