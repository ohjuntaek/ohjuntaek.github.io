---
layout: post
title: "뷰 공식 문서 공부_3_컴포넌트 집중"
date: 2019-07-03
categories: "doodling"
tags: [Vue.js, javascript]
image: http://gastonsanchez.com/images/blog/mathjax_logo.png
---

# 뷰의 컴포넌트 ~ 싱글 파일 컴포넌트까지!

- 컴포넌트는 인스턴스 이기도 하다. 그러므로 모든 옵션 객체를 사용할 수 있다(루트에만 사용하는 옵션은 제외?), 그리고 같은 라이프 사이클을 사용할 수 있다.

## 컴포넌트 사용하기

### 전역 등록
- Vue.component(tagName, options)


```js
Vue.component('my-component', {
  // 옵션
})
```


- 등록되면, 컴포넌트는 인스턴스의 템플릿에서 커스텀 엘리먼트로 사용할 수 있다.


```html
<div id="example">
  <my-component></my-component>
</div>
```


```js
// 등록
Vue.component('my-component', {
  template: '<div>사용자 정의 컴포넌트 입니다!</div>'
})

// 루트 인스턴스 생성
new Vue({
  el: '#example'
})
```


- 렌더링 결과


```html
<div id="example">
  <div>사용자 정의 컴포넌트 입니다!</div>
</div>
```


### 지역 등록
- components의 인스턴스 옵션으로 등록해 다른 인스턴스/컴포넌트의 범위에서만 사용할 수 있는 컴포넌트 생성


```js
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


### DOM 템플릿 구문 분석 경고

- DOM을 템플릿으로 사용할 때 (el 옵션을 사용해 기존 콘텐츠가 있는 엘리멘트를 마운트하는 경우), Vue는 템플릿 콘텐츠만 가져올 수 있기 때문에 html이 작동하는 방식에 제한사항.. 먼소리야


```html
<table>
  <my-row>...</my-row>
</table>
```


- 그러니깐 이런게 문제라는 거지 컴포넌트 쓰면
- 그래서 is를 쓴다 아하!


```html
<table>
  <tr is="my-row"></tr>
</table>
```


- 또 뭔가있네.. 다음 소스 중 하나에 포함되면 문자열 템플릿을 사용하는 경우에는 이러한 제한사항이 적용되지 않습니다
  - <script type="text/x-template">
  - JavaScript 인라인 템플릿 문자열
  - .vue 컴포넌트
따라서 가능하면 항상 문자열 템플릿(?)을 쓰는것이 좋다. 

※ [뷰 템플릿 정의 7가지 방법](https://github.com/FEDevelopers/tech.description/wiki/Vue%EC%97%90%EC%84%9C-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%ED%85%9C%ED%94%8C%EB%A6%BF%EC%9D%84-%EC%A0%95%EC%9D%98%ED%95%98%EB%8A%94-7%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)

### [data는 반드시 함수여야 한다](https://kr.vuejs.org/v2/guide/components.html#data-%EB%8A%94-%EB%B0%98%EB%93%9C%EC%8B%9C-%ED%95%A8%EC%88%98%EC%97%AC%EC%95%BC%ED%95%A9%EB%8B%88%EB%8B%A4)
- 예제에 왜 써야하는지 잘 나와있다!

### 컴포넌트 작성
- 부모와 자식, props는 아래로, events 위로..??
- 부모는 props를 통해 자식에게 데이터를 전달
- 자식은 events를 통해 부모에게 메세지를 보낸다..


## Props

### Props로 데이터 전달하기
- 모든 컴포넌트 인스턴스는 자체 격리된 범위가 있다.
- 즉, 하위 컴포넌트의 템플릿에서 상위 데이터를 직접 참조할 수 없으며 그러면 안된다.
- 데이터는 props 옵션을 사용해 하위 컴포넌트로 전달


```html
<!DOCTYPE html>
<html lang="en" dir="ltr">

<head>
  <meta charset="utf-8">
  <title></title>
</head>

<body>
  <div id='app'>
    <child message="안녕하세요!"></child>
  </div>
  
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script type="text/javascript">
    Vue.component('child', {
      // props 정의
      props: ['message'],
      // 데이터와 마찬가지로 prop은 템플릿 내부에서 사용할 수 있으며
      // vm의 this.message로 사용할 수 있습니다.
      template: '<span>{{ message }}</span>'
    })

    var vm = new Vue({
      el: '#app'
    })
  </script>
</body>

</html>

```


- 요렇게 상위 컴포넌트의 속성으로 전달하는데, 이거는 직접 값을 넣은거고 index4.html(VueStudy2의 할일 목록 예제) 처럼 v-bind써서 넣는게 일반적일듯


#### camelCase vs kebab-case

- html 속성은 대소문자를 구분 안한단다 아하! 어쩃든 그래서 케밥케이스를 써라..

#### 동적 Props

- 아까 위에 말한거네 v-bind로 속성 넣어서 props으로 전달
- v-bind에 인자없이 객체를 전달해도 props으로 전달할 수 있단다 함 보까


```js
todo: {
  text: 'Learn Vue',
  isComplete: false
}
```


```html
<todo-item v-bind="todo"></todo-item>

세임세임

<todo-item
  v-bind:text="todo.text"
  v-bind:is-complete="todo.isComplete"
></todo-item>
```


#### 리터럴 vs 동적

- 뭐 "1" 숫자로 넣으려면 v-bind 써라.. 왜냐하면 자바스크립트 표현식으로 해석되므로

> 여기서 보시라. props가 뭐냐? property 복수형이잖아? 컴포넌트의 속성이라 이거지. 속성이 뭐냐 html에서 엘리먼트의 속성.. 이러면 좀 쉬운듯


#### 단방향 데이터 흐름
- props는 상위 속성이 업데이터 되면 하위로 흐르게 된다. 하지만 반대는 안된다
- 하위 컴포넌트가 부모의 상태를 변경하는 걸 막아준다.
- props를 변경시키고 싶은 유혹 두가지 경우.. 라는데 뭔소린지 모르겠다. 예시보자


```js
props: ['initialCounter'],
data: function () {
  return { counter: this.initialCounter }
}
```


- props의 초기값을 초기값으로 사용하는 로컬 데이터 속성 정의 <- props가 초기값을 전달하는데 사용


```js
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```


- props값으로 부터 계산된 속성을 정의 <- props가 변경되어야 할 원시값으로 전달됨
- 뭐야이게.. 걍 쓰면 되는거 같은데 먼소린지 몰겠다

#### Prop 검증
- prop에 관한 요구사항을 지정
- 원래 props는 []로 들어오거든? 근데 {}로 하면 뒤에 밸류에 String 같은걸 넣어서 검사할수있다~ default도있다


### Prop가 아닌 속성
- 아니 Prop가 아닌 속성? 말이 돼?
- 컴포넌트로 전달되지만 정의되지 않은 속성.. 이란다 뭐지


```html
<bs-date-input data-3d-date-picker="true"></bs-date-input>
```


- 이게 예란다.. 모르겠는데

#### 존재하는 속성 교체/병합


```html
<input type="date" class="form-control">
```


- 이게 bs-date-input 컴포넌트의 템플릿이라 가정
- 데이터피커 플러그인이란거의 테마를 추가하기 위해 클래스를 추가한단다. 요렇게


```html
<bs-date-input
  data-3d-date-picker="true"
  class="date-picker-theme-dark"
></bs-date-input>
```


- 클래스에 뭔가 이상한게 있지?, 그리고 이상한 속성이 있는데.. 이게 정의되지 않은건가
- 여튼 이경우 class에 대한 두 개의 서로 다른 값이 정의 된단다
  1. 템플릿의 컴포넌트에 의해 설정된 form-control
  2. date-picker-theme-dark는 부모에 의해 컴포넌트로 전달됩니다
- 이게 먼소리야? large? type? form-control? 왜 서로 다른 값이 전달되지? 몰라 버려

> 그리고 부모로부터 자식으로 prop을 통해 전달한다는데 부모에서는 data에서 정의된거 v-bind를 통해 넣으니깐 되긴하네 이렇게 이해하자..

#### v-on을 이용한 사용자 지정 이벤트
- $on(eventName) : 이벤트를 감지
- $emit(eventName) : 이벤트를 트리거
- addEventListenr나 dispatchEvent랑 다르단다
- 예제


```html
<div id="counter-event-example">
  <p>{{ total }}</p>
  <button-counter v-on:increment="incrementTotal"></button-counter>
  <button-counter v-on:increment="incrementTotal"></button-counter>
</div>
```


```js
Vue.component('button-counter', {
  template: '<button v-on:click="incrementCounter">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    incrementCounter: function () {
      this.counter += 1
      this.$emit('increment')
    }
  },
})

new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
```


- 보면 이제 $on 이랑 $emit을 좀 알겠네 on은 리스너 등록(감지), emit은 실행(트리거).. 
- 이거보면 VueStudy2의 할일 예제도 좀 알거같다. 
- 여튼 이 예제는 내부 컴포넌트에서 emit을 통해 외부 이벤트를 트리거 시키는 예제인거 같다.
- 그리고 $on은 자식에서 호출한 이벤트는 감지 안한단다. emit으로 해도 감지 못하니깐 v-on을 템플릿에 반드시 지정해야 감지한다는 거 같은데.. $on쓴 예제가 없으니 감은 안오네

##### 컴포넌트에 네이티브 이벤트 바인딩

- 네이티브 이벤트가 뭐야? 


```html
<my-component v-on:click.native="doTheThing"></my-component>
```


#### 사용자 정의 이벤트를 사용하여 폼 입력 컴포넌트 만들기

- 자 v-model을 좀더 잘 이해해보자


```html
<input v-model="something">
```


이랑 같은건 뭘까?  


```html
<input
  v-bind:value="something"
  v-on:input="something = $event.target.value">
```


이랑 같단다.   

- 흠냐 $event 이게 뭐지? event.target.value 있는거 보니깐 뭔가 이벤트 객체 같은데 이게 어디서 나온겨.. 
- 여튼 v-model을 사용하면 value란 prop을 가지고, 새로운 값으로 input 이벤트를 내보낸단다. 그런거같네
- [뭔 통화 필터 예제?](https://jsfiddle.net/chrisvfritz/1oqjojjx/?utm_source=website&utm_medium=embed&utm_campaign=1oqjojjx) 가 있는데.. 어렵네 모르는게 많다.

#### 컴포넌트의 v-model 사용자 정의

> 여기까지 하면서 생각하는게 일단 v-model과 v-bind의 차이, 그리고 자식->부모, 부모->자식 data 전송  
> 1. v-model은 안에 data 값 왔다리갔다리, v-bind는 컴포넌트의 prop의 값 왔다리갔다리...
>   - prop의 값이 bind로 변하면 model 처럼 밖에꺼의 값도 변할려나? => 안변한다. bind는 자식 컴포넌트의 prop의 값을 바인딩 해주는 거라서..
>   - 그럼 prop의 값이 자식에서 변하면 밖에꺼도 변하냐? => 당연히 안변하지..라고 생각하면 안됨 일단 props를 바꾸는걸 먼저 막아버린다. 왜일까? 부모를 변화시키면 안되니까? 흠.. 
>   - 그럼 v-model로 v-bind로 props 넣어준거.. 는 못변화 시키니깐 부모에 있던 data 
> 2. 부모->자식 은 쉽다 props 써서 템플릿에 집어넣으면 됨. 자식-> 부모가 emit 써서 해야한단다 다른방법 모름..
> 3. 컴포넌트에서 v-model을 사용하면 자동으로 자식에 value라는 prop이 생성되고, 그 컴포넌트에 input이라는 이벤트 리스너가 등록된다.(위에 적어놓음) 그리고 이벤트 객체를 받아서 value값을 사용해 값을 바꾼다.
> 그러므로 emit을 사용해 자식이 바뀌면 부모꺼를 바꾸는 방식에서, v-model을 사용하면 value와 'input' event를 통해 바꿀 수 있다는 것.. 아오 어렵네 힘들었다.

- 부모 -> 자식으로, 자식 -> 부모로 data를 열심히 넘긴 코드.


```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="utf-8">
  <title></title>
</head>

<body>
  <div id="app">
    <parent>
    </parent>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script type="text/javascript">
    var vm = new Vue({
      el: '#app',
      data: {},
      components: {
        'parent': {
          template: '<div><부모닭><br>{{downValue}}<-downValue다 내려가라 ||| {{upValue}}<-upValue다 올라가라<br>\
              <input v-model="downValue">\
              <comp :downValue="downValue" @callSetter="setValue"/>\
            </div>',
          data() {
            return {
              downValue: "부모꺼",
              upValue: "부모꺼"
            }
          },
          methods: {
            setValue: function(value) {
              console.log("exec set" + value)
              this.upValue = value;
            }
          },
          components: {
            'comp': {
              template: '<div><자식이닭><br>{{downValue}}, {{upValue}}<br><input v-model="upValue"></div>',
              data() {
                return {
                  upValue: "자식꺼"
                }
              },
              props: [
                'downValue'
              ],
              created: function() {
                console.log("created comp");
                this.$emit('callSetter', this.upValue);
              }
            } // end of comp
          } // end of components of parents
        } // end of parents
      } // end of components
    })
  </script>
</body>

</html>

```


- index6.html 참조~~개힘드네~~~~
- [참조한 블로그1 : emit 값전달](https://blog-han.tistory.com/31)
- [참조한 블로그2 : 컴포넌트 v-model](https://idlecomputer.tistory.com/237) 


- 이 봐라 emit으로 분명히 부모의 메소드를 실행시키지? 그럼 저 callSetter라는 리스너는 분명히 자식꺼임 즉 부모의 메소드를 실행시킬수 있다 이걸 이용해서 값을 올린다.

- 이렇게 하고나면 이제 이 챕터의 '기본적으로 컴포넌트의 v-model은 value를 보조 변수로 사용하고 input을 이벤트로 사용하지만' 란 말이 이해가 되네 아오
- 뭐 그래서 input의 checked, change? 뭐 이런거 있는데 value땜에 충돌나잖아? 그러니깐 model이란 속성을 써라 걍 충돌날거같으면 봐라 아오 못보겠네

#### 비 부모-자식 간 통신

- 일단 이벤트란게 Vue 인스턴스/컴포넌트에서 일어나는건 알지? 여튼 컴포넌트에 등록된 이벤트는 뭐 $on으로도 등록 시킬수 있는거 같고 dom 안에 @리스너:트리거 하면 리스너가 새거 등록되는거 같네 이거두개가 똑같은 듯
- 즉 자식->부모 랑 똑같이 이 자식의 트리거를 사용해서 부모가 아닌 뭔가 인스턴스의 함수를 실행시켜야 한다 이말이지.. 근데 상태관리패턴인가 뭐 이런것도 있고 복잡해 보이네 예시도 자세히 안나와있고 검색하라

### 슬롯을 사용한 컨텐츠 배포

- 여기서도 강조하는게 컴포넌트에 들어가는 <comp v-show던v-if던v-bind던v-model이던="abc">
- 에서 abc는 절대 comp게 아니다 comp의 부모꺼(data)다.. 라는걸 강조하네
- 그리고 abc 저거 자식꺼 넣고 싶으면 template에 넣어라 라네
- 여튼 이건 기본 개념이고 이제 슬롯

#### [단일 슬롯](https://kr.vuejs.org/v2/guide/components.html#%EB%8B%A8%EC%9D%BC-%EC%8A%AC%EB%A1%AF)
- 예제 잘나와잇네.
- 컴포넌트 사이에 뭔가 컨텐츠를 넣는데 slot이 없으면 부모 컨텐츠가 삭제된단다. 또한 대체 컨텐츠도 넣을 수 있단다. 좋네..

#### 이름을 가지는 슬롯
- 예제 아주 좋다.. 헤더 푸터 구성할 때 함 써보도록


- 이제 뒤에꺼는.. 앞으로 과연 할까? 하긴 해야할거 같은데.. 모르겠다 책이나 인강보고 싶은데









