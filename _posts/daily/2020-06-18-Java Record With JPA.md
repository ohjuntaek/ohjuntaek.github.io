---
layout: post
title: Daily - Java Records – How to use them with Hibernate and JPA
category : daily
tag: [spring, jpa, java]
---

- [원문 링크](https://thorben-janssen.com/java-records-hibernate-jpa/)

## Records in Java

- 자바 14에서 나옴
- 간단하게 불변인 자료구조를 제공한다.
- > 코틀린의 data class 같은 것
```java
record BookWithAuthorNamesRecord(
        Long bookId, 
        String title, 
        Double price, 
        String authorNames) {}
```
- 묵시적으로 final이다.
- 자동으로 private final 필드랑 public 한 getter를 만들어 준다.
- 모든 필드에 대해 equals(), hashCode(), toString()을 구현해준다.

- 즉, read-only한 자료 구조를 만드는데 유용한다.
- 그러나 레코드를 읽기 쉽게 만들어주는 특징들은 entity에서 구현이 불가하다... 다음 섹션을 보자.

## Records can't be entites

- JPA의 엔티티는 요구사항이 좀 있는데
    - @Entity, private이 아닌 NoArgsConstruct
    - 그리고 top-level 클래스... 가 뭔 뜻이지 : inner class 면 안된다.
    - final을 안써서 프록시를 생성할 수 있다(컬렉션 같은거), id가 한개이상 있어야한다
    - 디비 컬럼과 매핑되는 건 파이널일 수 없다.
    - 속성에 접근하기 위해서 getter와 setter를 제공해야한다. => 접근 방식 바꾸면 될텐데?
- 보시듯이 record에선 안되는게 많다.
- 다만 하이버네이트는 이렇게 엄격하진 않다.
- 파이널 클래스(컬렉션 같은거)를 persist 할수 있고, 어떤 접근 메소드도 필요없다.
- 그러므로 entity로 만들수 있지만, DTO projection에 더 적합해 디비에 저장된 데이터의 읽기 전용 표현용으로 자주 쓴다.
- > 먼소리냐? 그 김영한 강의에서 domain 안에 query 패키지로 들어가는 그거 말하는 거 인듯

- > 근데 생각해보면 나는 그냥 hibernate 조건으로 써도 먹히던데? 희한하네..

## Records are great DTOs

- 뭐 dto로 프로젝션하면 당연히 좋고 엔터티를 api에 노출하지 않게 해주고 당연한 소리
- @SqlResultSetMapping 이란걸로 프로젝션 해줄수도 있는 듯
- 여튼 밑에 3가지 방법을 소개한다

- 

## Instantiating a record in JPQL

- JPQL로 new 떄리는거, 원본 링크 참조

## Instantiating a record in a CriteriaQuery

- 이걸 왜쓰냐 쿼리 dsl 쓰지, 원본 참조

## Instantiating a record from a native query

- @SqlResultSetMapping 으로 getResultList() 할때 먹히게 해준단다 원본 참조.



## 리뷰

- 별 건 없는거 같고 걍 JPA에 관한 좋은 링크하나 발견한듯.
- 찾다보니깐 https://thorben-janssen.com/youtube-channels-2020/ 가 있는데 이게 좋네