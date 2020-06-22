---
layout: post
title: "부스트코스_1"
date: 2019-05-30
categories: "doodling"
tags: [web, javascript]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---

# 부스트코스

예약 관리 시스템 만들기
- Spring, JDBC, JSP/SERVELT 이용


# 자바스크립트

### 기본 문법

- ECMAScript(ES)의 버전에 따라 자바스크립트 버전이 결정되고, 이를 자바스크립트 실행 엔진이 반영한다
- ES6가 2018년부터 표준으로 쓰이고 있음

#### 자료형

* 변수 선언
    - var, let, const로 선언 (scope가 다르다. 그럼 자료형은 => 자동으로 정해줌(""면 string, )
        - const : 상수, 한번 할당한 값에 재 할당이 되지 않는다.
* 연산자
    - const myname = name || "defaultname"; // name 이 있으면 name을 쓰고 아니면 오른쪽걸 쓴다  
    즉 왼쪽이 만족되면 오른쪽 거를 아예 안본다 자바랑 똑같은거 같은데??  
    > 생각해보니까 자바는 or, and 일때 이렇게 동작했던거 같다.. 맞나?
        - 근데 자바는 || 하면 논리 연산자니깐 true, false를 리턴하는데 이거는 그 값을 리턴하네? 그런갑다...
        [자스논리연산자MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/%EB%85%BC%EB%A6%AC_%EC%97%B0%EC%82%B0%EC%9E%90(Logical_Operators))
    
    - 0 == false => true
    - "" == false => true
    - null == false => false
    - 0 == "0" => true
    - __그러니깐 === 를 써서 타입까지 확인해라.. 헷갈리게 하지말고__
    - 참고로 삼항연산자는 ==을 사용하는 듯
    
* 자바스크립트의 타입
    - undefined, null, boolean, number, string(String 아닌갑다), object, function
    - 그러니깐 타입이 실행타임에 결정된단다..
    - toString.call 이라는게 있는데, 이걸로 타입을 확인하면 아주 정확하게? 뭐가 정확한거지.. 여튼 검색해봐라
        - 생각해보면 옛날에 타입 분명히 맞는데 다르다고 계속 나온게 있었지 이럴때 쓰면 되는 듯?
        - 댓글에 나와있는데, 어떤 객체의 인스턴스인지까지 알려준단다.
    - ※ char 없다 string임, int/float 없다 number임
    
> 딴건 자바랑 비슷한듯?  
> MDN 사이트.. 봐야겠지? 나중에 꼭!
[20만js강의전준비자료](https://school.programmers.co.kr/courses/9998/lessons/57828)
    
#### 비교, 반복(이런걸 튜링연산이라 하나?), 문자열 처리
* 반복문 arr.length 효율적으로
```javascript
var arr = [1,2,3];
for(var i = 0, len = arr.length; i < len; i++) {
    // arr.length
}
```
- 요새 for-of 라는 것도 나왔다

* 문자열 처리
- 문자열도 런타임때 객체(object)로 순간적으로 변해서 메서드를 쓸수 있다
```
"ab:cd".split(":");
"ab:cd".replace(":", "$");
" abced   ".trim();
````



#### ★함수★

- 넘나 중요한 것
```
function printName(firstname) {
    var myname = "jisu";
    return "name is " + firstname;
}

console.log(printName());
// firstname이 undefined을 할당이 된다.
console.log(printName('jisu', 'crong'));
// jisu가 출력이 된다.. 신기 이유를 안가르쳐 주네 걍 그런갑다~
```

- 함수 표현식 : 변수값에 함수를 담아 놓은 것
- 함수 선언식 : 
```
function printName(firstname) {

    var result = inner();
    console.log("name is "+ result);
    
    // var inner = 이게 있으면 함수 표현식이여서 타입 에러가 나는데, 없으면 함수 선언문이여서 된단다. 신기방기
    function() {
        return "inner value";
    }
}

printName();
// name is inner value가 출력이 된다
```

- 아주 말도 안되는데? 여기서 나온 개념
* 함수 - 표현식과 호이스팅
    - 자바스크립트에서는 실행이 되기 전에 함수, 변수에 대해 한번 훑고 한다
    - 이거 자바도 그런가? 옛날에 시스템프로그래밍 할때 무슨 테이블 같은게 있었는데
    거기서 컴파일 타임에 이런거 해줬었는데 함수변수 분류.. 근데 그거는 C고 자바도 똑같은거 같은데 함수가 밑에있다고 실행 안되고 이러진 않잖아 근데 뭔 테이블(아 심볼 테이블이였나) 만들진 않고 스태틱 영역에 떄려박을려나.. 아님 따로있나 관련 링크 [C 컴파일](https://gracefulprograming.tistory.com/16)[JAVA 컴파일](https://aljjabaegi.tistory.com/387)
    - 어쨌든 이 개념은 아니고, 호이스팅 이라는게 있는데 함수 선언된거는 위로 끌어 올린단다. 검색해란다
    - 댓글보니 호이스팅 하면 영 코드가 이상해지므로 표현식만 쓰는게 더글라스 크락포트가 말했단다  [덕's IT Story](http://itstory.tk/entry/4-함수와-프로토타입-체이닝-1?category=969082)

- 함수의 반환값은 기본으로 undefined. 딴건 안그런가?
- 자바처럼 void, int 이런게 없다. 그러니깐 형을 정한다는 개념이 없다 타입스크립트는 모르겠지만..

#### 함수의 arguments, 즉 인자에 대해

```
function a() {
    console.log(arguments);
    otherMethod(arguments[1]);
}
a(1,2,3);
// 맵형식으로 1,2,3이 출력된다
// 근데 for문해서 arguments[i] 해도 된단다. 왜? 몰러.. 맵형식인거야 뭐야
```
- 근데 arguments로 뭐 조작할려고 하면 안좋단다 이유는 잘 몰겠네
- arrow function 좀 알면 좋다

### window 객체, setTimeout()

* 함수의 인자로 함수를 받을수 있는데, 나중에 실행되는 함수를 콜백함수라고도 한다.
    - 보통 함수 인자를 왜받겠냐? 나중에 써먹을려고 받지 그러니깐 콜백함수 개념이 나오는거
    
> 여기서 setTimeout()하니깐 생각나는 비동기 예제
여기서 바로 비동기로 들어간다

### 비동기, Ajax, Promise 패턴

즉, 반복문 안에 비동기 함수가 들어가는 예제
```
for(var i = 0; i < 10; i++) {
    setTimeout(function() {
        console.log(i);
    }, 500);
}
```
이거 결과는 당연히 다 10 뜬다.
그럼 이거 가장 간단하게 어떻게 동기적으로 하냐?

```

for(var i = 0; i < 10; i++) {
    var a = function(i) {
		setTimeout(function() {
        	console.log(i);
    	}, 500);
    }
	a(i);
}

```
- 이러면 반복문 보낸 순서대로 받으니깐 당연하지??
- 이게 비동기도 제대로 모르고 구글맵코딩 때릴때 밤새 고민하던 문제였는데.. 아주 기초중에 기초
- 또 자바에서 콜백 썼을때 어캐 했었냐

```
public abstract class FCallback {
    public void onSuccess(){

    }
    public void onFailure(){

    }
}
```
- 요래가지고... 도대체 어떻게 한거야 아오
- 별거없네 그냥 java.net.HttpURLConnection으로 비동기 요청 보내는데 

```
HttpGet이란 사용자 정의 클래스를 예로 들자.
이거는 AsyncTask에서 상속을 받는데, 안드로이드에서 쓰는거라.. 새로 배워야할라나 REST 받아올려면
아니 rest 받을려면 걍 아작스 조지면 되지 않나? 굳이 백엔드에서 받아서 mvc로 넘길 필요가 있나..
아니 그럼 백엔드가 왜있어.. 뭐 OAuth 하고 할려고? 그럼 비동기처리 안하나 일단 이건 나중에 생각하고

여튼, 이 AsyncTask를 상속받은 HttpGet이 자동으로 커넥션 연결된걸 실행하는데, 리스너에 onSuccess(result)해서
result, 즉 받아온 값을 저장한다

그러면 액티비티에서 프로토콜의 메소드를 실행하는데 리스너의 추상메소드(onSuccess() 같은)를 구현해서 실행하므로
그 리스너에 구현된 걸 HttpGet이 AsyncTask가 실행하는 대로 실행 될 때 구현이 된 것이 실행되므로 다른 액티비티에서 받아오는 것 처럼 사용할 수 있다.. 좀 복잡하네 여튼 이런식으로 함
```

- 이제 비동기 처리 프로미스패턴으로 하자... 제발좀

### Event
- Element.addEventListner("click", function() {}, false);

##### test.js
```
var el = document.querySelector(".outside");

el.addEventListener("click", function(e) {
    console.log("clicked!!", e);
    var target = e.target;
    console.log(target.className, target.nodeName);
    console.log(target.innerText);
    debugger;
});
```


##### test.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div class="outside">outside element</div>
    <script src="test.js"></script>
</body>
</html>

```


### Ajax
- XMLHttpRequest 통신
- JSON(JavaScript Object Notation)의 약자로, 자바스크트립스에서 객체를 만들때 사용하는 표현식을 의미한다. 
> [참고영상] Philip Roberts: What the heck is the event loop anyway? : 아주 좋단다
- var json = JSON.parse("{}");



#### CORS
> 와 JSONP가 여기서 나오네.. 뭔지는 모르겠다

[설명링크!!](http://huns.me/development/1291)

- cross domain 문제





### 배열

- 리스트 형태
- var a = [];
- [1,2,3,4].indexOf(3); // 2
- [1,2,3,4].concat(2,3); // [1,2,3,4,2,3]
- [1,2,3,4].join(); // "1,2,3,4"
```
// forEach문.. 구형함수
[1,2,3,4].forEach(function(value,index,object) {
	console.log(v)
});
//- 1급 함수 : 함수를 변수로 취급한다! (함수를 동작하는 함수)

var mapped = [1,2,3,4].map(function(v) {
	return v * 2; // 배열의 원소를 돌면서 값을 변경시킨 후에 새로운 배열로 만들어서 반환한다 
});
```

### 객체
- JSON, 즉 맵 형태
- {}
```
{key:"value"}
console.log(myFriend["key"])
console.log(myFriend.key)

// for-in 으로 하면 편리하다
// Object.keys({~~~}) 하면 오브젝트 안에 있는 키값이 배열로 나오므로 뭐 포이치를 쓰든 포문을 쓰든 할 수 있다.
```


### DOM 조작
- document.getElementById()
- .querySelector() : css 셀렉터로 접근

- DOM 탐색 API
	- element의 유용한 속성 : tagName, textContent, nodeType
	- firstChild(공백, 텍스트도 포함), firstElementChild
- DOM 조작 API
	- removeChild()
	- appendChild()
	- insertBefore()
	- cloneNode()
	- replaceChild()
	- closest()
	
- ※ 크롬 개발자 도구 $0
- ※ 요새는 귀찮아서 프레임워크 쓰는게 편하긴 하다



------------------------------------------

# CSS

### 애니메이션

- 반복적인 움직임의 처리
- css3의 transition, transform
- FPS : 1초에 표현할 수 있는 정지화면 수, 60fps가 매끄러움
- 간단, 규칙 : css, 세밀한 조작 : js
- Javascript 애니메이션
	- setInterval() : 잘 안씀..
	- setTimeout() 재귀 호출
	- requestAnimationFrame
		- 해봐라 중요하다~~
- CSS로 애니메이션 구현하기
	- transform, transition
	- ※ inline 아니면 .으로 못 가져온다 제이쿼리하면 getCSS 이런거 된다고는 하는데...
	- transition에 ease-in-out 뭐 이런게 있단다
	- transform 아래에 GPU가속을 이용하는 하위 속성들이 많다.. scale(), rotate() 등

- load, DOMContentedLoaded
	- document.addEventListener("DOMContentloaded", function(){})
	- document.addEventListener("load", function(){})

- event delegation
	- ul > li > img에서 target(img, 클릭한 지점), currentTarget(ul, 이벤트가 발생한 지점)
	- 이벤트 버블링
	- 클릭한 지점이 하위 엘리먼트라고 해도, 그것을 감싸는 상위 엘리먼트까지 올라가면서 이벤트를 다 실행한다.
	- 
- HTML template
	- 서버에서 file로 보관하고 ajax로 요청해서 받아온다
	- html 코드안에 숨겨둔다
	=> script 태그가 type이 자바스크립트가 아니라면 렌더링하지않고 무시하는 것을 이용해 숨긴다
```
<script id="template-list-item" type="text/template"></script>
```
	- 템플릿 리터럴
	

------------------------------

# Spring Framework
- AOP와 instrumentation
	- instrument.. 라는게 있는데 여기서 안한다네? ....
	- Message 핸들러. 백기선에서 잠시 봄
	- spring-jdbc, spring-tx
	
## Spring JDBC

- JdbcTemplate (NamedParameterJdbcTemplate)
- 

## 레이어드 아키텍처

- 중복 되는 부분(Tab UI 같은..)
- 컨트롤러에서 중복되는 부분을 처리할려면?
	- 별도의 객체로 분리한다
	- 별도의 메소드로 분리한다
	- 예를들어 게시판에도 회원 정보를 보여주고, 상품 목록 보기도 회원 정보를 보여주는데
	- 그러면 중복되는 부분인 회원 정보를 읽어오는 코드는 어떻게 해야할까??
	- > 중복되는 부분을 서비스로 구현
- 서비스
    - 비즈니스 서비스
    - 하나의 비즈니스 로직은 하나의 트랜잭션으로 동작한다!
- 트랜잭션
    - ACID : 원자성, 일관성, 독립성, 비의존성
        - 원자성 : 전체가 성공하거나 전체가 실패한다 ex) 출금 : 잔액 조회하고 출금 금액이 잔액보다 작은 지 확인하고 잔액 빼고 언제 어디서 출금했는지 정보를 기록하고 출금한다
        - 일관성 : 트랜잭션의 작업 처리 결과가 항상 일관성이 있어야 한다
            - 트랜잭션이 진행되는 동안 데이터가 바뀌더라도 바뀐 데이터를 참조하면 안된다
        - 독립성 : 둘이 상의 트랜잭션이 동시에 병행 실행 되고 있을 때 어느 하나의 트랜잭션이 다른 트랜잭션의 연산을 끼어들 수 없다.
        - 지속성 : 트랜잭션이 성공적으로 완료되었을 경우, 결과가 영구적으로 반영되어야 한다.
        
    - setAutoCommit, @EnableTransactionManagement

### 설정의 분리
> 어려운데..  ContextLoaderListener? 

- Spring 설정 파일을 프리젠테이션 레이어쪽과 나머지를 분리할 수 있습니다.

- web.xml 파일에서 프리젠테이션 레이어에 대한 스프링 설정은 DispathcerServlet이 읽도록 하고, 그 외의 설정은 ContextLoaderListener를 통해서 읽도록 합니다.

- DispatcherServlet을 경우에 따라서 2개 이상 설정할 수 있는데 이 경우에는 각각의 DispathcerServlet의 ApplicationContext가 각각 독립적이기 때문에 각각의 설정 파일에서 생성한 빈을 서로 사용할 수 없습니다.

- 위의 경우와 같이 동시에 필요한 빈은 ContextLoaderListener를 사용함으로써 공통으로 사용하게 할 수 있습니다.

- ContextLoaderListener와 DispatcherServlet은 각각 ApplicationContext를 생성하는데, ContextLoaderListener가 생성하는 ApplicationContext가 root컨텍스트가 되고 DispatcherServlet이 생성한 인스턴스는 root컨텍스트를 부모로 하는 자식 컨텍스트가 됩니다.

- 참고로, 자식 컨텍스트들은 root컨텍스트의 설정 빈을 사용할 수 있습니다.
~~써봐야 알듯...~~




### 실습 : 방명록 만들기
- Spring jdbc로 Dao 작성
- 레이어드 아키텍처 구성
- 트랜잭션 처리, mvc에서 폼 값 입력받기, redirect, 컨트롤러에서 jsp에 전달한 것을 jstl과 el을 이용해 출력
- 페이지 링크(페이징)


guestbook - id, name, content, regdate
guestbook/list
guestbook/write

log - id, ip, method, regdate

- web.xml, WebMvcContextConfiguration.java
- ApplicationConfig.java, DbConfig.java





-------------------------------

> 내용도 너무 많고 복잡하네.. MVC 부분이 특히 maven 프로젝트에서 시작해서 설정해야해서 생각보다 복잡함
- 자바스크립트 스터디 문제.. 구글드라이브에 있다 참조























