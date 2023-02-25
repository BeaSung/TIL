## 람다식
- 함수(메서드)를 간단한 식으로 표현한 방법
- 익명함수 (반환타입, 이름을 지우기)

#### 함수와 메서드의 차이
- 근본적으로 동일
- **함수**: 일반적 용어 (클래스에 독립적)
- **메서드**: 객체지향개념 용어(클래스에 종속적)

<br>

#### 람다식 작성방법
1. 메서드의 이름, 반환타입 제거 '**->**'를 블록 앞에 추가
    ```
    (int a, int b) -> {
    return a > b ? a : b;
    }
    ```
2. **반환값 있는 경우**, 식이나 값만 적고 **return문 생략 가능** 
    ```
    (int a, int b) -> a > b ? a : b
    ```
3. 매개변수와 타입이 추론가능 시 생략가능(대부분의 경우 생략가능)
    ```
    (a, b) -> a > b ? a : b
    ```

####  람다식 작성 시 주의사항
1. 매개변수가 하나일 시 -> 괄호 생략가능(타입이 없을 때만)
    ```
    (a) -> a * a
    ```
    ```
    a -> a * a
    ```
2. 블록 안의 문장 하나뿐 -> 괄호 생략가능 
    ```
    (int i) -> {
        System.out.println(i);
    }
    ```
    ```
    (int i) -> System.out.println(i)
    ```
<br>

- 람다식은 익명 함수가 아니라 **익명객체**이다.
- 람다식을 다루기 위한 참조변수가 필요. 참조변수의 타입은 Object
- 하지만 Object리모콘으로는 메서드가 존재하지 않기 때문에 사용 안됨.
- 해결방법은 **함수형 인터페이스**

<br>

## 함수형 인터페이스
- 단하나의 추상 메서드만 선언된 인터페이스
- 람다식을 다루기 위해 사용하는 것
- `@FuncionalInterface` **annotation을 붙이자** (컴파일러가 한번 검증해준다.)
   ```
   interface MyFunction {
      public abstract int max(int a, int b);
   }
   ```
   ```
   MyFunction f = new MyFunction() {
      public int max(int a , int b) {
      return a> b ? a : b;
      }
   };
   ```
- 이전에 Obejct 타입으로 익명객체를 참조했을때는 메소드를 사용할 수 없었지만(Object일땐 없지만), 함수형 인터페이스 타입으로 선언하면 메소드 사용이 가능하다.
- 람다식을 다루기 위한 참조변수의 타입은 함수형 인터페이스 타입으로 한다.

<br>

##  java.util.function 패키지
- 함수형 인터페이스 API
- 자주 사용하는 다양한 함수형 인터페이스 제공(표준화)

| 인터페이스명   | 반환 자료형 | 메서드  | 매개 변수 |
|----------|--------|------|-------|
| Runnable | void   | run() | 없음    |
| Supplier | T      | get() | 없음    |
| Consumer | void   | accept(Tt) | 1개    |
| Function<T,R> | R   | apply(Tt) | 1개    |
| Predicate | boolean   | test(Tt) | 1개    |

<br>

## 메서드 참조(method reference)
- 람다식을 더 간단히 한 것 `(클래스이름 :: 메서드이름)`

<br>

#### static 메서드 참조
  - `(x) -> ClassName.method(x)` (람다)
  - `ClassName::method` (메서드참조)
    ```Java
    public class StaticExample { 
        public static void main(String[] args){
      
            // 람다식을 이용한 메서드 참조
            Function<String, string> helloLambda = (name) -> HelloTo.hello(name);
            System.out.println(helloLambda.apply("Bea"));
      
            // static 메서드를 이용한 메서드참조
            Function<String, string> helloStatic = HelloTo::hello;
            System.out.println(helloStatic.apply("개발자"));
        }
    }
      
    class HelloTo {
        public static String hello(String name) {
            return "Hello~" + name;
        }
    }
    ```
<br>

#### instance 메서드 참조
- `(obj, x) -> obj.method(x)`
- `ClassName::method`
    ```Java
    public class InstanceExample { 
        public static void main(String[] args){
            String greeting = "Hello";
            Consumer<String> consumer = System.out::println;
            consumer.accept(greeting);  
            
            writeString(greeting::toString);
        }
  
        public static void writeString(Supplier<String> supplier) {
            System.out.println(supplier.get());
        }
    }
    ```
    ```Java
    public class InstanceExample2 { 
        public static void main(String[] args){
            Math math = new Math();
  
            // 람다식을 이용한 메서드 참조
            BiFunction<Integer, Integer, Integer> minus1 = (a, b) -> math.minus(a, b);
  
            // static 메서드를 이용한 메서드참조
            BiFunction<Integer, Integer, Integer> minus2 = math::minus;
      
            System.out.println("람다식 - " + minus1.apply(10, 2));
            System.out.println("메서드 참조 - " + minus2.apply(5, 2));
        }
    }
      
    class Math {
        public int minus(int a, int b) {   // 2개의 매개변수, 반환자료형 있음
            return a - b;
        }
    }
    ```

<br>

#### 생성자의 메서드 참조
- Supplier<MyClass> s () -> new MyClass();
- Supplier<MyClass> s = MyClaas::new;
    ```java
    public class ConstructorExample {
        public static void main(String[]args){
            Supplier<Name> supplier1 = () -> new Name();
            Name name1 = supplier1.get();   // Name name = new Name();
            System.out.println("람다식 - " + name1.getName());
            
            Supplier<Name> = supplier2 = Name::new;
            Name name2 = supplier2.get();
            System.out.println("생성자 참조 - " + name2.getName());
        }
    }
    
    @Getter
    @NoArgsConstructor
    class Name {
        private String name;
    }
    ```

