# querydsl study

## 환경
- spring Boot 2.6.0
- gradle 7.4
- queryDsl 5.0.0

## 참고
- Unable to load class 'com.mysema.codegen.model.Type'. gradle 오류 발생   
 -> gradle 5.0 이상부터는 옵션을 추가해야함 (cc. https://www.inflearn.com/questions/355723)
- Q 파일 업로드 경로 확인
  - build.gradle 확인
  - def querydslDir = "$buildDir/generated/querydsl"

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
- fetchResults() : 페이징 정보 포함, total count 쿼리 추가 실행
- fetchCount() : count 쿼리로 변경해서 count 수 조회