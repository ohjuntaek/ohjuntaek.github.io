---
layout: post
title: "스프링 핵심 기술_1"
date: 2019-05-19
categories: "doodling"
tags: [spring]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---


# 스프링 프레임워크 핵심기술

- [IOC 컨테이너와 빈](#ioc--------)
  * [3강 : IoC 컨테이너 1부 : 스프링 IoC 컨테이너와 빈](#3강--ioc-컨테이너-1부--스프링-ioc-컨테이너와-빈)
  * [4강 : IoC 컨테이너 2부 : ApplicationContext와 다양한 빈 설정 방법](#4강--ioc-컨테이너-2부--applicationcontext와-다양한-빈-설정-방법)
  * [5강 : IoC 컨테이너 3부 @Autowired](#5강--ioc-컨테이너-3부-autowired)
  * [6강 : IoC 컨테이너 4부 : @Component와 컴포넌트스캔](#6강--ioc-컨테이너-4부--component와-컴포넌트스캔)
  * [7강 : IoC 컨테이너 5부 : 빈의 스코프](#7강--ioc-컨테이너-5부--빈의-스코프)
  * [8강 : IoC 컨테이너 6부 : Environment 1부. 프로파일](#8강--ioc-컨테이너-6부--environment-1부-프로파일)
  * [9강 : IoC 컨테이너 6부 : Environment 2부. 프로퍼티](#9강--ioc-컨테이너-6부--environment-2부-프로퍼티)
  * [10강 : IoC 컨테이너 7부 : MessageSource](#10강--ioc-컨테이너-7부--messagesource)
  * [11강 : IoC 컨테이너 8부 : ApplicationEventPublisher](#11강--ioc-컨테이너-8부--applicationeventpublisher)
  * [12강 : IoC 컨테이너 9부 : ResourceLoader](#12강--ioc-컨테이너-9부--resourceloader)
  * [13강 : Resource 추상화](#13강--resource-추상화)
  * [14강 : Validation 추상화](#14강--validation-추상화)
  * [15강 : 데이터 바인딩 추상화 : PropertyEditor](#15강--데이터-바인딩-추상화--propertyeditor)
  * [16강 : 데이터 바인딩 추상화 : Converter와 Formatter](#16강--데이터-바인딩-추상화--converter와-formatter)
    + [Converter](#converter)
    + [Formatter](#formatter)
    + [ConversionService](#conversionservice)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


--------------
# IOC 컨테이너와 빈

## 3강 : IoC 컨테이너 1부 : 스프링 IoC 컨테이너와 빈

- 빈 : 스프링 IoC 컨테이너가 관리하는 객체
- 스프링 IoC 컨테이너 : 빈 설정 소스로 부터 빈 정의를 읽어들이고, 빈을 구성하고 제공한다
~~ApplicationConfig 파일 말하는 건가?~~

- 왜 빈으로, IoC 컨테이너로 관리하게 하는가(장점)? : 1. DI 2. 빈의 스코프(싱글톤/프로토타입/리퀘스트/세션) 3. 라이프사이클 인터페이스 지원(@PostConstruct..~~이거AOP 아닌가?~~)

- 의존성을 가진 클래스에 대한 단위테스트가 힘들다
(13분쯤 BookServiceTest)
북리파지토리를 구현하지 않고는 북서비스가 코드가 있음에도 테스트를 할 수가 없다
=> @Mock 을 통해 가짜객체를 만들어서 북레파지토리 의존성 주입가능
when(bookRepository.save(book)).thenReturn(book) // save라는 메소드가 호출될떄 북이 들어오면 북을 리턴하라(동일한 인스턴스를 리턴한다면 동작)


- 스프링 IoC의 중요한 인터페이스 2가지 BeanFactory -> ApplicationContext(추가적으로 ApplicationEventPublisher 등등..)
[BeanFactory Docs](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html)    
뭐가이래 많어 어쩄든 중요한게 많다

~~아오 너무 빠르네 어렵고 스탑 자주해서 시간 적고 정리해야할듯~~


## 4강 : IoC 컨테이너 2부 : ApplicationContext와 다양한 빈 설정 방법

- web-start : aop, bean, web, webmvc
- @Autowired와 @Inject차이.. 는 뒤에 나온다 ~~현재 19강쨰 언제나와?~~ 

```java

package com.example.whiteship.book;

import org.springframework.stereotype.Service;

@Service
public class BookService {

    // 참고로 여기서 Autowired하고 ApplicationConfig에 걍 빈등록(뭐 세터에 레포 안넣어줘도)해도 메인에서 당연히 먹힌다
    // 근데 여기서 세터로 의존성 주입하는걸로 했으니깐 레포가 세터로 들어와서 먹히는게 당연히 먹히는데
    // 만약에 생성자를 사용하는 경우에는 앱콘피그에 빈등록을 @Bean 어쩌고 new BookService(); 해야하는데. 당연히 안먹히겠지? 기본생성자가 없어서 아예 컴파일 오류 뜬다는 말인거 같은데
    // 여튼 그래서 세터를 많이 쓴다고 들었는데 이게 하나의 이유인듯
    BookRepository bookRepository;

    public void setBookRepository(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }
}


@Configuration
@ComponentScan(basePackageClasses = {BookService.class, WhiteshipApplication.class}) // 이거 뭐 알지? 이게 스프링부트에서 해주는거랑 가장비슷하단다
public class ApplicationConfig {

    @Bean
    public BookRepository bookRepository() {
        return new BookRepository();
    }
    @Bean
    public BookService bookService() {
        BookService bookService = new BookService();
        bookService.setBookRepository(bookRepository()); // 북Repo 세터를 사용한 의존성 주입
        return bookService;
    }
}

import java.util.Arrays;

@SpringBootApplication
public class WhiteshipApplication {
    public static void main(String[] args) {
//        ApplicationContext context1 = new GenericXmlApplicationContext() : 수업시간엔 이거 썼었네
//        ApplicationContext context = new ClassPathXmlApplicationContext("application.xml");
        ApplicationContext context = new AnnotationConfigApplicationContext(ApplicationConfig.class); // 딱보면 애노테이션 방법으로 콘피그 등록하는거

        String[] beanDefinitionNames = context.getBeanDefinitionNames();
        System.out.println(Arrays.toString(beanDefinitionNames));
        BookService bookService = (BookService) context.getBean("bookService");
        System.out.println(bookService.bookRepository != null); // bookRepository가 property를 통해 주입되었다
        // 객체 주입방식이 property랑 뭐 생성자 주입하고 또 뭐있나? 정리를 해야함
    }
}



<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--<bean id="bookService" class="com.example.whiteship.book.BookService">
        <property name="bookRepository" ref="bookRepository" />
    </bean>

    <bean id="bookRepository" class="com.example.whiteship.book.BookRepository"/>

    여튼 이렇게 하면 하나하나 다 설정해줘야하는데 굉장히 번거롭다..
    -->

    <!-- 그래서 component-scan 쓴다 : 스프링 2.5부터 가능해진 애노테이션 기반의 빈 설정 방법-->
    <context:component-scan base-package="com.example.whiteship.book"/>
</beans>



```
- 주입 방식 : xml 빈등록 -> xml 컴포넌트 스캔 -> java Bean등록 -> java Component 스캔
- ※주의! : 생성자 방식으로 빈 의존성 주입하면 Autowired가 안되나?? 여튼 AppConfig 직접 만들면 아예 빈등록컴파일 에러뜰거고...  
안만들고 걍 스프링부트에 맡기면.. 어캐될려나 모르겠다 아마 안될듯?(된다)


## 5강 : IoC 컨테이너 3부 @Autowired

- 빈이 없으면 당연히 빈 없습니다 오류 뜸
- @Repository랑 @Component 분리하는 이유 : AOP 할때 좋단다

- 같은 타입의 빈이 여러개 일 때 : @Primary, 모든 빈 다 받기, @Qualifier (+필드명을 클래스에 id랑 같게 하면 되긴하는데 추천안함) ~~코드엔 안적을란다 강의를 보던지(대략 10분쯤) 검색하던지 하면 알수있을듯~~

- 동작 원리
    - BeanPostProcessor란 인터페이스에 의해 동작한단다 : ~~빈의 초기화라이프사이클(빈의 이니셜라이제이션이라는 라이프사이클이 있단다) 이후에 사용되는 인터페이스인가? 잘모르겠다~~
    [BeanPostProcessor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/config/BeanPostProcessor.html)
    - 뭐 여기서 AutowiredAnnotationBeanPostProcessor가 작동을 해서 @Autowired가 된단다나..
    - 그래서 @PostConstruct에서 Autowired 한거 사용할 수 있다나

  
- @Autowired(required = false)에 대해 알자
- @Autowired 가 없으면? 있으면?
> 없으면 아예 주입 시도를 안함  
> 있으면 주입시도하는데, require=false이면 주입 실패해도 걍 넘어가고 딴거한다(주입 시도한 객체 빈 등록 이라던지..)
### 주의점
> 1. 위에서 기본 생성자를 막으면 오류가 난다고 했는데, AppConfig, 즉 빈 설정 직접 안하고 그냥 @Autowried하면 생성자 뭘 하던 알아서 잘 된다  
> 2. @Autowired 생성자 vs 필드/세터
>   - 필드를 넣은 생성자를 선언하면 빈 등록할 때 선언한 생성자를 사용하므로 주입이 된다
>   - 세터나 그냥 필드 선언은 @Autowired 안하면 사용을 안하니깐 해줘야지 널포인터 안나고 사용할 수 있다.
>> 내 이해를 돕기위해 만든 코드(직접 빈 등록 + 자동 빈 등록)

```
/*
package com.example.demo;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
// Runner -> Bean -> BeanField
@Configuration
public class AppConfig {
    @Bean
    public AutowiredTestBean autowiredTestBean(AutowiredTestBeanField autowiredTestBeanField) {
        AutowiredTestBean autowiredTestBean = new AutowiredTestBean();
        autowiredTestBean.setAutowiredTestBeanField(autowiredTestBeanField);
        return autowiredTestBean;
    }

    @Bean
    public AutowiredTestRunner autowiredTestRunner(AutowiredTestBean autowiredTestBean) {
        AutowiredTestRunner autowiredTestRunner = new AutowiredTestRunner(autowiredTestBean);
//        autowiredTestRunner.setAutowiredTestBean(autowiredTestBean);
        return autowiredTestRunner;
    }

    @Bean
    public AutowiredTestBeanField autowiredTestBeanField() {
        return new AutowiredTestBeanField();
    }
}
*/

package com.example.demo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class AutowiredTestBean {
    @Autowired
    AutowiredTestBeanField autowiredTestBeanField;

    /*public void setAutowiredTestBeanField(AutowiredTestBeanField autowiredTestBeanField) {
        this.autowiredTestBeanField = autowiredTestBeanField;
    }*/
}

package com.example.demo;

import org.springframework.stereotype.Component;

@Component
public class AutowiredTestBeanField {
    int a = 3;
    String b = "s";
}


package com.example.demo;

import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class AutowiredTestRunner implements CommandLineRunner {
    AutowiredTestBean autowiredTestBean;

    public void setAutowiredTestBean(AutowiredTestBean autowiredTestBean) {
        this.autowiredTestBean = autowiredTestBean;
    }

    /*public AutowiredTestRunner(AutowiredTestBean autowiredTestBean) {
        this.autowiredTestBean = autowiredTestBean;
    }*/

    @Override
    public void run(String... args) throws Exception {
        System.out.println(autowiredTestBean.autowiredTestBeanField.a);
    }
}

package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);


        /*ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);

        AutowiredTestRunner autowiredTestRunner = (AutowiredTestRunner) ctx.getBean("autowiredTestRunner");
        try {
            autowiredTestRunner.run(args);
        } catch (Exception e) {
            e.printStackTrace();
        }*/
    }

}

```


>> 만약 실제로 @Configuration 해서 빈등록 한다면.. 위에 코드가 완성되는데  
>> 여튼 이 코드에서 보면 Runner -> Bean -> BeanField에 의존하잖니?  
>> IoC 보면(AppConfig.java) 생성자는 빈 등록할려면 무조건 사용하는거니깐 굳이 Autowired 안해도 되고, 세터나 필드는 따로 뭐 코드 안적어주면 주입안하는거니깐 위에 @Autowired 를 적어줘야한다! 이거인듯? 이렇게 알자...


## 6강 : IoC 컨테이너 4부 : @Component와 컴포넌트스캔

- ComponentScan에 basePackageClasses 가 중요하단다 당연.
- AutoConfigurationExcludeFilter, TypeExcludeFilter라는게 있단다. 이게 뭐 거르는 거라는데 걍 있다는거 알면 된단다
- 뭐 구동시간 줄일려면 펑션을 사용한 빈 등록이 있다는데 리플렉션/프록시를 사용안한단다 
- ~~아니 자바 10부터 var가 된다고? ㄷㄷ~~
- 뭐 평셔널 ctx.registerBean 같은것도 쓰네 희한하네


## 7강 : IoC 컨테이너 5부 : 빈의 스코프

- @Scope("prototype")

- 프로토타입 빈이 싱글톤 빈을 참조하면? : 당연 아무 문제 없다
- 싱글톤 빈이 프로토타입 빈을 참조하면? : 당연히 프로토타입 빈이 업데이트 안된다.
    - 해결방법 1 : proxyMode = ScopedProxyMode.TARGET_CLASS 쓰면 된단다.. 이해는 상당히 어려운데.. 
        - 이해 : 프록시를 거쳐서 참조하게 한단다. 왜냐하면 직접쓰면 바꿔줄 여지가 없는데 바꿔줄수 있게 Proxy로 감싸는 것
        - 클래스도 프록시를 만들수 있게 하는 뭐 CG라이브러리? 인터페이스로만 만들수 있는데... 먼소리야
        - 뭐 어쩄든 저거하면 프록시 빈을 주입해주는 거란다 (프록시패턴 => AOP에서 자세히 설명)
    - 2 : ObjectProvider<Proto> proto, return proto.getIfAvailable() 이렇게 쓰면 되는데 추천 안한단다 왜냐하면 포조를 좀 바꿔주니깐.. 등
- 싱글톤시 위험한거 : 스레드 안세이프(그 뭐였지 syncronized 뭐 있었는데.. 여튼 싱글톤은 특히 스레드 세이프 하게 짜야한다!)


## 8강 : IoC 컨테이너 6부 : Environment 1부. 프로파일

- ApplicationContext가 빈팩토리 기능만하는건 아니다. 보면 
```
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory,
		MessageSource, ApplicationEventPublisher, ResourcePatternResolver
```
엄청 많이 상속받는데 EnvironmentCapable, 여기서 프로파일 기능? 에대해서 이 강의에서 살펴본다 

- 프로파일 : 빈들의 그룹
- 메이븐도 프로파일 있는데, 어떤 환경이란다. 개발용땐 이런 프로파일을 쓰고, 실제론 이런 프로파일을 쓰고...
- 이런 기능은 Environment라는 인터페이스를 통해 사용할 수 있딴다
- @Configuration에 @Profile("test")하고
- vm 옵션에 Dspring.profiles.active="test"
- 근데 Configuration 으로 ApplicationContext 만들면 불편하니깐 걍 빈위에다 @Profile 떄려박아도 된다.
- @Profile("!test1 & test2 | test3") 이런것도 된단다. 딱봐도 not

> 

## 9강 : IoC 컨테이너 6부 : Environment 2부. 프로퍼티

- vmoption : -Dapp.name=spring5 이렇게 줄수 있단다. 이걸 어떻게 접근하냐?
- app.properties로 줄수도 프로퍼티 줄수도 있는건 당연

```
        @Value(${app.name})
        String appName;

        Environment environment = ctx.getEnvironment();
        System.out.println(environment.getProperty("app.name"));
        System.out.println(appName);
```


## 10강 : IoC 컨테이너 7부 : MessageSource

- 국제화(i18n) 기능을 제공하는 인터페이스
- ApplicationContext가 MessageSource를 상속받는다.
- messages.ko_KR.properties 처럼 앞에 messages가 있으면 자동으로 MessageSource 가 인식해서 사용할 수 있음

```
	System.out.println(messageSource.getMessage("greeting", new String[]{"keesun"}, Locale.KOREA));
	System.out.println(messageSource.getMessage("greeting", new String[]{"keesun"}, Locale.getDefault()));
```

등..

## 11강 : IoC 컨테이너 8부 : ApplicationEventPublisher


- 옵저버 패턴의 구현체 ~~뭔지 모르는데?~~
- 이벤트 기반의 프로그래밍을 할때 유용
- 4.2 이후로 이벤트 객체는 ApplicationEvent를 상속시키지 않아도, 핸들러에 implements ApplicationListener<MyEvent> 안하고 @EventListener 써도 그냥 들어간단다.
	- 그래서 이게 스프링이 추구하는 비침투성? 머시기 머시기


오랜만에 코드 볼까
```
package com.example.whiteship;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationEventPublisher;
import org.springframework.stereotype.Component;

@Component
public class AppRunner implements ApplicationRunner {
    @Autowired
    ApplicationEventPublisher publisherEvent;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        publisherEvent.publishEvent(new MyEvent(this, 100));
    }
}


package com.example.whiteship;

import org.springframework.context.ApplicationEvent;

public class MyEvent {

    private int data;

    private Object source;

    public MyEvent(Object source) {
        this.source = source;
    }
    public MyEvent(Object source, int data) {
        this.source = source;
        this.data = data;
    }

    public int getData() {
        return data;
    }
}
package com.example.whiteship;

import org.springframework.context.ApplicationListener;
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;

@Component
public class MyEventHandler {

    @EventListener
    public void onApplicationEvent(MyEvent event) {
        System.out.println("이벤트 받았다. 데이터는 "+event.getData());
    }
}

```

- 이벤트 핸들러를 @Order, @Async라는걸(이거는 @EnableAsync 해줘야함) 써서 순서, 비동기로 사용하게 할 수 있단다
- ContextRefreshedEvent, ContextStartedEvent, ContextCloseEvent를 붙여서 라이프사이클 제어 할수 있는거 같네.. 이런게 있구나


## 12강 : IoC 컨테이너 9부 : ResourceLoader

- 리소스를 읽어오는 기능을 제공하는 인터페이스

```
package com.example.whiteship;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;
import org.springframework.stereotype.Component;

@Component
public class AppRunner implements ApplicationRunner {
    @Autowired
    ResourceLoader resourceLoader;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Resource resource = resourceLoader.getResource("classpath:text.txt");
        System.out.println(resource.exists());
        System.out.println(resource.getDescription());

    }
}

```

> 지금까지 ApplicationContext, IoC를 살펴보았고, 여기에 위의 ResourceLoader, Event, MessageSource, Environment.. 등의 기능이 있습니다~

------------------------------

## 13강 : Resource 추상화

- java.net.URL을 추상화 한 것
- 추상화시킨 이유 : 클래스패스로 리소스를 가져온게 없고.. 등등
- 리소스 구현체
	- UrlResource
	- ClassPathResource : 접두어 classpath: 를 지원
	- FileSystemResource
	- ServletContextResource
	
- 접두어 강제
	- classpath://
	- file://
	- 대부분 WebApplicationContext(ServletContextResource로 구현)로 ctx를 구현하지만, 이를 대부분 모르기 때문에 명시적으로 접두어 강제하는것이 좋다.

- 기본적으로 서블릿 콘텍스트 리소스가 되는데, 컨텍스트 패스부터 찾고, 그러나 스프링부트의 내장형톰캣은 컨텍스트패스가 지정되어있지 않으므로 리소스 폴더안에 있는 리소스를 찾을 수 없다.. (강의 대략 10분 ~ 13분쯤 잘봐라)

## 14강 : Validation 추상화

- 애플리케이션에서 사용하는 객체 검증용 인터페이스
- BeanValidation.org : 자바 표준스택?

```

public class EventValidator implements Validator {

    @Override
    public boolean supports(Class<?> clazz) {
        return Event.class.equals(clazz); // 파라미터로 전달되는 객체값이 Event인지 확인
    }

    @Override
    public void validate(Object target, Errors errors) {
        ValidationUtils.rejectIfEmptyOrWhitespace(errors, "title", "notempty", "Empty title is not allowed");
        // 이게 뭐냐 딱보면 title이 비었으면 notempty뽑아라...
        /*Event event = (Event)target;
        if (event.getTitle() == null) {
            errors.reject("notempty", "Empty title is not allowed");
        }*/
    }
}

package com.example.whiteship;

import org.springframework.context.ApplicationEvent;

import javax.validation.constraints.*;

public class Event {
    Integer id;

    @NotEmpty
    String title;

    @NotNull @Min(0)
    Integer limit;

    @Email
    String email;

    public Integer getLimit() {
        return limit;
    }

    public void setLimit(Integer limit) {
        this.limit = limit;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Integer getId() {
        return id;
    }

    public String getTitle() {
        return title;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public void setTitle(String title) {
        this.title = title;
    }
}

package com.example.whiteship;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.core.io.Resource;
import org.springframework.core.io.ResourceLoader;
import org.springframework.stereotype.Component;
import org.springframework.validation.BeanPropertyBindingResult;
import org.springframework.validation.Errors;
import org.springframework.validation.Validator;

import java.util.Arrays;

@Component
public class AppRunner implements ApplicationRunner {

    @Autowired
    Validator validator;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println(validator.getClass());
        Event event = new Event();
        event.setLimit(-1);
        event.setEmail("aaa2");
//        EventValidator eventValidator = new EventValidator();
        Errors errors = new BeanPropertyBindingResult(event, "event"); // 이게 뭘까? 어렵네

//        eventValidator.validate(event, errors);
        validator.validate(event, errors);

        System.out.println(errors.hasErrors());
        errors.getAllErrors().forEach(e -> {
            System.out.println("===== error code =====");
            Arrays.stream(e.getCodes()).forEach(System.out::println);
            System.out.println(e.getDefaultMessage());
        });
    }
}



```

- Validator 인터페이스를 구현하여 supports로 타입확인? 해주고 validate에서 하고싶은거 해주고
- 굳이 안하고 그냥 사용할려면 자바빈즈 객체같은거에 @NotNull, @NotEmpty, @Email 같은거 해주고 Validator를 @Autowired로 주입시키면 쓸수 있다



## 15강 : 데이터 바인딩 추상화 : PropertyEditor

- 데이터 바인딩 : 프로퍼티값을 타겟 객체에 설정하는 기능
- 즉, 사용자 입력값을(스트링) 애플리케이션 도메인 모델에 동적으로 변환해 넣어주는 기능(뭐 int나 등등..)
- 고전적인 방식 : PropertyEditor(스프링 3.0 이전..)


```

package com.example.whiteship;

public class Event {
    private Integer id;

    private String title;

    public Event(Integer id) {
        this.id = id;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    @Override
    public String toString() {
        return "Event{" +
                "id=" + id +
                ", title='" + title + '\'' +
                '}';
    }
}

@RunWith(SpringRunner.class)
@WebMvcTest
public class EventControllerTest {
    @Autowired
    MockMvc mockMvc;

    @Test
    public void getTest() throws Exception {
        mockMvc.perform(get("/event/1"))
                .andExpect(status().isOk())
                .andExpect(content().string("1"));

    }
}

@RestController
public class EventController {

    @InitBinder
    public void init(WebDataBinder webDataBinder) {
        webDataBinder.registerCustomEditor(Event.class, new EventEditor());
    }

    @GetMapping("/event/{event}")
    public String getEvent(@PathVariable Event event) {
        System.out.println(event);
        return event.getId().toString();
    }
}

public class EventEditor extends PropertyEditorSupport {
    @Override
    public String getAsText() {
        Event event = (Event)getValue();
        return event.getId().toString();
    }

    @Override
    public void setAsText(String text) throws IllegalArgumentException {
        setValue(new Event(Integer.parseInt(text)));
    }

}


```

- 뭐 이런식으로 PropertyEditorSupport를 상속받아서 컨트롤러에서 @initBinder해줘서 에디터 등록해주면 이렇게 된다
- 여기서 아주아주 주의사항!
	- 절대 PropertyEditorSupport를 상속받은 클래스(EventEditor)를 빈으로 등록해서 싱글톤으로 관리해주면 완전히 망한다 왜냐 setValue(), getValue()에서 사용하는 음 자원? 이 statefull, 즉 스레드세이프하지 않기 떄문이다.. 그래서 절대 빈으로 등록하지 말고 @InitBinder 사용해서 써란다
	- 참고로 PropertyEditor는 문자열과 오브젝트와의 관계..
	
- 근데 이건 다 스프링 3.0 이하에서 사용하던 거라.. 지금도 쓸란가? 안쓸거 같은데 너무 위험하기도하고




## 16강 : 데이터 바인딩 추상화 : Converter와 Formatter

- 아까 사용한 PropertyEditor 대신 Converter를 사용.. 이건 스레드세이프하단다


### Converter
```
public class EventConverter {
    public static class StringToEventConverter implements Converter<String, Event> {
        @Override
        public Event convert(String s) {
            return new Event(Integer.parseInt(s));
        }
    }
    public static class EventToStringConverter implements Converter<Event, String> {
        @Override
        public String convert(Event source) {
            return source.getId().toString();
        }
    }
}

@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addConverter(new EventConverter.StringToEventConverter());
    }
}

@RestController
public class EventController {

    @GetMapping("/event/{event}")
    public String getEvent(@PathVariable Event event) {
        System.out.println(event);
        return event.getId().toString();
    }
}

```

- 이러면 잘 변환 되는데,
- 사실 Event 같은 거는 걍 Integer 로 받으면 알아서 컨버팅 해주기때문에 굳이 안해도 된단다. 안되있는 것만 해줘라


### Formatter

- 로케일을 받아서 국가별로 다르게 메세지를 날려줄 수 있단다.. 찾아보면 잘 나올듯 컨버터랑 비슷하니 생략

### ConversionService
- 그러니까 위의 두개가 이 인터페이스를 통해서 스레드세이프하게 사용할 수 있단다
- DefaultFormattingConversionService -> ConversionService/FormatterRegistrty(->ConverterRegistry)


- 뭐 ConversionService 투스트링하면 변환할수있는 목록 보여줬었나? 여튼 대부분 변환해준다 스프링부트에서.. 잘보고 사용하라



## 17강 : SpEL

- 객체 그래프(?)를 조회
- jstl 비슷하단다, 메소드 호출 지원, 문자열 템플릿 기능 제공
- 스프링 시큐리티, 데이터, 타임리프 등 엄청 많이 쓴다

- 표현식 : #{}
- 프로퍼티 : ${}
- 표현식 안에는 프로퍼티 사용 가능!

```


@Component
public class AppRunner implements ApplicationRunner {
    @Value("#{1 + 1}")
    int value;

    @Value("#{'hello' + 'world'}")
    String greeting;

    @Value("#{1 eq 1}")
    boolean trueOrFalse;

    @Value("${my.value}")
    int myValue;

    @Value("#{${my.value} eq 100}")
    boolean isMyValue100;

    @Value("#{sample.data}")
    int sampleData;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("=======");
        System.out.println(value);
        System.out.println(greeting);
        System.out.println(trueOrFalse);
        System.out.println(myValue);
        System.out.println(isMyValue100);
        System.out.println(sampleData);

        ExpressionParser parser = new SpelExpressionParser();
        Expression expression = parser.parseExpression("2 + 100"); // 자체가 Expression(#{}) 이다
        Integer value = expression.getValue(Integer.class); // 이때 ConversionService를 사용한단다.
        System.out.println(value);

    }
}


```


[SpEL DOCS](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html)

* 사용하는 곳
- @Value 애노테이션
- @ConditionalOnExpression : 선택적으로 빈을 등록하거나 빈 설정파일을 읽을 때 사용
- Spring Security
- Spring DATA의 @Query

> [MyBatis에서 #{}과 ${}를 사용하는데...](https://logical-code.tistory.com/25)    
> 근데 이건 SpEL이랑 상관없어 보이는데 상관 있나?? 뭔가 MyBatis에서 #쓰면 어떻게 SQL인젝션 처리하고 파라미터로 바꿔주게 해놨고 #은 표현식이니깐 뭔가 바꾸는게 되고(ConversionService 같은거 좀 바꿔서??) $는 걍 출력하니깐 안된다 이런건가..??  


-----------------------

## 18강 : 스프링 AOP : 개념 소개

- AspectJ/스프링AOP 연동, 트랜잭션, 캐시
- OOP를 보완하는 수단. 흩어진 Aspect를 모듈화 할 수 있는 프로그래밍 기법
- 로깅/트랜잭션..
- Aspect/Advice/JoinPoint/Pointcut
- AOP 적용방법
    - 컴파일 - 걍 코드 넣어서 컴파일
    - 로드 타임 - 로드타임 위빙?
    - 런타임 - 프록시
    
    
## 19강 : 스프링 aop : 프록시 기반 AOP
- 프록시 기반 AOP
- 스프링 빈에 AOP를 적용할 수 있다
- 프록시 : 접근제어/부가기능 추가
- 토비의 스프링3에 아주 자세히 나와있단다
- BeanPostProcessor를 구현한 AbstractAutoProxyCreator로 동적 프록시를 구현할 수 있다??

> 프록시 패턴 등에 대해 설명한 강의.. ~~코드는 labssafy거 보기~~


## 20강 : 스프링 aop : @AOP

- 애노테이션 기반 AOP!
- 서론 끝났고 이제 본론

```
package com.example.whiteship;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

@Component
public class AppRunner implements ApplicationRunner {
    @Autowired
    EventService eventService; // 인터페이스 타입으로 주입 받는게 항상 좋다 (왜??)


    @Override
    public void run(ApplicationArguments args) throws Exception {
        eventService.createEvent();
        eventService.publishEvent();
        eventService.deleteEvent();
    }
}


package com.example.whiteship;

public interface EventService {
    void createEvent();

    void publishEvent();

    void deleteEvent();
}

package com.example.whiteship;


import org.springframework.stereotype.Service;

@Service
public class SimpleEventService implements EventService {
    @PerLogging
    @Override
    public void createEvent() {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Created an event");
    }
    @PerLogging
    @Override
    public void publishEvent() {
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Published an event");
    }

    public void deleteEvent() {
        System.out.println("Delete an event");

    }

}

package com.example.whiteship;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class PerfAspect {

//    @Around("execution(* com.example..*.EventService.*(..))")
    @Around("@annotation(PerLogging)")
    public Object logPerf(ProceedingJoinPoint pjp) throws Throwable {
        long begin = System.currentTimeMillis();
        // Advice가 적용되는 대상
        Object retVal = pjp.proceed();
        System.out.println(System.currentTimeMillis()-begin);
        return retVal;
    }

    @Before("bean(simpleEventService)")
    public void hello() {
        System.out.println("hello");
    }
}

package com.example.whiteship;

import java.lang.annotation.*;

/**
 * 이 애노테이션을 사용하면 성능을 로깅해 줍니다
 */
@Retention(RetentionPolicy.CLASS)
@Target(ElementType.METHOD)
@Documented
public @interface PerLogging {
}

package com.example.whiteship;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class WhiteshipApplication {
    public static void main(String[] args) {
        SpringApplication.run(WhiteshipApplication.class, args);
    }
}



```

요렇게 씁니다. PerfAspect.java 주의깊게 보기! 


## 21강:Null-safety

@NonNull
@Nullable
@NonNullApi
@NonNullFields

- 컴파일 오류나게 해준단다 툴의 도움을 받아서






