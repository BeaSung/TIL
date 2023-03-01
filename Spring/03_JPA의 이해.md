## 1. JPA란?

JPA 는 **Java Persistence(영속성) API**이다.

여기서 Persistence란, 사전적 의미로 "영속성"을 의미하며, 데이터가  (없어지지 않고 오랜동안) 지속됨을 의미한다.

- 램은 휘발성 데이터를 저장 - 컴퓨터가 꺼지면 사라짐 
- 데이터가 날라가지 않도록 -> 하드디스크에 저장하면 안 사라짐 
- -> 이게 퍼시스턴스(영구히 기록)

<br>

`CRUD`: Create Read Update Delete

**Create**는 **Client > Server > DB**로 전달되는 과정이 있다. DB란 데이터를 정리하는 창고이다.

DB에 데이터를 등록하려면 Server가 DB에 관리를 요청하면되는데, Server는 Java, DB는 SQL을 사용하고, DB는 자바를 이해하지 못한다는 문제점이있다.

**이를위한 도구가 JPA이다.**

<br>

JPA는  DB관리에 편리한 여러도구까지 제공한다.

JPA의 핵심 도구는 `Entity`와 `Repository`가 있다.

<br>


**`Entity`는 자바객체를 DB가 이해할수있게 잘 규격화된 데이터이다.**

**잘규격화된 `Entity`는 `Repository`라는 일꾼을 통해서 잘 전달되고 DB에게 전달되고 처리된다.**

<br>

### 다시 JPA란,
Java Persitance API 는 **"자바에서 데이터를 영구히 기록할 수 있는(DBMS에) 환경을 제공하는 API"** 라고 말할 수 있다.

<br>

## 2. ORM이란?
ORM(Object Relational Mapping)이란, **객체를(Object)와 데이터베이스의 데이터를 연결해주는(매핑) 기술이다.**

DB 데이터와 JAVA에서의 데이터 타입이 다르기 때문에, DB의 데이터를 JAVA에서 사용하기 위해서, Object를 만들어야하는데, 이를 모델링이라고 한다.

DB에서 데이터를 받기위해, JAVA에서 코드, 즉 모델링을 통해 객체에 데이터를 받아올 수 있다.

반대로 자바에서 객체 코드를 이용해서 DB 데이터 테이블을 생성할 수 있는데, 이를 자동으로 해주는게 ORM이다.

ORM 없이 DataBase의 데이터를 조작하기 위해서는 위와 같은 일련의 과정(CRUD)을 통해야 하는데, 그 과정을 ORM에서 자동으로 제공해준다.

<br>

## 3. JPA의 구조
JPA는 특정기능을 제공하는 라이브러리가 아닌, `인터페이스`이다.

그저 인터페이스이기 때문에 `Hibernate`, `OpenJPA` 등이 JPA를 구현한다.

**하이버네이트 ORM(Hibernate ORM)** 은, 자바 언어를 위한 객체 관계 매핑 프레임워크로, 객체 지향 도메인 모델을 관계형 데이터베이스로 매핑하기 위한 프레임워크를 제공한다.

JPA는 DB와 자바 객체를 매핑하기 위한 인터페이스(API)를 제공하고 JPA 구현체(하이버네이트)는 이 인터페이스를 구현한 것이다.

<br>

## 4. JPA 사용법 (JpaRepository)

### Entity
먼저 데이터베이스에 저장하기 위해 유저가 정의한 클래스가 필요한데 그런 클래스를 `Entity`라고 한다. **Domain**이라고 생각하면 된다.

일반적으로 RDBMS에서 Table을 객체화 시킨 것으로 보면 된다.
그래서 Table의 이름이나 컬럼들에 대한 정보를 가진다.

````java
package com.example.firstproject.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
public class Article {
    
    @Id
    @GeneratedValue
    private Long id;
    
    @Column
    private String title;
    
    @Column
    private String content;
    public Article(Long id, String title, String content) {
        this.id = id;
        this.title = title;
        this.content = content;
    }
    
    @Override
    public String toString() {
        return "Article{" +
                "id=" + id +
                ", title='" + title + '\'' +
                ", content='" + content + '\'' +
                '}';
    }
}
````

#### @ 어노테이션


- **@Entity**
  - 클래스 위에 선언하여 이 클래스가 엔티티임을 알려준다. 이렇게 되면 JPA에서 정의된 필드들을 바탕으로 데이터베이스에 테이블을 만들어준다.

- **@Id, @GeneratedValue**
  - primary key를 가지는 변수를 선언하는 것을 뜻한다. 
  - @GeneratedValue는 이 PK가 자동으로 1씩 증가하는 형태로 생성될지 등을 결정해주는 어노테이션이다. 

- **@Table**
  - 별도의 이름을 가진 데이터베이스 테이블과 매핑한다. 
  - 기본적으로 @Entity로 선언된 클래스의 이름은 실제 데이터베이스의 테이블 명과 일치하는 것을 매핑한다. 
  - 따라서 @Entity의 클래스명과 데이터베이스의 테이블명이 다를 경우에 @Table(name=" ")과 같은 형식을 사용해서 매핑이 가능하다.

- **@Column**
  - @Column 선언이 꼭 필요한 것은 아니다. 
  - 하지만 @Column에서 지정한 변수명과 데이터베이스의 컬럼명을 서로 다르게 주고 싶다면 @Column(name=" ") 같은 형식으로 작성하면 된다. 
  - 그렇지 않은 경우에는 기본적으로 멤버 변수명과 일치하는 데이터베이스 컬럼을 매핑한다.

<br>

### Repository
Entity클래스를 작성했다면 이번엔 Repository 인터페이스를 만들어야 한다.

스프링부트에서는 Entity의 기본적인 CRUD가 가능하도록 JpaRepository 인터페이스를 제공한다.

````java
public interface ArticleRepository extends JpaRepository<Article, Long> {
}
````

Spring Data JPA에서 제공하는 JpaRepository 인터페이스를 상속하기만 해도 되며,
인터페이스에 따로 @Repository등의 어노테이션을 추가할 필요가 없다.

JpaRepository를 상속받을 때는 사용될 Entity 클래스와 ID 값이 들어가게 된다.

즉, `JpaRepository<T, ID>` 가 된다.

그렇게 JpaRepository를  단순하게 상속하는 것만으로 위의 인터페이스는 Entity 하나에 대해서 아래와 같은 기능을 제공하게 된다.

| method    | 기능                                      |
|-----------|-----------------------------------------|
| save()    | 레코드 저장 (insert, update)                 |
| findOne() | primary key로 레코드 한건 찾기                  |
| findAll() | 전체 레코드 불러오기. 정렬(sort), 페이징(pageable) 가능 |
| count()   | 레코드 갯수                                  |
| delete()  | 레코드 삭제                                  |

<br>
<br>

### 레퍼런스
- [SpringBoot JPA 예제](http://jdm.kr/blog/121)
- [스프링 부트 : 기본 개념 1) Entity, Repository 개념](https://whitepro.tistory.com/265)
- [spring-data-jpa + querydsl 로 개발하기 - 1](https://adrenal.tistory.com/23)

