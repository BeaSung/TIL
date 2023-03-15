## IoC와 DI ( 객체 주입 )
### Ioc

<img src="https://velog.velcdn.com/images/hj_/post/c0d1ba03-8bbc-464f-9a30-2f56c5f82d6b/image.PNG">

- **IoC** : 프로그램의 흐름이 개발자의 코드가 아닌 외부에 의해 제어되는 개념
- **IoC Container**
  - 핵심 객체를 관리하는 창고 
  - Controller, Service, Repository 등의 다양한 객체들을 만들고 관리 
  - IoC에 담긴 객체들은 필요에 따라 또 다른 객체들로 주입될 수 있음 
  - 이러한 객체 생성과 관리, 주입은 개발자의 코드가 아닌 IoC 컨테이너에 의해 통제

### DI
- **DI** : 의존성 주입하기
- 필요한 객체를 외부에서 또 다른 객체로 꽂아주는(주입하는) 방식
- 동작에 필요한 객체를 외부에서 받아옴

### 관련 어노테이션
- `@SpringBootTest` : IoC Container를 테스트에서 사용하기 위해 사용
- `@Autowired` : IoC Container가 객체를 DI 하도록 함
  - new 를 이용하여 직접 객체를 생성하지 않아도 됨
- `@Component` : 해당 클래스를 객체로 만들고, 이를 IoC Container에 등록하도록 함

<br>

## AOP, 관점 지향 프로그래밍 (부가 기능 주입)
### AOP 설명
- AOP (Aspect-Oriented Programming)
- 핵심 기능과 함께 Logging, Sercure, Transaction 등과 같은 부가 기능이 필요 
- 부가 기능을 특정 지점에 삽입하는 기법 ( 특정 로직을 주입 )
- 간격하고 효율적인 프로그래밍이 가능하도록 함

### 관련 어노테이션
- `@Aspect` : 부가 기능 주입을 위한 AOP 클래스 선언 
- `@Pointcut` : 주입 대상 지정 
- `@Before` : 대상 실행 이전에 수행 
- `@After` : 대상 실행 후, 수행 
- `@AfterReturning` : 대상 실행 후, 수행 (정상 수행 시)
- `@AfterThrowing` : 대상 실행 후, 수행 (예외 발생 시)
- `@Around` : 대상 실행 전후로 수행

### ex> 댓글 생성 입출력 Logging
- 댓글 서비스가 호출될 때, 입력값과 결과값을 log로 확인
- `@Slf4j`, `log.info()` 를 사용하는 방법
  - 핵심 코드가 아닌 부가적인 코드가 함께 섞여 있기 때문에 좋은 방법은 아님
  - 파라미터가 주어지는 모든 메소드에 직접 작성을 해주어야함
  - AOP를 사용하여 해결

#### 1. 입력값 logging AOP
```java
// 대상 메소드 선택 : CommentService#create()
    @Pointcut("execution(* com.ghkwhd.project.service.CommentService.create(..))")   // 어느 지점을 타켓으로 하여 부가기능을 삽입할 것인지
    private void cut() {

    }

    // 실행 시점 설정 : cut()의 대상이 수행되기 이전
    @Before("cut()")    // 부가 기능이 cut()에 지정된 Pointcut을 대상으로 아래 메소드가 실행된다
    // 입력되는 전달값이 무엇인지 logging을 위해 작성
    public void loggingArgs(JoinPoint joinPoint) {
        // joingPoint : cut()의 대상 메소드 (메소드를 둘러싼 지점?)

        // 입력값 가져오기
        Object[] args = joinPoint.getArgs();

        // 클래스명
        String className = joinPoint.getTarget().getClass().getSimpleName();

        // 메소드명
        String methodName = joinPoint.getSignature().getName();

        // 입력값 로깅하기
        for(Object obj : args) {
           log.info("{}#{}의 입력값 => {}", className, methodName, obj);
        }

    }
```

- 기본 패키지에 aop 패키지 생성 -> Debugging 클래스 생성
- `@Aspect` : AOP 선언 ( AOP 클래스임을 선언 ) : 부가기능을 주입하는 클래스 
- `@Component` : 객체 생성은 IoC Container에게 맡김 (IoC Container가 해당 객체를 생성 및 관리)
- `@Slf4j` : Logging 기능을 위해 추가 
- `@Pointcut("excution(경로)")` : 어느 지점을 타켓으로 하여 부가기능을 삽입할 것인지
- `@Before()` : () 안의 대상이 수행되기 이전에 수행되도록 함

#### 2. 반환값 logging AOP
```java
// 실행 시점 설정 : cut()에 지정된 대상 호출 성공 후 수행
    @AfterReturning(value = "cut()", returning = "returnObj")
    public void loggingReturnValue(JoinPoint joinPoint,  // cut의 대상 메소드
                                   Object returnObj) {   // return 값

        // 클래스명
        String className = joinPoint.getTarget().getClass().getSimpleName();

        // 메소드명
        String methodName = joinPoint.getSignature().getName();

        // 반환값 로깅
        log.info("{}#{}의 반환값 => {}", className, methodName, returnObj);

    }
```

- @AfterReturning(value = "대상", returning = "Obj명")
  - value로 지정된 대상 호출 성공 후 수행 
  - returning에 지정된 obj명과 현재 메소드의 Object 파라미터의 이름과 동일해야함 ( return값을 받아오기 위해 )


#### 3. AOP 대상 범위 변경
```java
@Pointcut("execution(* com.ghkwhd.project.service.CommentService.*(..))")
```
- 메소드 이름 대신 * 표시로 변경
- CommentService의 모든 메소드에 적용

### 수행시간 측정 AOP
#### 1. 어노테이션 생성
```java
@Target({ElementType.TYPE, ElementType.METHOD}) // 어노테이션 적용 대상
@Retention(RetentionPolicy.RUNTIME) // 어노테이션 유지 기간 (RUNTIME 시점까지 유지하겠다)
public @interface RunningTime {
}
```

- 기본 패키지/annotation 패키지 생성 
- 패키지 내부에 Annotation으로 파일 생성 (커스텀 어노테이션)
- `@Target({ElementType.TYPE, ElementType.METHOD})` : 어노테이션 적용 대상 지정
- `@Retention(RetentionPolicy.RUNTIME)` : 어노테이션 유지 기간 (RUNTIME 시점까지 유지하겠다)

#### 2. 적용하기
```java
@Aspect
@Component
@Slf4j
public class PerformanceAspect {

    // 특정 어노테이션을 대상 지정
    @Pointcut("@annotation(com.ghkwhd.project.annotation.RunningTime)")
    private void enableRunningTIme() {}

    // 기본 패키지의 모든 메소드
    @Pointcut("execution(* com.ghkwhd.project..*.*(..))")
    private void cut() {}

    // 실행 시점 설정 : 두 조건을 모두 만족하는 대상을 전후로 부가 기능을 삽입
    @Around("cut() && enableRunningTIme()")
    public void loggingRunningTime(ProceedingJoinPoint joinPoint) throws Throwable {
        // ProceddingJoinPoing : 대상을 실행까지 할 수 있는 joingPoint

        // 메소드 수행 전, 측정 시작
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        // 메소드 수행
        Object returningObj = joinPoint.proceed();    // 타켓팅이 된 대상을 수행
        String methodName = joinPoint.getSignature().getName();

        // 메소드 수행 후, 측정 종료 및 로깅
        stopWatch.stop();
        log.info("{}의 총 수행 시간 => {} sec", methodName, stopWatch.getTotalTimeSeconds());
    }

}
```

- `cut()` 메소드에 지정된 모든 메소드이면서 RunningTime 어노테이션을 가지고 있는 애들을 대상으로 수행시간을 측정
- 특정 메소드에 @RunningTime을 선언하면 그 메소드의 수행 시간을 측정 가능

<br>

## ObjectMapper( JSON을 객체로 변환 )
### ObjectMapper
- JSON과 자바 객체 간의 변환을 제공하는 객체
- 요청한 JSON을 DTO로 만들고 ( readValue() )
- 응답을 위한 DTO를 JSON으로 변환 ( wirteValueAsString() )


<br>
<br>

### 레퍼런스
- [스프링 강의](https://youtu.be/5d3wEN0_MLY "홍팍의 스프링 강의")

