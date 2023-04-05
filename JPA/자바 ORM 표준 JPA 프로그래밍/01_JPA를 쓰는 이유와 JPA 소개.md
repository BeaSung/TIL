# JPA를 쓰는 이유와 JPA 소개
## SQL의 문제점?
1. 같은 코드의 **무한 반복**
   - sql 코드, 기본 **CRUD의 반복** 
2. **패러다임의 불일치**
   - 객체와 관계형 DB의 차이 → 객체를 DB에 넣을 때 문제 발생
3. SQL에 의존한 개발
   - column 변경이 생겼을때 영향 받는 쿼리를 모두 조사하고 수정해야한다.
   - SQL 쿼리를 직접 작성하고, 객체 엔티티가 아닌 DB 테이블을 직접적으로 변경하도록 코드를 작성하게 되어 인프라 레이어에 강한 결합이 발생한다.

### 객체 -RDB 패러다임의 불일치란?
**객체는 추상화, 상속, 다형성(클래스, 메소드)**의 특징을 가지고, **RDB는 데이터 중심**으로 이뤄져, RDB에 객체를 저장하는 데 불일치가 발생한다.

1. **상속**
- 객체랑 다르게 테이블은 상속이라는 기능이 없어서, 개발자가 여러 설정/쿼리를 생성했어야한다. <br>
  → JPA에서는 여러 쿼리를 한번에 실행해서 해결, 마치 자바 컬렉션처럼!
   
  <br>
   
2. **연관관계**
- 객체는 참조를 사용해서 연관된 객체를 조회하는 데, 테이블은 외래키로 연관관계를 설정하고 조인으로 연관 테이블을 조회한다.
   ```java
   //원래의 객체 지향 방식
   class Member {
   String id; //MEMBER_ID 컬럼 사용
   Team team; //참조로 연관관계를 맺는다. => Long teamId; 테이블처럼 바꿀 경우
   String username;//USERNAME 컬럼 사용
   Team getTeam() {
   return team;
   }
   }
   class Team {
   Long id; //TEAM_ID PK 사용
   String name; //NAME 컬럼 사용
   }
   ```

  <br>

3. **객체 그래프 탐색**
- 객체 그래프가 어디까지 탐색할 수 있는 지는 SQL을 사용해보아야 한다. <br>
  → JPA는 실제 객체를 사용하는 시점까지 DB조회를 미룬다(지연로딩). 따라서 연관된 객체를 신뢰하고 조회할 수 있다.
    ```java
    Member member = jpa.find(Member.class, memberId); //최초에 select 쿼리 발생
    
    Order oreder = memeber.getOrder();
    order.getOrderDate(); //order를 사용하는 시점에 select 쿼리 발생! -> 지연로딩
    ```

<br>

4. **비교**
- DB는 PK(기본 키)로 각 로우를 구분하는 반면, 객체는 동일성/동등성 비교를 한다.
    ```java
    String memberId = "100";
    Member member1 = memberDAO.getMember(memberId);
    Member member2 = memberDAO.getMember(memberId);
    
    member1 == member2; //다르다.
    // DB에서 같은 로우를 조회했지만, 
    // 객체 입장에서는 new로 생성된 각각 다른 인스턴스이기때문에 다른값으로 나온다.
    ```
    → JPA는 DB의 같은 로우를 조회할 경우, 1차 캐시로 동일성 비교를 보장한다.

<br>

## JPA(Java Persistence API)란?
**자바의 ORM 기술 표준**
- **Java Persistence API**, 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스

### 여기서 ORM(Object-Relational Mapping)이란?
**객체 - RDB 매핑해주는 기술**
- 객체는 객체대로 / RDB는 RDB대로 설계후 그 사이의 차이(패러다임의 차이)는 ORM이 SQL을 자동 생성하여 해결해준다.

### Hibernate란?
**JPA 구현체의 한 종류**로, (DataNucleus, EclipseLink등 다른 구현체도 존재) <br>
**JPA가 DB와 자바 객체를 매핑하기 위한 인터페이스이고 Hibernate는 이를 구현한 라이브러리이다.** (마치 인터페이스-클래스의 관계)

### Spring Data JPA란?
**JPA를 편하게 쓰기 위한 모듈** <br>
- EntityManager가 아닌, Repository를 정의하여 사용하는 더 쉬운 방법 (Repository 내부적으로는 EntityManager을 사용)

<br>

## 그래서 왜 JPA를 사용해야 하는가?
1. **생선성**
   - 기본적이고 반복적인 CRUD를 자동으로 생성
2. **유지보수**
   - 컬럼 변경 시, 관련되는 SQL을 전수조사하고 수정하는 작업을 하지 않아도 된다.
     - 객체지향적인 관점으로 엔티티를 설계할 수 있게 되어 재사용성, 가독성, 유지보수성 등의 장점이 있다.
3. **패러다임 불일치 해결**
   - 상속, 연관관계, 객체 그래프 탐색, 비교 등 패러다임의 불일치 문제를 중간에서 해결해준다.
4. **데이터 접근 레이어 추상화와 벤더 독립성**
   - 특정 데이터베이스에 종속되지 않음으로서 DBMS 변경을 유연하게 할 수 있다.


<br>
<br>

### 레퍼런스
- [김영한 - 자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard "인프런 강의")

