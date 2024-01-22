# JPQL Basic, Advanced</br></br>


# Chapter 1. JPQL 기본 문법과 쿼리 API</br>
(1) 정의 : JPQL은 객체지향 쿼리 언어다. 따라서 데이터베이스 테이블을 대상으로 쿼리를 작성하는 것이 아닌
엔티티 객체를 대상으로 쿼리를 작성한다.</br></br>
(2) JPQL은 SQL 자체를 추상화해서 특정 데이터베이스에 독립적으로 설계되어 있다.</br></br>
(3) JPQL은 마지막에 설정된 Database dialect에 맞춘 SQL로 변환된다.</br></br></br></br>



1-1. JPQL 문법</br>
(1) JPQL 문법은 일반적인 SQL 문법과 유사하다.</br></br>
(2) select, from, where, groupby, having, orderby join ... update, delete(벌크 연산, 수많은 반복적인 작업 수행 시 수행하는 연산) </br></br>
(3) 엔티티와 속성은 대소문자를 구분한다. (Member, age)</br></br>
(4) JPQL 키워드 자체는 대소문자 구분이 없다.(select, FROM, where...) </br></br>
(5) 엔티티 자체의 이름을 사용한다.</br></br>
(6) 별칭(alias)는 필수적으로 적어주어야 하며 as는 생략 가능하다.</br></br></br></br>



1-2 JPQL 문법 - 집합과 정렬 </br>
(1) 카운트 count()</br>
(2) 합 sum()</br>
(3) 평균 avg()</br>
(4) 최댓값 max() </br>
(5) 최솟값 min() </br>
(6) group by, having </br>
(7) order by</br></br>

- 기본적으로 ANSI SQL에서 제공하는 표준 함수들이 제공된다. </br></br></br></br>



1-3, TypeQuery, Query</br>
(1) TypeQuery : 반환 타입이 명확할 때 사용</br></br>
(2) Query : 반환 타입이 명확하지 않을 때 사용</br></br></br></br>



1-4. 결과 조회 API</br>
(1) getResultList() : 조회 결과가 하나 이상일 때 사용, 리스트 형태로 반환</br>
만약 결과가 없다면 빈 리스트를 반환한다.</br></br>

(2) getSingleList() : 조회 결과가 정확히 하나일 때 사용(값이 존재한다고 보장될 때), 단일 객체를 반환</br>
- 만약 결과가 없으면 javax.persistence.NoResultException 예외 발생 </br>
- 결과가 2개 이상이면 javax.persistence.NonUniqueResultException 예외 발생 </br></br></br></br>



1-5. 파라미터 바인딩 : 이름 기준과 위치 기준</br>
(1) 이름 기준</br>
select m from Member m where m.username = :username</br>
-> query.setParameter("username", usernameParam);</br></br>

(2) 위치 기준 : 위치 기반은 중간에 순서가 꼬이게 되면 큰 장애가 발생할 수 있어서 권장되지 않는다.</br>
select m from Member m where m.username = ?1</br>
-> query.setParameter(1, usernameParam);</br></br></br></br></br>






# Chapter 2. 프로젝션(SELECT)</br>
(1) 프로젝션이란, select절에 조회할 대상을 지정하는 것을 말한다.</br></br>
(2) 데이터베이스는 컬럼명만 지정하면 되지만 JPQL은 프로젝션 대상이 엔티티, 임베디드 타입, 스칼라 타입(숫자, 문자 기본 타입)등 다양한 데이터 타입이 존재한다.</br>
- select m from Member m : entity projection</br>
- select m.team from Member m : entity projection </br>
- select m.address from Member m : embedded type projection </br>
- select m.username, m.age from Member m : scala type projection</br>
- distinct로 중복 제거가 가능하다. </br></br>

(3) 엔티티 프로젝션인 경우 select절에 포함된 모든 대상이 영속성 컨텍스트 내부에 관리된다.</br></br>
(4)

