# querydsl study

## 환경
- spring Boot 2.6.0
- gradle 7.4
- queryDsl 5.0.0

## 참고
- Unable to load class 'com.mysema.codegen.model.Type'. gradle 오류 발생   
 -> gradle 5.0 이상부터는 옵션을 추가해야함 (ref. https://www.inflearn.com/questions/355723)
- Q 파일 업로드 경로 확인
  - build.gradle 확인
  - def querydslDir = "$buildDir/generated/querydsl"
- fetchResults(), fetchCount() 5.0.0 버전에서 deprecated (ref. https://devwithpug.github.io/java/querydsl-with-datajpa)
- AssertJ 필수 부분 정리 (ref. https://pjh3749.tistory.com/241)

## JPQL 장점
1. 컴파일 시 오류가 발생한다.
2. 자동으로 파라미터 바인딩 처리를 해준다.

- Query DSL 사용 시 JPQL 쿼리문 보고 싶을 때
  jpa:properties:hibernate:use_sql_comments : true 

## JPQL이 제공하는 모든 검색 조건 제공
`member.username.eq("member1") // username = 'member1'`

`member.username.ne("member1") //username != 'member1'`

`member.username.eq("member1").not() // username != 'member1'`

`member.username.isNotNull() //이름이 is not null`

`member.age.in(10, 20) // age in (10,20)`

`member.age.notIn(10, 20) // age not in (10, 20)`

`member.age.between(10,30) //between 10, 30`

`member.age.goe(30) // age >= 30`

`member.age.gt(30) // age > 30`

`member.age.loe(30) // age <= 30`

`member.age.lt(30) // age < 30`

`member.username.like("member%") //like 검색`

`member.username.contains("member") // like ‘%member%’ 검색`

`member.username.startsWith("member") //like ‘member%’ 검색`

## 결과 조회
- fetch() : 리스트 조회, 데이터 없으면 빈 리스트 반환
- fetchOne() : 단 건 조회
  - 결과가 없으면 : null
  - 결과가 둘 이상이면 : com.querydsl.core.NonUniqueResultException
- fetchFirst() : limit(1).fetchOne()
- fetchResults() : 페이징 정보 포함, total count 쿼리 추가 실행, count 쿼리가 먼저 나가고 select 처리가 됨  
-> 성능상 실무에서 count 쿼리와 select 쿼리가 분리되어야하는 경우가 존재 (복잡한 쿼리는 fetchResults 사용 X)
- fetchCount() : count 쿼리로 변경해서 count 수 조회

## 조인
- 기본 조인 : 조인의 기본 문법은 첫 번째 파라미터에 조인 대상을 지정하고, 두 번째 파라미터에 별칭(alias)으로 사용할 Q 타입을 지정하면 된다.
  - join(조인 대상, 별칭으로 사용할 Q타입)
- on절 조인
  - ON절을 활용한 조인(JPA 2.1부터 지원)
    - 조인대상필터링 
    - 연관관계없는엔티티외부조인
  - 하이버네이트 5.1부터 on 을 사용해서 서로 관계가 없는 필드로 외부 조인하는 기능이 추가되었다. 물론 내부 조인도 가능하다.

> 일반조인 : leftJoin(member.team, team)  
> on조인 : from(member).leftJoin(team).on(xxx)

> join() , innerJoin() : 내부 조인(inner join)  
> leftJoin() : left 외부 조인(left outer join)  
> rightJoin() : rigth 외부 조인(rigth outer join)  
> JPQL의 on과 성능 최적화를 위한 fetch 조인 제공 -> on 절
