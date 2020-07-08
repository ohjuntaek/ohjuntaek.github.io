---
layout: post
title: Daily - Spring Data JPA from 0-100 in 60 minutes
categories : daily 
date: 2020-07-08
---

[원문 영상 링크](https://www.youtube.com/watch?time_continue=933&v=Zyqpo8gxSO0&feature=emb_logo)

한번 적으면서 들어보자

- using spring data jpa
- would like start story
- ![normal bike ](/assets/2020-07-08-SpringOne-Spring-Data-JPA-0-100-머시기/2020-07-08-19-30-11.png)
- BMX bike, but normal bike. take bikes apart
- 머시기 부품 버리고 뜯고... 저 뾰족 튀어나온게 뭐에 쓰는지 몰라서 버렸다는 건지 뭔지
- 어쨌든 그게 사실은 coaster brake 하는데 필요한 건데 뭐 몰랐다... 이런 애기를 하는데 왜하냐
- 스프링 데이터 jpa 쓰는 넘들 스택오버플로우 보니깐 내가 받은 인상은 지들이 뭐하는지 모른다는 거다.
- 그래서 기초부터 하겠다..
- has tricky questions 
- ![person](/assets/2020-07-08-SpringOne-Spring-Data-JPA-0-100-머시기/2020-07-08-19-43-57.png)
- @Version : Optimistic locking
- ![](/assets/2020-07-08-SpringOne-Spring-Data-JPA-0-100-머시기/2020-07-08-createPerson.png)
- 뭐라는거야... 아니 주석 친곳에서 디비에 select count(*) 날리면 항상 1이 나올거냐? 라고 묻는다.
- > 아... 헷갈리네 플러쉬할때 1 나올텐데 저 jpql 쏘면 플러쉬 날리니깐 3번째에서만 1이 나올거 같은데?
- 맞네... 뭐 delayed writing 땜에 그렇단다. 최대한 늦게 디비에 쏜다. 1차 캐시에 id 조회하니깐 이미 있는 넘이네? 하면 안가져온다~ 
- 근데 마지막엔 왜 가져오냐? 저 jpql 보내는거는 jpa는 아무것도 모른다 그냥 jpql 번역해서 db에 쏘니깐 당연히 하기전에 플러쉬 나간단다.
- ![](/assets/2020-07-08-SpringOne-Spring-Data-JPA-0-100-머시기/2020-07-08-19-59-52.png)
- setName 쓰면 과연 Person이 바뀔까? 당연히 바뀌지 트랜잭션 끝나면~

- ![](/assets/2020-07-08-SpringOne-Spring-Data-JPA-0-100-머시기/2020-07-08-20-04-47.png)
- ![](/assets/2020-07-08-SpringOne-Spring-Data-JPA-0-100-머시기/2020-07-08-20-54-25.png)
- 자 위에 보면 hobby가 매니투매니로 걸려있지? address는 원투원으로... 이거 위처럼 읽으면 어떻게 될까?
- 레이지 로딩 익셉션이 발생한다! 왜? 모르겠는데 이거는... 하비랑 어드레스 cascade로 조회할 때 안가져오나??
- 아하 이게 트랜잭션 밖이였지 근데 매니투매니는 기본적으로 레이지니깐 발생하겠네.
- 그럼 위에 getAddress()는? 레이지 로딩 익셉션이 언제 발생하냐 그 객체 가져온다고 발생하진 않는다 프록시 걸려있으니깐... 사용할 때 발생하지
- 아니 근데 cascade 쓰면... 아오 cascade 또 잘못 생각했네 이거는 같이 저장하거나 같이 삭제하는거지 지연 로딩, 즉시 로딩이랑은 관련이 없다 이제 이말이 느낌이 오네

What does Spring Data JPA do for you?

### 총평

- 다 아는건데도 불구 알아듣는거 너무 오래걸림... 영어 공부 좀 하자
- 걍 들어도 귀에 쏙쏙 들어올 그날을 위해