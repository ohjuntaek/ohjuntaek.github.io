---
layout: post
title: "뷰 공식 문서 공부"
date: 2019-07-01
categories: "doodling"
tags: [Vue.js, javascript]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---


## vue-cli 기본 프로젝트 구조
- 디렉티브 : v- : 렌더링 된 DOM에 특수한 반응형 동작?

## SFY 1주차 프로젝트 구조(vue-cli)

- index.html -> main.js -> App.vue
                                -> HomePage.vue
                           -> HomePage.vue

# Vue js 공식홈피 공부

## 시작하기

```
<div id="app">
  {{ message }}
</div>

var app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!'
  }
})

<div id="app-2">
  <span v-bind:title="message">
    내 위에 잠시 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!
  </span>
</div>

var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '이 페이지는 ' + new Date() + ' 에 로드 되었습니다'
  }
})

```
- 속성에 data 넣고 싶으면 v-bind, 걍 넣을려면 {{}} 쓰는 건가?
- 그 외 디렉티브 : v-if, v-for.. 전부 data에 있는거 써먹는다
- v-on : methods에 있는거 써먹는다. 이벤트 리스너..
- v-model : 양방향 바인딩이란다. data에 있는거 써먹는다. input에 v-model 속성 딱넣어주면 된다

### 컴포넌트를 이용한 작성 방법
```
<ol>
  <!-- todo-item 컴포넌트의 인스턴스 만들기 -->
  <todo-item></todo-item>
</ol>

// todo-item 이름을 가진 컴포넌트를 정의합니다
Vue.component('todo-item', {
  template: '<li>할일 항목 하나입니다.</li>'
})
```
- 이건 왜 el 안쓰고 머 이상하게 했냐.. 태그를 고대로 때려박네 여튼 이게 컴포넌트란다



```
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <div id="app-7">
      <ol>
        <!--
          이제 각 todo-item 에 todo 객체를 제공합니다.
          화면에 나오므로, 각 항목의 컨텐츠는 동적으로 바뀔 수 있습니다.
          또한 각 구성 요소에 "키"를 제공해야합니다 (나중에 설명 됨).
         -->
        <todo-item
          v-for="item in groceryList"
          v-bind:todo="item"
          v-bind:key="item.id">
        </todo-item>
      </ol>
    </div>
  </body>
</html>

<!-- 도움되는 콘솔 경고를 포함한 개발 버전 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script type="text/javascript">
  Vue.component('todo-item', {
    props: ['todo'],
    template: '<li>{{ todo.text }}</li>'
  })

  var app7 = new Vue({
    el: '#app-7',
    data: {
      groceryList: [
        { id: 0, text: 'Vegetables' },
        { id: 1, text: 'Cheese' },
        { id: 2, text: 'Whatever else humans are supposed to eat' }
      ]
    }
  })
</script>

```
- 요건 뭐야..new Vue랑 component를 같이썼네
- component가 todo-item 같이 태그 만드는? 근데 태그라 부르면 안되나.. 여튼 component 주석치니깐 <todo-item> 이게 없다고 오류뜨네
- props는 뭐여.. 아오 부모 컴포넌트의 데이터를 자식 컴포넌트에 넣는다? 이거 js스터디할떄 문제 어디서 본거 같은데
- 어쨌든 #app-7 > ol > todo-item 인데 Vue 객체? 컴포넌트아냐? new Vue는 #app-7에 생성햇고
- 아오 모르겠다 걍 속성인가? 속성을 정의하는데 item이 도는거니깐 item을 받는데
- `<todo-item>` 컴포넌트는 `<li>{{todo.text}}</li>` 를 받으니깐 이걸 딱딱 넣어주는 갑네
- 아니 아까 v-for를 대충 넘기니깐 모르네... 다시 보자
  
```
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
  
  
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'JavaScript 배우기' },
      { text: 'Vue 배우기' },
      { text: '무언가 멋진 것을 만들기' }
    ]
  }
})
```
- 이것도 1 머시기 2 머시기 뜨거든 즉 li를 3번 돌려준다 delegating? 같은거 안하고
- 그러니깐 위에꺼도 todo-item을 돌리는데, 이 태그는 컴포넌트로 선언되어있고 todo에 item을 넣기 때문에 내용이 생성된다
- 키는 뭐냐? 모르겠다..
- 그리고 부모에서 prop으로 자식으로 전달된다는데 prop 안쓰면 못쓰나? 확인해보자
- 걍 v-for {{item}} 해도 잘만 되네.. 내생각엔 prop란 속성을 통해 v-bind로 넣어서 다시 자바스크립트에서 렌더링 한다는 거를 말하는거 같네 왜 이딴식으로 하냐? 뭔가 자식과 부모 분리? 그런건가.. 잘모르겠네

> 여기서 갑자기 컴포넌트 공부해라고 하면서 링크를 보여준다 컴포넌트 공부
-----------------
## 컴포넌트
- 뭐 캡슐화 어쩌고 나오다가 is 속성? 이건 뭐야 확장된 원시 html 엘리먼트..
### 컴포넌트 사용하기
#### 전역 등록
- 아 Vue.component가 전역 컴포넌트 등록이란다. Vue는 새 Vue 인스턴스고..
- 이 component는 template으로 딱 렌더링이 되네 new Vue는 렌더링이 {{}} 직접 해줬어야 했던거 같은데

#### 지역 등록
- new Vue안에 component 속성 넣으면 지역적으로 등록된단다. 예시 적어볼까?

```
var Child = {
  template: '<div>사용자 정의 컴포넌트 입니다!</div>'
}

new Vue({
  // ...
  components: {
    // <my-component> 는 상위 템플릿에서만 사용할 수 있습니다.
    'my-component': Child
  }
})
```

- 아니 너무 긴데... 여튼 new Vue는 인스턴스라하고 컴포넌트는 컴포넌트네
- 다시 돌아가자..

--------------------------------

## Vue 인스턴스

- 아 요 new Vue({}) 안에 들어가는게 options 객체인갑다

### 속성과 메소드
- 각 Vue 인스턴스는 data 객체에 있는 모든 속성을 프록시 처리한다. 뭐 바로 .쓸수 잇다는 건가?
- 여튼 바뀌면 화면에 바로 렌더링 되는데, 이미 있는거만 그러니깐 쓸거있으면 미리 초기화해놔라
- Object.freeze () 란게 있단다
- $붙인거는 기본적으로 있는 인스턴스 속성이란다. 예시를 보자


```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 는 인스턴스 메소드 입니다.
vm.$watch('a', function (newVal, oldVal) {
  // `vm.a`가 변경되면 호출 됩니다.
})
```


- 사용자 정의속성 (봐라 위에 var data 있잖아 헷갈리니깐..) 이랑 구분할려고 붙여준단다.

### 인스턴스 라이프사이클 훅
- 다이어그램.. 
- init 상태야. beforeCreate 생성. 그다음 init 되고 created. 여기서 el이 잇냐?
  - el 없잖아? vm.$mount(el) 불러
- template 있냐? 있으면 이걸로 render 시키고 없으면 밖에 {{}}이거 써먹는 건가..??
- 그리고 beforeMount하고 vm.$el을 생성한다?? 뭐 엘리먼트 찾는건가..
- 그리고 mounted 하고 마운트 된다? 이러면서 반응형으로 랜더 시키는갑네 그러면서 update
- 그리고 사라진단다. destroyed


> 아오 지겹네 많이 한거 같으니 이제 vue-cli 구조나 보자
>> 하나도 몰것네...

~~인스턴스 라이프사이클 아니 봤는데 우예쓰는지 모르겠는데? 예시를 써봐야하는데..~~


```javascript
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 는 vm 인스턴스를 가리킵니다.
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```


- 이러면 인스턴스가 생성된 후에 (즉, a가 생성된 후에) created의 function이 실행되서 a is 1 이나온다.
- 만약 created 안하고 그냥 methods해서 실행하면 안되겄지?
- 되는데? 그리고 methods 선언하고 {{t()}} 쓰니깐 실행되네.. {{}}가 의미하는게 뭘까
- 여튼 라이프사이클이랑 MVVM 패턴이랑 컴포넌트를 완벽히 익혀야겠다


## 템플릿 문법

- Vue.js는렌더링된DOM을기본인스턴스의데이터에선언적으로바인딩할수있는HTML기반템플릿구문을사용합니다모든Vuejs템플릿은스펙을호환하는브라우저및HTML파서로구문분석할수있는유효한HTML입니다내부적으로Vue는템플릿을가상DOM렌더링함수로컴파일합니다반응형시스템과결합된Vue는앱상태가변경될떄최소한으로DOM을조작하고다시적용할수있는최소한의컴포넌트를지능적으로파악할수있습니다가상DOM개념에익숙하고...렌더링함수를직접작성할수있으며선택사항으로JSX를지원합니다
- 먼소리야?


### 보간법(Interpolation)


#### 문자열 보간법
- 데이터 바인딩의 가장 기본형태는 Mustache 구문을 사용한 텍스트 보간( 걍 {{msg}} )
- 데이터 객체의 msg 속성이 변경될 때마다 갱신된다
- v-once 디렉티브 : {{}}가 다시는 변경되지 않는다.


#### 원시 HTML 보간법
- 무스태치는 HTML이 아닌 일반 텍스트로 데이터를 재해석한다
- 그러므로 html을 출력하려면 v-html을 사용해야 합니다.~~어 근데 신기하게 메소드는 출력되네.. 하긴 html이 아니니깐~~
- 예시


```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```


- Vue는 문자열 기반 템플릿 엔진이 아니기 때문에 v-html을 이용해 템플릿을 사용할 수 없다.
- 먼소리야? <template>을 v-html으로 못쓴단건가... 문자열 기반 템플릿 엔진이 아니라서? 몰라그런갑다~
- 컴포넌트는 또 기본단위로 사용.. 먼소리야 번역이상한건가
- [XSS 취약점](https://en.wikipedia.org/wiki/Cross-site_scripting) 갑자기 나오네. 위키공부 꼭 하기
  - 여튼 HTML 보간은 신뢰할 수 있는 콘텐츠에서만 사용하고 사용자가 제공한 콘텐츠에서는 사용하면 안된다?
  - HTML 보간이 v-html 말하는 건가? 텍스트 보간이 {{}}니깐..  
- 그렇단다


#### 속성 보간법

- 머스타취는 HTML 속성에서 사용 못하므로 v-bind 사용해라. v-bind가 아.. 속성에 data 집어넣고 싶을떄 쓰는 거인거 같네


```html
<button v-bind:disabled="isButtonDisabled">Button</button>
isButtonDisabled가 null, undefined 또는false의 값을 가지면 disabled 속성은 렌더링 된<button>엘리먼트에  포함되지 않습니다.
```


#### JavaScript 표현식 사용

- 어떻게 구분하는진 모르겠지만.. javascript 표현식도 머스타치구문에서 사용할 수 있단다 단 단일표현식만! 구문안됨
- 뭐 템플릿 포현식은 샌드박스처리? Math 같은 전역 사용에만 접근? 사용자 정의 전역에 액세스하지말라? 먼소리야


### 디렉티브

- v 머시기
- 디렉티브 속성값은 단일 JavaScript 표현식이 된단다(v-for제외)
- 디렉티브의 역할은 표현식의 값이 변경될 때 사이드 이펙트를 반응적으로 DOM에 적용하는 것
- 디렉티브 생각나는거 v-bind, v-model, v-if, v-for.. 정도


```javascript
<p v-if="seen">이제 나를 볼 수 있어요</p>
```


#### 전달 인자

- 뭐야이게


```html
<a v-bind:href="url"> ... </a>
```


- 여기서 href가 전달인자라는데? 걍 속성아냐? 
- 앨리먼트의 href 속성을 표현식 url의 값에 바인드 하는 v-bind 디렉티브에게 알려준다?? 먼소리야? 걍 바인딩해준다 이소린듯..
- DOM 이벤트를 수신하는 v-on 디렉티브


```html
<a v-on:click="doSomething"> ... </a>
```


#### 수식어

- 점으로 표시되는 특수 접미사. 첨보는 건데
- 디렉티브를 특별한 방법으로 바인딩 해야 함을 나타낸다.
- .prevent : 트리거된 이벤트에서 event.preventDefault()를 호출하도록 v-on 디렉티브에게 알려준다


```html
<form v-on:submit.prevent="onSubmit"> ... </form>
```


- 이러면 기본을 막도록.. 그러니깐 내장되어있는 건가?
- v-on, v-model 사용할때 수식어 많단다


### 약어

- v-bind: 약어 :
- v-on 약어 @


## Computed와 watch

### Computed 속성

- 템플릿에 표현식({{}}) 을 넣으면 편리하지만 간단한거만 넣어라


```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```


- 이런식으로 하면 안좋음
- 그래서 computed 쓴다

#### 기본 예제


```html
<div id="example">
  <p>원본 메시지: "{{ message }}"</p>
  <p>역순으로 표시한 메시지: "{{ reversedMessage }}"</p>
</div>
```


```js
var vm = new Vue({
  el: '#example',
  data: {
    message: '안녕하세요'
  },
  computed: {
    // 계산된 getter
    reversedMessage: function () {
      // `this` 는 vm 인스턴스를 가리킵니다.
      return this.message.split('').reverse().join('')
    }
  }
})
```


- methods랑 차이는? methods는 reverseMessage() 해줘야 실행이 됐었는데, 이거는 안해도 되네. 이게 차이는 아닐꺼고..
- 모르겠다. 의존관계? reversedMessage에 대한 getter 함수?

#### computed 속성의 캐싱 vs 메소드
- 오 여기 내가 궁금한거 똑같이 나오네


```html
<p>뒤집힌 메시지: "{{ reversedMessage() }}"</p>

// 컴포넌트 내부
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```


- __차이점은 computed 속성은 종속 대상을 따라 저장(캐싱)된다는 것__
- 먼소리냐. 해당 속성이 종속된 대상이 변경될때만 함수를 실행한다.
- 이에 비해 methods를 호출하면 렌더링을 할때마다 항상 함수를 새로 실행한다~~오호라...~~

#### computed 속성 vs watch 속성
- 명령형 프로그래밍(watch) vs 선언형 프로그래밍(computed)
- 비교분석


```
<div id="demo">{{ fullName }}</div>
```



```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```


- watch 잘 모르겠네.. firstName이 function(val)로 들어가서 firstName이 바뀌면 이 함수가 실행되는 건가


```js
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```



- 목표데이터를 직접 정의하므로 더 간결해지는 듯..

#### computed 속성의 setter 함수

- 아 getter가 기본이고 setter가 따로 있구나.. 이러니깐 computed가 좀 감이오네


```js

// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...

```


- vm.fullName = 'John Doe' 하면 밑에 setter함수가 실행된단다. 굳


### watch 속성
- 와 예제가 너무 좋은데?


```html
<div id="watch-example">
  <p>
    yes/no 질문을 물어보세요:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```


```js
<!-- 이미 Ajax 라이브러리의 풍부한 생태계와 범용 유틸리티 메소드 컬렉션이 있기 때문에, -->
<!-- Vue 코어는 다시 만들지 않아 작게 유지됩니다. -->
<!-- 이것은 이미 익숙한 것을 선택할 수 있는 자유를 줍니다. -->
<script src="https://unpkg.com/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://unpkg.com/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: '질문을 하기 전까지는 대답할 수 없습니다.'
  },
  watch: {
    // 질문이 변경될 때 마다 이 기능이 실행됩니다.
    question: function (newQuestion) {
      this.answer = '입력을 기다리는 중...'
      this.getAnswer()
    }
  },
  methods: {
    // _.debounce는 lodash가 제공하는 기능으로
    // 특히 시간이 많이 소요되는 작업을 실행할 수 있는 빈도를 제한합니다.
    // 이 경우, 우리는 yesno.wtf/api 에 액세스 하는 빈도를 제한하고,
    // 사용자가 ajax요청을 하기 전에 타이핑을 완전히 마칠 때까지 기다리길 바랍니다.
    // _.debounce 함수(또는 이와 유사한 _.throttle)에 대한
    // 자세한 내용을 보려면 https://lodash.com/docs#debounce 를 방문하세요.
    getAnswer: _.debounce(
      function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = '질문에는 일반적으로 물음표가 포함 됩니다. ;-)'
          return
        }
        this.answer = '생각중...'
        var vm = this
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = '에러! API 요청에 오류가 있습니다. ' + error
          })
      },
      // 사용자가 입력을 기다리는 시간(밀리세컨드) 입니다.
      500
    )
  }
})
</script>
```


- 코드는 잘 모르겠지만.. 이런게 있다~
- [vm.$watch API](https://kr.vuejs.org/v2/api/#vm-watch)가 있단다 또 ㄷㄷ















