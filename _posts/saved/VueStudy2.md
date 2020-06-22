---
layout: post
title: "뷰 공식 문서 공부_2"
date: 2019-07-02
categories: "doodling"
tags: [Vue.js, javascript]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---


## 클래스와 스타일 바인딩

- 데이터 바인딩은 앨리먼트의 클래스목록과 인라인 스타일을 조작하기 위해 일반적으로 사용(v-bind)
- class와 style에 v-bind를 사용할 때 특별한 기능 제공

### HTML 클래스 바인딩하기

#### 객체 구문


```html
<div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
```


```js
var vm = new Vue({
  el : '#app',
  data : {
    isActive: true,
    hasError: false
  }
})
```


- 요렇게 쓴다.. false면 사라지는거 알지?
- 그리고 v-bind:class는 일반 class랑 공존할 수 있단다


```html
<div v-bind:class="classObject"></div>
```


```js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```


- 똑같단다. 훨씬 간단하네


```js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```


- computed의 객체를 반환하는 속성에도 바인딩할 수 있단다 괜히 더 복잡한거 같은데.. 좋은거라네 뭐가 좋지


#### 배열 구문


```html
<div v-bind:class="[activeClass, errorClass]"></div>
```


```js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```


- 토글할려면?
  - 삼항 연산자(문서봐라 생략, 비효율적)
  - 배열 내 객체 구문
  - 이렇게 한다..
  
  
```html
<div v-bind:class="[{ activeClass: isActive }, errorClass]"></div>
```


- 여기서 errorClass는 무조건 들어가고, active는 isActive에 의해 결정된다.(객체 구문)
- 이거 왜쓰지? 고정 시킬려면 걍 class에 넣으면 되고, 토글할려면 객체 구문이 훨씬 편하고.. 흠 몰라 이런게있다

#### 컴포넌트와 함께 사용하는 방법

- [컴포넌트](https://kr.vuejs.org/v2/guide/components.html)


```js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```


```html
<my-component class="baz boo"></my-component>
```


- 렌더링 결과


```html
<p class="foo bar baz boo">Hi</p>
```


- 어캐된거냐, 템플릿의 클래스가 컴포넌트의 루트 엘리먼트에 추가된다.

### 인라인 스타일 바인딩

- 뭐 비슷하네 style에 객체로 때려박아라
- 자동접두사? 뭐야이게 뭐 자동감지한다는데.. [벤더 접두사](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix)
- 다중 값 제공 : 브라우저가 지원하는 배열의 마지막 값만 렌더링 한단다 나중에 나오면 봐라

## 조건부 렌더링

- v-if에 대해..
- key : 이 엘리멘트는 완전히 별개이므로 다시 사용하지 마세요(원래는 안에 속성만 바뀌는데.. 여튼 [공식문서](https://kr.vuejs.org/v2/guide/conditional.html#key%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%9E%AC%EC%82%AC%EC%9A%A9-%EA%B0%80%EB%8A%A5%ED%95%9C-%EC%97%98%EB%A6%AC%EB%A8%BC%ED%8A%B8-%EC%A0%9C%EC%96%B4) 봐라)
- v-show : v-if랑 똑같은데, 엘리멘트의 display만을 조작하는 갑다.


## 리스트 렌더

- v-for
- 예제가 아~주 많네..
- enumarte 처럼 index를 쓸수 잇네
- of란 것도 있다네 이건 뭐야? 
- key도 있네. 객체 반복돌리면 (key, value), 걍 공통 키거나 그냥 배열이면 걍쓰면됨 index 는 둘다 됨
- 이게 자바스크립트의 Object.keys()인거 같은데
- [key](https://kr.vuejs.org/v2/guide/list.html#key) ? 뭐야이거 하나도 모르겠네

### 배열 변경 감지

- push pop sort reverse splice 등(원본 변형)
- 새 배열 반환 filter concat slice
- [주의사항!](https://kr.vuejs.org/v2/guide/list.html#%EC%A3%BC%EC%9D%98-%EC%82%AC%ED%95%AD), 변경 감지 못하는 경우..

#### [할일 목록 전체 예제](https://kr.vuejs.org/v2/guide/list.html#v-for-%EC%99%80-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8)

~~index4.html 보면 있다~~
- li 안에 is 속성 뭐야이거? 이게 컴포넌트로 정의하는 건가..
- 일단 보면 todo-item이라는 컴포넌트는 `<li>{{title}}<button></li>`로 구성되있고
- emit? 이게 뭘까..
- 컴포넌트를 해야 알겠구만..
- 잘 보면 느낌은 온다.
  
## 이벤트 핸들링

- 한번 읽어 봐라

## 폼 입력 바인딩

- v-model
- 여기 폼 예제 좋은게 너무너무 많다 잘 빼껴 쓰도록




> 드디어 기본 다봤고.. 이제 제일 중요한 컴포넌트, 컴포넌트 등록(슬롯은뭐야?), 렌더, 싱글파일컴포넌트, 라우팅, 상태관리, 서버사이드 렌더링.. 을 해야한다. 아오 너무 많네 오늘은 싱글파일컴포넌트 까지만 하자



                                                                               
