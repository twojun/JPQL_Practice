# JPQL Basic, Advanced</br>

(1) 내용 수정 필요 (last modified date : 24.04.19)</br></br></br></br>




# 객체지향 쿼리 언어 I</br>
## 1. 소개</br>
(1) JPA는 쿼리를 다양하게 작성할 수 있는 방법을 지원한다.</br>
- JPQL(Java Persistence Query Language) : 엔티티 객체 자체를 대상으로 쿼리를 날릴 수 있는 객체지향 쿼리 언어를 말한다.
- JPA Criteria : 자바 코드로 쿼리를 짜서 JPQL를 빌드해주는 제너레이터 클래스의 모음 </br>
- QueryDSL : 자바 기반의 타입 세이프한 도메인 특화 라이브러리로 실제 자바 코드로 쿼리를 쉽게 작성할 수 있게 해주며 런타임 시점에 타입 체크를 수행해 오류를 방지하는 장점이 있다. </br>
- Native SQL : JPA를 쓰더라도 DB에 종속적인 제한된 쿼리가 필요할 수도 있다. 그러한 상황에서 사용한다.</br>
- JDBC Template api를 직접 사용하는 것(SpringJdbcTemplate와 함께 사용된다.)</br></br></br></br></br>



## 2. JPQL</br>
(1) JPA는 일반 SQL을 표준화한 데이터베이스 테이블에 독립적인 엔티티 객체를 중심으로 개발할 수 있는 객체지향 쿼리 언어를 제공하는데 이를 JPQL이라고 한다.</br></br>
(2) 조회 시에도 테이블이 아닌 객체를 대상으로 검색한다 (수정필요).</br></br></br></br></br>



## 3. JPA Criteria</br>
(1) 문자가 아닌 실제 자바 코드로 JPQL을 빌드해주는 제너테이터 라이브러리</br>
(2) JPA 공식 스펙이지만 명확한 단점으로는 너무 복잡하고 실용성이 없다.</br>
(3) JPA Criteria 대신에 QueryDSL 사용이 권장된다.</br></br></br></br></br>



## 4. QueryDSL</br>
(1) jpa criteria와 유사하게 자바 기반의 타입 세이프한 도메인 특화 라이브러리로 자바 코드로 JPQL을 작성할 수 있다.</br>
(2) 자바 코드로 작성되기 때문에 오류를 잡을 수 있다.</br>
(3) 동적 쿼리 작성이 수월하고 직관적이다.</br>
(4) 확실히 사용하기 위해 JPQL에 대한 학습이 선행되어야 한다. </br>
(5) 실무에서 사용이 권장된다.</br></br></br></br></br></br></br>





# Chapter 1. JPQL 기본 문법과 쿼리 API</br>
(1) 정의 : JPQL은 객체지향 쿼리 언어다. 따라서 데이터베이스 테이블을 대상으로 쿼리를 작성하는 것이 아닌
엔티티 객체를 대상으로 쿼리를 작성한다.</br></br>
(2) JPQL은 SQL 자체를 추상화해서 특정 데이터베이스에 독립적으로 설계되어 있다.</br></br>
(3) JPQL은 마지막에 설정된 Database dialect에 맞춘 SQL로 변환된다.</br></br></br></br></br>




## 1-1. JPQL 문법</br>
(1) JPQL 문법은 일반적인 SQL 문법과 유사하다.</br></br>
(2) select, from, where, groupby, having, orderby join ... update, delete(벌크 연산, 수많은 반복적인 작업 수행 시 수행하는 연산) </br></br>
(3) 엔티티와 속성은 대소문자를 구분한다. (Member, age)</br></br>
(4) JPQL 키워드 자체는 대소문자 구분이 없다.(select, FROM, where...) </br></br>
(5) 엔티티 자체의 이름을 사용한다.</br></br>
(6) 별칭(alias)는 필수적으로 적어주어야 하며 as는 생략 가능하다.</br></br></br></br></br>




## 1-2 JPQL 문법 - 집합과 정렬 </br>
(1) 카운트 count()</br>
(2) 합 sum()</br>
(3) 평균 avg()</br>
(4) 최댓값 max() </br>
(5) 최솟값 min() </br>
(6) group by, having </br>
(7) order by</br></br>

- 기본적으로 ANSI SQL에서 제공하는 표준 함수들이 제공된다. </br></br></br></br></br>





## 1-3, TypeQuery, Query</br>
(1) TypeQuery : 반환 타입이 명확할 때 사용</br></br>
(2) Query : 반환 타입이 명확하지 않을 때 사용</br></br></br></br></br>





## 1-4. 결과 조회 API</br>
(1) getResultList() : 조회 결과가 하나 이상일 때 사용, 리스트 형태로 반환</br>
만약 결과가 없다면 빈 리스트를 반환한다.</br></br>

(2) getSingleList() : 조회 결과가 정확히 하나일 때 사용(값이 존재한다고 보장될 때), 단일 객체를 반환</br>
- 만약 결과가 없으면 javax.persistence.NoResultException 예외 발생 </br>
- 결과가 2개 이상이면 javax.persistence.NonUniqueResultException 예외 발생 </br></br></br></br></br>




## 1-5. 파라미터 바인딩 : 이름 기준과 위치 기준</br>
(1) 이름 기준</br>
select m from Member m where m.username = :username</br>
-> query.setParameter("username", usernameParam);</br></br>

(2) 위치 기준 : 위치 기반은 중간에 순서가 꼬이게 되면 큰 장애가 발생할 수 있어서 권장되지 않는다.</br>
select m from Member m where m.username = ?1</br>
-> query.setParameter(1, usernameParam);</br></br></br></br></br></br></br></br>






# Chapter 2. 프로젝션(SELECT)</br>
(1) 프로젝션이란, select절에 조회할 대상을 지정하는 것을 말한다.</br></br>
(2) 데이터베이스는 컬럼명만 지정하면 되지만 JPQL은 프로젝션 대상이 엔티티, 임베디드 타입, 스칼라 타입(숫자, 문자 기본 타입)등 다양한 데이터 타입이 존재한다.</br>
- select m from Member m : entity projection</br>
- select m.team from Member m : entity projection </br>
- select m.address from Member m : embedded type projection </br>
- select m.username, m.age from Member m : scala type projection</br>
- distinct로 중복 제거가 가능하다. </br></br>

(3) 엔티티 프로젝션인 경우 select절에 포함된 모든 대상이 영속성 컨텍스트 내부에 관리된다.</br></br></br></br></br>




## 2-1. 프로젝션 - 여러 값 조회하기 </br>
(1) 가장 많이 사용되는 방법 : new 연산자로 조회하기 </br>
- 단순 값을 DTO로 바로 조회 : select new jpabook.jpql.UserDTO(m.username, m.age) from Member m</br>
- 패키지명을 포함한 전체 클래스명 입력 </br>
- 순서와 타입이 일치하는 생성자 별도 필요 </br>
- 클래스 경로가 길어질 수 있는데 이후에 QueryDSL로 해결 가능하다.</br></br></br></br></br></br></br>







# Chapter 3. 페이징과 조인  </br>
## 3-1. 페이징 api </br>
(1) JPA는 페이징을 다음 두 API로 추상화했다. </br>
- setFirstResult(int startPosition) : 조회 시작 위치 (0부터 시작) </br>
- setMaxResults(int maxResult) : 조회할 데이터 수  </br> </br> </br> </br></br>




## 3-2. 조인 </br>
(1) 내부 조인</br>
- select m from Member m (inner) join m.team t</br></br>

(2) 외부 조인</br>
- select m from Member m left (outer) join m.team t</br></br>

(3) 세타 조인 </br>
- select count(m) from Member m, Team t where m.username = t.name </br></br></br></br></br>





## 3-3. 조인 : on 활용</br>
(1) on절을 활용한 조인이 가능하다.(jpa 2.1부터 지원)</br>
(2) 조인 대상 필터링</br>
(3) 연관관계가 없는 엔티티의 외부 조인이 가능하다.(Hibernate 5.1 이상부터 지원)</br></br></br></br></br>





## 3-4. 조인 대상 필터링 </br>
(1) 회원과 팀을 조인하면서, 팀 이름이 A인 팀만 조인 </br>
- JPQL : </br>
- select m, t from Member m left join m.team t on t.name = 'A'</br>

- SQL</br>
- select m.*, t.* </br>
    from Member m</br>
    left join Team t on m.TEAM_ID = t.id and t.name = 'A'</br></br></br></br></br>




## 3-5. 연관관계가 없는 엔티티 외부 조인 </br>
(1) 회원의 이름과 팀의 이름이 같은 대상 조인
- JPQL : </br>
- select m, t from Member m left join Team t on m.username = t.name</br>

- SQL</br>
- select m.*, t.* </br>
    from Member m</br>
    left join Team t on m.username = t.name </br></br></br></br></br></br></br>








# Chapter 4. 서브 쿼리 </br>
## 4-1. 나이가 평균보다 많은 회원 조회  </br>
select m from Member m </br>
where m.age > (select avg(m2.age) from MEmber m2) </br> </br> </br></br></br>




## 4-2. 한 건이라도 주문한 고객 </br>
select m from Member m </br>
where (select count(o) from Order o where m = o.member) > 0  </br> </br> </br> </br></br>




## 4-3. 서브 쿼리에서 지원하는 함수 </br>
(1) 서브 쿼리에 결과가 존재하면 참 : exists (subquery) </br> </br>
(2) all (subquery) :  조건을 모두 만족하면 참 </br> </br>
(3) any, some : 같은 의미, 조건을 하나라도 만족할 시 참  </br> </br> 
(4) in (subquery) : 서브 퀴리의 결과 중 하나라도 같은 것이 존재하면 참  </br> </br> </br>

1번 예시 : 팀 A 소속인 회원 </br>
- select m from Member m </br>
  where exists (select t from m.team t where t.name = 'A') </br> </br>

2, 3번 예시 : 전체 상품의 각각의 재고보다 주문량이 많은 주문들 </br>
- select o from Order o </br>
  where o.orderAmount > all (select p.stockAmount from Product p) </br> </br>

2, 3번 예시 : 어떤 팀이든 팀에 소속된 회원 </br>  
- select m from Member m </br>
  where m.team any (select t from Team t) </br></br></br></br></br>





## 4-4. (중요) JPA 서브 쿼리의 한계 </br>
(1) JPA 표준 스펙에서는 where, having에서만 서브 쿼리를 쓸 수 있다.</br>
(2) 하이버네이트에서는 select절에서도 사용할 수 있다.</br></br>

(3) 중요</br>
- from절의 서브쿼리는 JPQL에서 불가능하다. 이러한 경우 JOIN으로 풀어서 해결한다. </br></br></br></br></br></br></br>







# Chapter 5. JPQL 타입 표현식과 기타식, 조건식(CASE), JPQL 함수</br>
## 5-1. JPQL의 타입 표현 </br>
(1) 문자 : 'hello', 싱글 쿼테이션 표현 시 ('')</br>
(2) 숫자 : 10L(Long), 10D(Double), 10F(Float)</br>
(3) boolean : TRUE, FALSE</br>
(4) enum : (패키지명 포함) jpabook.MemberType.Admin</br>
(5) 엔티티 타입 : TYPE(m) = Member (상속 관계에서 사용) </br></br></br></br></br>




## 5-2. JPQL 기타</br>
(1) exists, in </br>
(2) and, or, not </br>
(3) =, >, >= <. <= 등 대소비교 표현식 </br>
(4) between, like, is null, ... </br></br></br></br></br>




## 5-3. 조건식(CASE)</br>
(1) 기본 case, 단순 case 식</br>

(2) coalesce : 하나씩 조회해서 null이 아니라면 반환한다.</br>
사용자 이름이 없으면 이름 없는 회원을 반환</br>
- select coalesce(m.username, '이름 없는 회원') from Member m</br></br>

(3) nullif : 두 값이 같으면 Null, 다르면 첫 번째 값 반환 </br>
사용자 이름이 '관리자'면 null을 반환하고 나머지 본인의 이름을 반환</br>
- select nullif(m.username, '관리자') from Member m </br></br></br></br></br>




## 5-4. JPQL 기본 함수 : 데이터베이스와 관계없이 JPA에서 기본적으로 제공하는 함수들</br>
(1) concat : 문자열 더하기 </br>
(2) substring : 문자열 자르기(범위를 포함해서)</br>
(3) trim : 공백 제거 </br>
(4) lower, upper : 대/소문자로 변환</br>
(5) length : 문자의 길이 </br>
(6) locate </br>
(7) abs, sqrt, mod </br>
(8) size, index(JPA 용도) </br></br></br></br></br>




## 5-5. JPQL 사용자 정의 함수 </br>
(1) 하이버네이트에선 사용자 정의 함수를 사용하기 전에 Dialect에 추가해야 한다. </br> </br>
(2) 사용하는 데이터베이스 dialect를 상속받고, 사용자 정의 함수를 등록해야 한다. </br></br></br></br></br></br></br>








# Chapter 6. 경로 표현식</br>
(1) 경로 표현식이란 .(dot)을 찍어서 객체 그래프를 탐색하는 것을 의미한다. </br></br></br>



## 6-1. 경로 표현식 용어 정리</br>
(1) 상태 필드(state field)</br>
- 단순히 값을 저장하기 위한 필드 : m.username ( select m.username ... _) </br> </br>

(2) 연관 필드(association field)</br>
- 연관관계를 위한 필드</br>
- 단일 값 연관 필드 : @ManyToOne, @OneToOne (타겟 대상이 엔티티인 경우)</br>
- 컬렉션 값 연관 필드 : @OneToMany, @ManyToMany (타겟 대상이 컬렉션인 경우) </br></br></br></br></br>



## 6-2. 경로 표현식 특징(중요한 점) </br>
(1) 상태 필드 : 경로 탐색의 끝으로써 더 이상 탐색이 불가능하다.</br>
(2) 단일 값 연관 경로 : 묵시적 내부 조인이 발생한다. 이 상태에서는 더 탐색할 수 있다.</br></br>
(3) 컬렉션 값 연관 경로 : 묵시적 내부 조인이 발생한다. 더 이상 탐색 불가능하다.</br>
- from절에서 명시적 조인을 통해 별칭을 얻으면 별칭을 통해 탐색할 수 있다.</br></br>

(4) 핵심 : 묵시적 조인은 권장되지 않는다. 명시적 조인을 사용해야 이후 튜닝하기에 직관적이고 수월하며
묵시적 조인을 사용하면 이후 실무에서 운영단계에서는 큰 이슈나 운영상 어려움이 있을 수 있다.</br></br></br></br></br>




## 6-3. 명시적 조인, 묵시적 조인  </br>
(1) 명시적 조인 : join 키워드를 직접 사용해서 join을 명시하는 것 </br> 
select m from Member m join m.team t </br> </br>

(2) 묵시적 조인 : 경로 표현식에 의해 묵시적으로 SQL join이 발생하는 것  </br>
select m.team from Member m </br> </br> </br> </br></br>




## 6-4. 실무에서는? </br>
(1) 가급적이면 묵시적 조인 대신 명시적 조인을 사용한다. </br>
(2) 조인은 SQL 성능튜닝에 중요한 포인트이다. </br>
(3) 묵시적 조인 사용 시 조인이 일어나는 상황을 한 번에 파악하기 어렵다는 단점이 있다. </br> </br> </br> </br></br></br></br>








# Chapter 7. JPQL Fetch join (실무에서 중요)  </br>
## 7-1. fetch join? </br>
(1) 표준 SQL 문법이 아니며 JPA에서 성능 최적화를 위해 제공하는 join 기능으로 Fetch join은 JPA에서 연관된 엔티티를 함께 로딩하는 기능을 말한다. 
기본적으로 JPA는 연관된 엔티티를 지연 로딩(Lazy Loading)으로 설정하여, 해당 연관 엔티티가 실제로 사용될 때 로딩되도록 하고 있지만  한 번의 쿼리로 모든 연관 엔티티를 함께 로딩하고 싶을 때가 있는데
이때 Fetch join을 사용한다.</br> </br>
(2) 연관된 엔티티나 컬렉선에 대해 SQL 한 번에 함께 조회하는 기능 </br> </br>
(3) join fetch 키워드 사용  </br> </br>
(4) 문법 : [left [outer] | inner] join fetch ...(조인 경로)</br></br></br></br></br>




## 7-2. Entity fetch join</br>
(1) 회원을 조회하면서 연관된 팀도 함께 조회하는 경우</br>
(JPQL)</br>
select m from Member m join fetch m.team</br></br>

(SQL)</br>
select M.*, T.* from member m
join team T on M.team_id = T.id; </br></br></br></br></br>




## 7-3. 컬렉션 fetch join </br>
(1) 일대다, 컬렉션 페치 조인의 경우 (다대일은 문제되지 않음.)</br>
- 데이터베이스에 실제 쿼리를 날리게 되면 실제 나오는 결과 로우가 모두 다르다.
  jpa 입장에서는 DB에서 결과가 나온 로우 수만큼 컬렉션 개수를 돌려줘야 하도록 설계되어 있다.</br></br>


(2) 중복이 싫다면? : fetch join과 distinct</br>
- JPQL의 distinct는 2가지 기능을 제공</br>
- SQL에 distinct를 추가 </br>
- 애플리케이션 레벨에서 엔티티 중복을 제거시킨다.</br></br></br></br></br>




## 7-4. Fetch join, 일반 Join의 차이 </br>
(1) 일반 join의 경우 연관된 엔티티를 함께 조회하지 않는다.</br></br>
(2) JPQL은 결과를 반환할 때 연관관계를 고려하지 않는다.</br></br>
(3) fetch join을 사용할 때만 연관된 엔티티도 함께 조회한다(즉시로딩) </br></br>
(4) fetch join은 객체 그래프를 sql을 통해 한 번에 조회하는 개념이다.</br></br></br></br></br>




## 7-5. fetch join의 특징과 한계점</br>
(1) fetch join 대상에는 별칭을 가급적 사용하지 않는다. </br>
- JPA에서 원칙상 의도한 설계는 fetch join을 사용해서 객체 그래프라는 것을 통해 기본적으로 데이터를 모두 끌어와야 한다. 일부를 걸러내면서 조회하는 것은 JPA에서 의도한 설계와 맞지 않다.</br>
- 데이터의 정합성 측면, 객체 그래프의 사상이 깨지기 때문에 쓰지 않는다. </br>
- 일부를 걸러내면서 조회하기 위해서는 처음부터 일부를 걸러내는 쿼리를 따로 작성하는 방식으로 풀어내야 한다. </br>
- 연관관계를 찾아간다는 것은(객체 그래프를 탐색한다는 것은) JPA 설계 컨셉상 연관된 엔티티가 모두 나와야 한다는 것을 기본으로 하고 있다.</br>
- 따라서 별칭을 사용하는 것은 하이버네이트에서는 가능하나 가급적이면 사용하지 않는다.</br></br></br>

(2) 둘 이상의 컬렉션은 fetch join 할 수 없다.</br></br></br>

(3) 컬렉션을 fetch join 하면 페이징 API(setFirstResult(), setMaxResults())를 사용할 수 없다.</br>
- 일대다 컬렉션 fetch join시, 로우 수에 따라서 컬렉션 수가 그만큼 늘어난다. 여기서 페이징을 사용해서 개수 제한을 두게 되면
  모든 로우수가 나오는 것이 아닌 페이징 처리한 개수만큼 나올 수 있기 때문에 실제 데이터 개수와 맞지 않게 조회 결과가 나올 수 있기 때문 </br>
- 다대일, 일대일 같은 단일 연관 값 필드들은 fetch join을 해도 페이징 처리가 가능함.</br>
- (중요) 일대다 컬렉션의 경우 fetch join하고 페이징 처리 시 메모리에서 페이징 처리를 하기 때문에 장애로 이어질 수 있다 </br></br></br></br></br>




## 7-6. @BatchSize</br>
(1) 일대다 컬렉션에서는 @BatchSize를 사용할 수 있다. </br></br>
(2) persistence.xml 파일 등에서 Global setting으로 batch size를 정할 수 있다.  </br></br></br></br></br>




## 7-7. fetch join의 특징과 한계 정리</br>
(1) 연관된 엔티티들을 모두 SQL 한 번으로 끌고오는 것을 말하고 성능 최적화에서 사용되는 방법이다.</br></br>
(2) 엔티티에 직접적으로 적용하는 글로벌 로딩 전략보다 우선순위가 높다.</br>
(@ManyToOne(fetch = FetchType.LAZY) 등의 어노테이션을 글로벌 로딩 전략이라고 함)</br></br>
(3) 실무에서 글로벌 로딩 전략은 모두 LAZY loading으로 설정한다.</br></br>
(4) 최적화가 필요한 영역, N+1 문제가 터지는 부분에만 Fetch join을 설정한다.</br></br>
(5) 모든 문제를 fetch join으로 해결할 수는 없다.</br></br>
(6) fetch join은 객체 그래프를 유지할 때 사용하면 효과적이다.</br></br>
(7) 여러 테이블을 join해서 엔티티의 형태가 아닌 전혀 다른 결과를 내야 하는 상황에서는 fetch join 보다는 일반적인 join을 사용하고
필요한 데이터들만 조회해서 DTO로 변환하는 것이 효과적이다.</br></br> </br> </br> </br> </br></br>







# Chapter 8. 다형성 쿼리, 엔티티 직접 사용하기 </br>
## 8-1. TYPE - 자식 엔티티 조회 </br>
(1) 조회 대상을 자식으로 한정 </br>
(jpql) select i from Item i where type(i) in (Book, Movie)</br>
(SQL) select i from i where i.DTYPE in('B', 'M')</br></br></br></br></br>




## 8-2. TREAT(JPA 2.1)</br>
(1) 자바의 타입 캐스팅과 유사</br>
(2) 상속 구조에서 부모의 타입을 특정 자식 타입으로 다룰 때 사용한다.</br>
(3) from , where, select(하이버네이터에서 지원) 사용 가능 </br></br>

예시) 부모인 Item과 자식 Book 엔티티가 있다.</br>
(jpql) select i from Item i where treat(i as Book).auther = 'kim'</br>
(SQL) select i.* from i where i.DTYPE = 'B' and i.auther = 'kim'</br></br></br></br></br>




## 8-3. 엔티티 직접 사용하기 </br>
(1) 엔티티 직접 사용하기 - 기본 키 값 </br>
JPQL에서 엔티티를 직접 사용하면 SQL에서 해당 엔티티의 기본 키 값을 사용</br>
(jpql) select count(m.id) from Member m  // 엔티티의 아이디를 사용 </br>
       select count(m) from Member m     // 엔티티를 직접 사용 </br></br>

(sql) -> jpql 둘 다 같은 쿼리가 실행된다.</br>
       select count(m.id) as cnt from Member m </br></br>

(2) 엔티티를 파라미터로 전달 </br>
> select m from Member m where m = :member</br></br>

(3) 식별자를 직접 전달 </br>
> select m from Member m where m.id = :memberId</br></br>

> 공통으로 실행된 SQL : select m.* from Member as m where m.id = ?</br></br></br></br></br>




## 8-4. 엔티티 직접 사용하기 - 외래 키 값</br>
(1) select m from Member m where m.team = :team </br>
(2) select m from Member m where m.team.id = :teamId </br>
(3) 실행된 SQL : select m.* from Member m where m.team_id = ? </br></br></br></br></br></br></br>






# Chapter 9. Named query, 벌크 연산 </br>
## 9-1. Named query?</br>
(1) 특정 쿼리를 미리 정의해서 이름을 부여해두고 사용하는 JPQL을 Named query라고 한다.</br></br>
(2) 정적 쿼리 형태 </br></br>
(3) 어노테이션, XML 영역에서 정의할 수 있다.</br></br>
(4) 애플리케이션 로딩 시점에 초기화 후 재사용 가능하다.</br>
(5) (장점) 애플리케이션 로딩 시점에 쿼리를 검증(Validation)할 수 있다.</br></br></br></br></br>



## 9-2. 벌크 연산</br>
(1) 벌크 연산이란 특정 PK를 찍어 수정 및 삭제하는 작업이 아닌 한 번에 여러 데이터를 수정하고 삭제하는 연산을 말한다.</br></br>
(2) 벌크 연산도 JPQL을 사용하는 것이기 때문에 연산 후 자동으로 영속성 컨텍스트가 플러시(flush는 커밋 시점, 쿼리를 날리거나, 강제 플러시 호출 시 flush가 진행됨)된다. </br></br>
(3) 벌크 연산 없이 JPA 변경 감지를 통해 여러 데이터를 한 번에 변경하려면 너무 많은 SQL이 실행된다는 단점이 있다.</br></br></br></br></br>


## 9-3. 벌크 연산 예제</br>
(1) 쿼리 한 번으로 여러 테이블의 로우를 변경하고자 한다(엔티티). </br></br>
(2) executeUpdate() 메서드를 사용하고 해당 메서드의 결과는 영향 받은 엔티티 수를 반환하게 된다.</br></br>
(3) UPDATE, DELETE 연산을 지원한다.</br></br>
(4) INSERT(insert into, select, 하이버네이트에선 지원) 사용 가능, JPA 표준 스펙 상으로는 없음 </br></br></br></br></br>


## 9-4. 벌크 연산 주의사항 </br>
(1) 벌크 연산은 영속성 컨텍스트를 무시하고 데이터베이스에 바로 쿼리를 날리게 된다.</br></br>
(2) 해결방법</br>
2-a 영속성 컨텍스트에 아무런 작업 없이, 벌크 연산을 먼저 실행한다 </br>
2-b 영속성 컨텍스트에 엔티티가 존재하는 상황이라면, 벌크 연산 수행 이후 영속성 컨텍스트를 초기화시킨다.

