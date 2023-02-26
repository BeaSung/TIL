## 스트림(Stream) 정의
- 다양한 데이터 소스를 표준화된 방법으로 다루기 위한 것

#### 스트림의 과정
1. 스트림을 만든다.
2. 중간연산(0~**n번**) 
3. 최종연산(**1번**, 스트림 요소를 소모)

#### 스트림의 특징
1. Read only(원본 변경 x)
2. 일회용이다(필요시 스트림 다시 생성)
3. 최종연산 전까지 중간연산 수행 안됨.
4. 작업을 내부 반복으로 처리한다 (forEach : 코드간결, 성능저하)
5. 멀티쓰레드로 병렬처리(==병렬스트림, parallel()호출)

#### 기본형 스트림
- `IntStream`, `LongStream`, `DoubleStream`
- 더 효율적으로 사용하기 위함
- 오토박싱 언박싱의 비효율이 제거됨 (**Stream<Integer>대신 IntStream**)
- 숫자와 관련된 유용한 메서드를 **Stream<T>보다 더 많이 제공한다.**

<br>

## 스트림 만들기
#### 스트림 만들기 - 컬렉션
- Collection 인터페이스의 stream()으로 컬렉션을 스트림으로 변환
    ```java
    Stream<E> stream()  // Collection 인터페이스의 메서드
    ```
    ```java
    List<Integer> list = Arrays.aslist(1, 2, 3, 4, 5);
    Stream<Integer> intStream = list.stream();  // list를 스트림으로 변
    
    // 스트림의 모든 요소를 출력
    intStream.foreach(System.out::print);  // 출력: 12345 
    ```
  
<br>

#### 스트림 만들기 - 배열
- **객체 배열**로부터 스트림 생성
    ```java
    Stream<T> Stream.of(T... values) // 가변인자
    Stream<T> Stream.of(T[])
    Stream<T> Arrays.stream(T[])
    ```
- **기본형 배열**로부터 스트림 생성
    ```java
    IntStream IntStream.of(int... values)
    IntStream IntStream.of(int[])
    IntStream Arrays.stream(int[])
    ```

<br>

## 스트림의 연산
- 스트림이 제공하는 기능
  - `중간연산` : 연산결과가 스트림, 반복적으로 적용가능(0~n번)
  - `최종연산` : 연산결과가 스트림이 아닌 연산, 단 한번만 적용가능(스트림의 요소를 소모, 0~1번)
  ```java
  stream.distinct().limits(5).sorted().forEach(System.out::println);  // forEach는 최종 나머지는 중간
  ```

<br>

#### 최종연산
| 최종연산                     | 설명                           |
|--------------------------|------------------------------|
| `Optional<T>`            | 최소 요소 하나를 반환(null처리 방법 중 하나) |
| `reduce()` & `collect()` | 핵심 최종 연산                     |
| `reduce()`               | 스트림의 요소를 하나씩 줄여가면서 계산(리듀싱)   |
| `collect()`              | reduce()를 이용해서 grouping      |


#### 중간연산
| 중간연산                     | 설명                                                   |
|--------------------------|------------------------------------------------------|
| `skip()`, `limit()`      | 자르기                                                  |
| `filter()`, `distinct()` | 요소 걸러내기 (중간연산이기 때문에 여러번 사용가능)                        |
| `sorted()`               | 정렬하기 (정렬 시 **정렬기준**과 **정렬대상** 필요)                    |
| `map()`                  | 요소 변환                                                |
| `peek()`                 | 스트림의 요소를 소비하지 않고 **엿보기**(중간중간에 작업결과 확인 시 사용. 디버깅 용도) |
| `flatMap()`              | 스트림의 스트림을 **스트림으로 변환**. 하나의 스트림으로 변환                 |

<br>

## Optional<T>
- T 타입 객체의 Wrapper Class
  ```java
  public final class Optional<T> {
      private final T value;  // T 타입의 참조변수
                  ....
  }
  ```

<br>

#### Optional을 다루는 이유
1. Null을 직접다루는 건 위험
   - NullPointerException발생 위험
2. Null체크를 꼭 해줘야 함
   - if문 필수, 코드가 지저분해 짐
- 해당 처리와 관련된 다른 처리 방법
  - 빈 배열, 빈 문자열로 초기화 (Null을 직접 다루는 위험성을 회피하기 위함)

<br>

#### Optional<T> 객체 생성하기
- Optional<T> 객체를 생성하는 방법
  ```java
  String str = "abc";
  
  Optional<String> optVal = Optional.of(str);
  Optional<String> optVal = Optional.of("abc");
  ```

- null대신 빈 Optional<T> 객체를 활용하자
  ```java
  Optional<String> optVal = null;   // null로 초기화 -> 바람직하지 X
  Optional<String> optVal = Optional.<String> empty();
  ```

<br>

#### Optional<T> 객체의 값 가져오기
- `get()`, `orElse()`, `orElseGet()`, `orElseThrow()`
  ```java
  Optional<String> optVal = Optional.of("abc")
  
  String str1 = optVal.get();
  String str2 = optVal.orElse("");
  String str3 = optVal.orElseGet(String::new);
  String str4 = optVal.orElseThrow(NullPointerException::new);
  ```

<br>

#### 기본형 값을 감싸는 Wrapper Class
- 성능을 올리려고 사용
- `OptionalInt`, `OptionalLong`, `OptionalDouble`
- OptionalInt의 **값 가져오기**: `int getAsInt()`
- 빈 Optional 객체와의 비교
  ```java
  // 숫자 0이 들어있는지, 비어있는 것인지 구별할 때 사용
  OptionalInt opt = OptionalInt.of(0);
  OptionalInt opt2 = OptionalInt.empty();
  
  // isPresent()를 사용해서 구분할 수 있다.
  System.out.println(opt.isPresent());   // true
  System.out.println(opt2.isPresent());   // false
  System.out.println(opt.equals(opt2));   // false
  ```
  
