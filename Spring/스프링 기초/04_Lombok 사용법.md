## Lombok

### Lombok 이란
Lombok은 애너테이션 기반으로 코드를 자동완성 해주는 라이브러리이다. Lombok을 이용하면 Getter, Setter, Equals, ToString 등과 같은 코드를 자동완성 시킬 수 있다.

<br>

```java
public class Member{
    private String id;
    private String name;

    public Member(){}

    public String getId(){
        return id;
    }
    public void setId(String id){
        this.id = id;
    }
    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    }
}
```

위와 같이 VO 클래스 정의를 Lombok을 이용하면 아래와 같이 간단하게 작성할 수 있다.

````java
@Getter
@Setter
@NoArgsConstructor
public class Member{
    private String id;
    private String name;
}
````

<br>

### Lombok의 장점
- 애너테이션 기반의 코드 자동 생성을 통한 생산성 향상 
- 코드 가독성 및 유지보수성 향상 
- Getter, Setter 외에 빌더 패턴이나 로그 생성 등 다양한 방면으로 활용 가능

<br>

### Lombok 사용시 주의할 점
Lombok 사용 시 주의가 필요한 부분이 분명히 존재한다. (하단 레퍼런스에 **'Lombok 을 사용할 때 주의해야 하는 점들'** 글 참고) 그래서 올바로 알고 주의해서 사용하기를 권장한다.

<br>

### 대표적인 예제

#### @Getter @Setter
Lombok에서 가장 자주 활용하는 애너테이션이다. @Getter와 @Setter를 클래스 이름 위에 적용시키면 모든 변수들에 적용이 가능하고, 변수 이름 위에 적용시키면 해당 변수들만 적용 가능하다. 
````java
@Getter
public class Member{
    private String id;
    @Setter
    private String name;
}
````

<br>

#### @AllArgsConstructor
@AllArgsConstructor는 모든 변수를 사용하는 생성자를 자동완성 시켜주는 애너테이션이다.
````java
@Getter
@AllArgsContructor
public class Member{
    private String id;    
    private String name;
    
    /* AllArgsContructor를 통해 아래와 같은 생성자를 자동 생성할 수 있다.
    public Member(String id, String name){
      this.id = id;
      this.name = name;
    }
    */
}
````

#### @NoArgsConstructor
@NoArgsConstructor는 어떠한 변수도 사용하지 않는 기본 생성자를 자동완성 시켜주는 애너테이션이다.
````java
@Getter
@NoArgsContructor
public class Member{
    private String id;    
    private String name;
    
    /* NoArgsContructor를 통해 아래와 같은 생성자를 자동 생성할 수 있다.
    public Member(){}
    */
}
````

#### @RequiredArgsConstructor
@RequiredArgsConstructor는 특정 변수만을 활용하는 생성자를 자동완성 시켜주는 애너테이션이다. 생성자의 인자로 추가할 변수에 @NonNull 어노테이션 또는 변수에  final로 선언해서 의존성을 주입받을 수 있다.
```java
@Getter
@RequiredArgsContructor
public class Member extends Common{
    @NonNull
    private String id;    
    private final String name;  // final 선언
    
    /* RequiredArgsContructor 를 통해 아래와 같은 생성자를 자동 생성할 수 있다.
    public Member(String id, String name){
    	this.id = id;
        this.name = name;
    }
    */
}
```
위 애너테이션들의 필드 선언순서에 따라 생성자 인자가 정해진다. 나중에 리펙토링을 하게 되면 인자 순서가 변경될 수 있다는 점을 기억하자.

<br>

#### @EqualsAndHashCode
@EqualsAndHashCode 애너테이션을 활용하면 클래스에 대한 equals 함수와 hashCode 함수를 자동으로 생성해준다.
```java
@RequiredArgsContructor
@EqualsAndHashCode(of={"id", "name"}, callSuper=false)
public class Member extends Common{
    @NonNull
    private String id;    
    private final String name;  // final 선언
        
}
```

<br>

#### @ToString
@ToString 애너테이션을 활용하면 클래스의 변수들을 기반으로 ToString 메소드를 자동으로 완성시켜 준다. 출력을 원하지 않는 변수에 @ToString.Exclude 애너테이션을 붙여주면 출력을 제외할 수 있다. 또한 상위 클래스에 대해도 toString을 적용시키고자 한다면 상위 클래스에 @ToString(callSuper = true) 를 적용시키면 된다. 
````java
@ToString
public class Member extends Common{

    @ToString.Exclude
    private String id;    // id는 toString에서 제외
    
    private final String name;  // final 선언
}
````

<br>

#### @Data
@Data 애너테이션을 활용하면 @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor를 자동완성 시켜준다.

<br>

#### @Builder
@Builder 애너테이션을 활용하면 해당 클래스의 객체의 생성에 Builder패턴을 적용시켜준다. 모든 변수들에 대해 build하기를 원한다면 클래스 위에 @Builder를 붙이면 되지만, 특정 변수만을 build하기 원한다면 생성자를 작성하고 그 위에 @Builder 애너테이션을 붙여주면 된다.
```java
@NoArgsContructor
@Getter
//@Builder  // 모든 변수를 초기화 하는 builder 생성시 클래스 레벨에 선언
public class Member extends Common{

    private String id;    
    private String name;
    private String phone;
    
    // 특정 변수만을 초기화 하려면 특정 생성자에 선언
    @Builder
    public Member(String id, String name){
    	this.id = id;
        this.name = name;
    }
    
    /* 
    @Builder 선언시 Lombok에 의해 빌더생성을 위한 다음의 코드가 생성됨.
    public static Member.MemberBuilder builder() {
        return new Member.MemberBuilder();
    }

    public static class MemberBuilder {
        private String id;
        private String name;

        MemberBuilder() {
        }

        public Member.MemberBuilder id(final String id) {
            this.id = id;
            return this;
        }

        public Member.MemberBuilder name(final String name) {
            this.name = name;
            return this;
        }

        public Member build() {
            return new Member(this.id, this.name);
        }

        public String toString() {
            return "Member.MemberBuilder(id=" + this.id + ", name=" + this.name + ")";
        }
    }    
    */
}
```

빌더를 생성했다면 아래와 같이 사용하면 된다.
````java
public class MemberController{

    public ResponseEntity init(){
    	
        // 빌더 패턴을 이용한 객체 생성
        Member member = Member.builder()
            .id("hong")
            .name("홍길동")
            .build();
        
        return ResponseEntity.ok(member);    
    }
}
````

<br>

## 리팩토링이란?
리팩토링은 객체지향 개발자라면 반드시 익혀야 하는 기법이다. 

겉으로 드러나는(화면상) 기능은 그대로 지만, 코드 구조와 구현 방법을 바꿈으로써 더 나아지는 것에 주력하는 작업이다.

<br>

### 리팩토링을 하는 이유
- 코드의 가독성이 높아짐 
- 미래에 닥쳐올 변화에 효율적으로 대응하기에 적합 
- 생산성 증가 , 코드의 품질 향상 
- 유지보수 용이 
- 견고하면서 유연한 소프트웨어를 만들 수 있음

<br>
<br>

### 레퍼런스
- [[Java] Lombok 사용법](https://phantom.tistory.com/63)
- [Lombok 기본 사용법 익히기](https://blog.advenoh.pe.kr/java/Lombok-%EA%B8%B0%EB%B3%B8-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%9D%B5%ED%9E%88%EA%B8%B0/)
- [Lombok 을 사용할 때 주의해야 하는 점들 정리](https://jake-seo-dev.tistory.com/70)
- [리팩토링이란?](https://yjkim97.tistory.com/34)
