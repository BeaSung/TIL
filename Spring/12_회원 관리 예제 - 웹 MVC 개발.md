## 백엔드 개발 핵심 노트

- 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(viewResolver)가 화면을 찾아서 처리한다.
    - 스프링 부트 템플릿엔진 기본 viewName 매핑
    - ``resources:templates/`` + (ViewName) + ``.html``


- **Gredle Project Build 하는 법(bash shell)** (windows cmd의 경우 ``./``를 제외)
    - gradlew가 있는 경로에서 명령어: ``./gradlew build``
    - 삭제 명령어: ``./gradlew clean`` or ``./gradlew clean build``
      삭제 시 주의 할 것! build 폴더를 삭제하기 때문에 다른 프로세스가 build 폴더를 사용해서는 안됨!
    - ``./build/libs``의 jar 실행 명령어: ``java -jar`` + (jar 파일명) +  ``.jar``


- `@ResponseBody`가 달린 컨트롤러의 메서드는 정적, 동적 웹 페이지가 아닌 **객체(.json)를 반환**한다.
    - ViewResolver에 가지 않는다.
    - HttpMessageConverter의 Defualt는 json이지만 xml로 반환 할 수 있다.


- `Optional<T>`
    - `` return Optional.ofNullable(Object);`` 객체가 Null이면 Null을 반환, 그렇지 않으면 객체를 반환
    - `.stream().findAny();`


- AtomicLong, ConCurrentHashMap


- JUnit `@Test`
    - ``Assertions.assertThat(object01).isEqualTo(object02)``: object01과 object02를 hashcode를 비교한다.
    - 테스트는 실행은 코드 순서를 보장 받지 못 한다. 즉, 테스트 메서드가 끝나면 데이터를 Clear 해주어야 한다.
        - 테스트는 서로 순서, 의존관계 없이 설계되어야 한다.
    - @AfterEach는 @Test 실행이 단위마다 끝날 때 마다 실행된다.
        - 반대로 @Test 실행 전에 작업을 실행 할 수 있는 @BeforeEach 또한 존재한다.
    - 원하는 클래스를 테스트 할 때는  Class를 선언한 곳에서 Create Test를 하면 된다.(Windows 기준 ``Ctrl`` + ``Shift`` + ``T``)
    - 테스트 메서드는 한글로 적어도 무방하다.
    - 테스트 기법: givne, when, then 패턴
        - given: 무언가 주어졌을 때
        - when: 이 것을 실행 했을 때
        - then: 결과로 나와야 할 것


- 여러 곳에서 하나의 객체를 사용할 때는 스프링 컨테이너에 빈으로 등록한다.
    - 즉, 싱글톤으로 등록된다. 그러나 설정을 통해 싱글톤이 되지 않도록  사용할 수 있다.


- HTTP 메서드는 form 태그 하위 input 태그의 name 속성과 스프링 컨트롤러의 파라메터에 해당하는 클래스의 인스턴스 변수와 매칭됨


- Controller에서 Model 객체를 파라메터로 View에게 넘겨 줄 수 있다.


- **JDBC**
    1. DataSource
        1. Connection (DB 연결)
        2. PreparedStatement (Qurey)
        3. ResultSet (결과반환)


- `@SpringbootTest`
    - 테스트 시 스프링 컨테이너와 함께 실행된다.


- `@Transactional`
    - Query 후  Commit이 되어야 트랜잭션이 발생됨
    - 트랜잭션을 롤백해버린다면? Query 하기 전으로 돌아간다.
    - Test 할 때 `@Transaction`를 사용하면 트랜잭션을 롤백하기 때문에 다음 Test 시 DB에 영향을 주지 않는다.


- **단위 테스트 vs 통합 테스트**
    - 단위 테스트: 각 서비스 별 자바 코드 테스트가 더 나은 테스트 일 수 있다.
    - 복합 테스트: 스프링 컨테이너를 실행하여 복합적인 테스트가 필요할 수 도 있다.


- **스프링 JdbcTemplate**
    - Jdbc API의 반복적인 부분을 제거할 수 있다. 


## 객체지향 설계 원칙 SOLID
### SOLID - SRP

### SOLID - 개방폐쇄 원칙(Open-Closed Principle)
확장에는 열려있고, 수정, 변경에는 닫혀있다.

- 스프링의 DI(Dependency Injection)을 사용하면 **기존의 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경** 할 수 있다.

<br>

## 스프링 빈을 등록하는 2가지 방법
### 1. 컴포넌트 스캔과 자동 의존관계 설정
- ``@Component`` 에노테이션이 있으면 스프링 빈으로 자동 등록된다.
    - ``@Contoller``, ``@Service``, ``@Repository``은 ``@Component``를 가지고 있다.
- ``@ComponentScan``은 ``@Component``를 자동으로 스프링 빈에 등록하기 때문이다.

### 2. 설정 파일을 통해 직접 등록하는 방법
1. ``@Configuration`` 선언된 클래스를 ComponentScan할 수 있는 경로에 정의한다.
2. ``@Bean``을 스프링빈에 등록할 개체를 반환하는 함수에 선언한다.
- 이 방법으로 ``@Service``, ``@Repository``을 대체할 수 있다.
- 스프링 빈에 등록할 객체가 정형화 되지 않거나, 상황에 따라 구현 클래스를 변경해야 할 때 이 방법을 사용한다.


### 의존성 주입(Dependency Injection)에는 필드 주입, setter 주입, 생성자 주입이 있다.
- `@Autowired`를 사용하여 의존성을 주입할 수 있다.(생성자 주입일 경우 생성자가 하나이면 `@Autowired`생략 가능)


#### 의존관계가 실행중에 동적으로  변경하는 경우는 거의 없으므로 생성자 주입을 권장한다.
- setter 주입의 경우 접근지정자가 public이기 때문에 의도치 않은 변경 등이 문제가 된다.


## 알아야 할 것
- JUnit Test
    - (org.junit.jupiter.api.Assertions) Assertions.assertThat
    - (org.assertj.core.api.Assertions) Assertions.assertThrow


## JPA
객체 중심의 설계, **ORM**이다.(ORM: Object Relational Mapping)

JPA는 인터페이스 성향을 띄며 그 구현체로 Hibernate가 있다.

데이터를 저장, 변경 시 Transaction이 필수이기 때문에 Service 계층에서 `@Transactional`을 잊지 말자!


## 스프링 데이터 JPA

JPA를 편리하게 사용하도록 도와주는 기술

interface로 생성하며 JpaRepository를 상속받는데, 스프링 데이터 JPA가 스프링 빈에 구현체를 만들어서 자동으로 등록한다. (개발자가 구현체를 만들 필요가 없음)

JpaRepository에서 공통적인 메서드를 제공한다.(`save(), findById(Long id), ...`)

공통적인 메서드 외에 필요로 하는 메서드를 추가할 경우?
- 인터페이스를 통한 CRUD가 가능하기 때문에 `findByName(), findByEmail()` 처럼 메서드 이름 만으로 조회가 가능하다.

페이징 기능 자동 제공

더욱 복잡한 쿼리가 필요한 경우에는 QueryDSL 이라는 라이브러리를 사용한 동적 쿼리 작성이 가능하다.

JPA는 네이티브 쿼리를 작성 할 수 있도록 남겨두었기 때문에 SQL를 작성하여도 무방하다.


## AOP
#### 핵심 로직 상황(core concern)과 공통 로직 상황(cross-cutting concern)을 분리하자!

ex) 각 서비스 메서드가 작성완료된 상태에서 각 각의 메서드의 실행 시간을 구하는 코드를 추가해야 된다면?
➡ 서비스 메서드를 전부 수정하지 않고, AOP를 활용하여 해결 할 수 있다.

AOP Class에 `@Aspect` 할 것! ➡ 작성 후 스프링 빈에 등록 ➡ `@Around`로 타겟 지정
- `@Configuration`에서 `@Bean`을 직접 등록 할 때는 자기 자신을 `@Around` 타겟으로 지정하기 때문에 스프링 빈의 순환참조 문제가 발생 할 수 있다. ➡ 타겟에서 제외하는 `!target(com.pyong.hellospring.SpringConfig)`를 `@Around`에 추가하여 순환 참조 문제를 해결한다.

#### 어떻게 AOP가 적용될 수 있을까?
- MVC 패턴 기준 Controller와 Service 사이에 가짜 Service가 되는 프록시가 `joinPoint.proceed()`를 호출
  Cglib 프록시가 주입 `EnhancerBySpringCGLIB`



<br>
<br>

### 레퍼런스
- [인프런 강의: 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/lecture?courseSlug=스프링-입문-스프링부트&unitId=49587&tab=curriculum "인프런 강의")

