# JPA 시작하기

## Entity 정의
```java
@Entity
public class Member {

    @Id
    private Long id;
    private String name;
}
```

### @Entity
- 클래스를 테이블과 매핑함을 명시
- `@Entity`로 정의된 클래스를 **엔티티 클래스**라고 한다.

### @Table
- 엔티티 클래스에 매핑할 테이블 정보를 정의
- `@Table(name="member")` : name 속성을 사용하면 테이블명을 지정할 수 있음
- 생략하면 기본적으로 엔티티 이름을 테이블 이름으로 맵핑

### @Id
- 테이블 기본 키에 매핑할 속성에 정의
- @Id가 선언된 필드를 **식별자 필드**라 함

### @Column
- 필드를 컬럼에 매핑
- name 속성을 이용하여 테이블에 매핑할 컬럼명을 정의할 수 있음
- 생략하면 기본적으로 필드명이 컬럼명으로 맵핑

<br>

# JPA 설정
## 데이터베이스 방언
- `spring.jpa.database-platform` 으로 데이터베이스 방언을 설정할 수 있다.
- JPA는 특정 DBMS에 종속적이지 않은 기술이다. 데이터베이스 방언을 설정하면 해당 DBMS에 맞는 SQL로 변환되어 DB에 요청이 보내어진다. 이 이점으로 DBMS를 교체하느 것에도 큰 어려움이 없게 된다.

## 엔티티 매니저 팩토리 & 엔티티 매니저
### 엔티티 매니저 팩토리(EntityManagerFactory)
- JPA를 사용하기 위해서는 우선 EntityManagerFactory(엔티티 매니저 팩토리)가 생성되어야한다.
- 엔티티 매니터 팩토리는 애플리케이션 전체에서 **딱 한번만 생성하고 공유되어 사용**해야한다.
- 스프링 부트 환경에서는 초기 구동시에 자동으로 생성해준다.

### 엔티티 매니저(EntityManager)
- 엔티티 매니저 팩토리에서 엔티티 매니저를 생성한다.
- 엔티티 매니저는 **데이터소스와 커넥션을 유지하고 통신**한다.
- 스레드 간 공유하거나 재사용하게 되면 예상치못한 트랜젝션 오류가 발생할 수 있다.

### 트랜젝션(Transaction)

> 트랜잭션이란?
: - DB의 상태가 변화하는 작업 단위로, 그 작업 단위는 개발자가 정함
: - 사용자가 DBMS에게 요청하는 일련의 작업들

- JPA를 사용할 때 항상 **트랜잭션 안에서 변경을 해야한다.**
- 트랜잭션 없이 데이터를 변경할 경우 예외가 발생한다.
- 트랜잭션을 시작하기 위해서는 엔티티 매니저에서 트랜잭션 API를 받아와야한다.
```java
public static void main(String[] args) {
    EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("hello");

    EntityManager entityManager = entityManagerFactory.createEntityManager();

    EntityTransaction transaction = entityManager.getTransaction();
    transaction.begin();   // 트랜잭션 시작

    try {
    Member findMember = entityManager.find(Member.class, 1L);
    findMember.setName("HelloJPA");

    transaction.commit();   // 트랜잭션 반영
    } catch (Exception e) {
    transaction.rollback();
    } finally {
    entityManager.close();
    }

    entityManagerFactory.close();
    }
```

## JPQL이란?
- **엔티티 객체**를 대상으로 쿼리 (반면 SQL은 **테이블**을 대상으로 쿼리)
- JPQL은 DB SQL에 의존하지 않고, 쉽게 말하면 객체지향 SQL이라 보면 된다.

