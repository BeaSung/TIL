## java.lang패키지와 유용한 클래스

## Object클래스
#### java.lang 패키지
- java.lang 패키지는 자바에서 가장 기본적인 동작을 수행하는 클래스들의 집합이다.
- 따라서 자바에서는 java.lang 패키지의 클래스들은 **import 문을 사용하지 않아도** 클래스 이름만으로 바로 사용할 수 있도록 하고 있다.

#### java.lang.Object 클래스
- java.lang 패키지 중에서도 가장 많이 사용되는 클래스는 바로 Object 클래스이다.
- Object 클래스는 모든 자바 클래스의 **최고 조상 클래스**가 된다.
- 따라서 자바의 모든 클래스는 Object 클래스의 모든 메서드를 바로 사용할 수 있다. 
- 이러한 Object 클래스는 **필드를 가지지 않으며**, 총 11개의 메서드만으로 구성되어 있다.

#### Object클래스의 메서드(가장 중요한 3가지)
| 메서드                               | 설명                                    |
|-----------------------------------|---------------------------------------|
| public boolean eauqls(Object obj) | 객체 자신과 객체 obj가 같은 객체인지 알려준다(같으면 true) |
| Public int hashcode()             | 객체 자신의 해시코드를 반환한다                     |
| Public String toString()          | 객체 자신의 정보를 문자열로 반환한다                  |
<br>

#### equals() 메서드
- equals() 메서드는 해당 인스턴스를 매개변수로 전달받는 참조 변수와 비교하여, 그 결과를 반환한다. 
- 이때 **참조 변수가 가리키는 값(주소 값)을 비교**하므로, **서로 다른 두 객체는 언제나 false를 반환하게 된다.**
- 다음 예제는 equals() 메서드를 이용하여 두 인스턴스를 서로 비교하는 예제이다.
```java
Car car1 = new Car();
Car car2 = new Car();

System.out.println(car1.equals(car2));
    
// 실행 결과
// false
```
<br>

#### equals() 오버라이딩
- equals()오버라이딩을 안해주면 참조변수에 저장된 값(주소값)으로 판단한다.(Object equals는 원래 주소값을 비교함)
- 같은 value라도 각각 다른 참조변수에 저장되어, 주소값은 다르기 때문에 다르다고 나온다. 
- 그러므로, 참조변수가 가지고 있는 value를 비교하기 위해서는 equals()의 오버라이딩을 써야한다.
- 오버라이딩을 하므로 괄호 안에있는 **value를 비교하게 한다.**
```java
class Car { 
    long id;
    
    public boolean equals(Object obj) {
       if(obj instanceof Car)
           return id==((Car)obj).id;
       else
           return false;
    }
    Car(long id){
       this.id=id;
    }
}

public class Ex01 {

    public static void main(String[] args) {
        Car c1=new Car(8011081111222L);
       Car c2=new Car(8011081111222L);
        
        if(c1.equals(c2)) {
          System.out.println("같은차량");
       }
       else {
          System.out.println("다른차량");
       }
    }
}
    // 결과 : 같은차량
```
<br>

#### hashCode 메서드
- hashCode 메서드는 객체의 주소 값을 이용해서 해싱(hashing) 기법을 통해 해시 코드를 만든 후 반환한다.
- 그렇기 때문에 **서로 다른 두 객체는 같은 해시 코드를 가질 수 없게 된다.** 그래서 해시코드는 `객체의 지문`이라고도 한다.
- 엄밀히 말하면 해시코드는 주소값은 아니고, `주소값으로 만든 고유한 숫자값`이라고 하는게 옳다.
- 자바에서의 동일성은 equals()의 반환값이 true, hashCode() 반환값이 동일함을 의미한다.
- 그래서 일반적으로 equals()와 hashCode()는 함께 override 한다.
```java
class Person {
    String name;
 
    public Person(String name) {
        this.name = name;
    }
}
 
public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("홍길동");
        Person p2 = new Person("홍길동");
 
        // 객체 인스턴스마다 각기 다른 주해시코드(주소))를 가지고 있다.
        System.out.println(p1.hashCode()); // 622488023
        System.out.println(p2.hashCode()); // 1933863327
    }
}
```
<br>

#### hashCode() 오버라이딩
- **기본 동작** : JVM이 부여한 코드값. 인스턴스가 저장된 가상머신의 주소를 10진수로 반환
- **override 목적**: 두 개의 서로 다른 메모리에 위치한 객체가 동일성을 갖기 위해
- 해시코드란 JVM이 인스턴스를 생성할 때 메모리 주소를 변환해서 부여하는 코드
- 실제 메모리 주소값과는 별개의 값이며 실제 메모리 주소는 System 클래스의 identityHashCode()로 확인할 수 있다.
- 자바에서의 동일성은 equals()의 반환값이 true, hashCode() 반환값이 동일함을 의미한다.
- 그래서 일반적으로 equals()와 hashCode()는 함께 override 한다.

#### toString() 메서드
- toString() 메소드는 **해당 인스턴스에 대한 정보를 문자열로 반환한다.**
- 이때 반환되는 문자열은 클래스 이름과 함께 구분자로 '@'가 사용되며, 그 뒤로 16진수 해시 코드(hash code)가 추가된다.
- 16진수 해시 코드 값은 인스턴스의 주소를 가리키는 값으로, 인스턴스마다 모두 다르게 반환된다.
- 다음 예제는 toString() 메소드를 이용하여 인스턴스의 정보를 출력하는 예제이다.
```java
Car car1 = new Car();
Car car2 = new Car();

System.out.println(car1.toString());
System.out.println(car2.toString());

// 실행 결과
// Car@15db9742
// Car@6d06d69c
```
<br>

#### toString() 오버라이딩
- 인스턴스나 클래스에 대한 정보 또는 인스턴스 변수들의 값을 문자열로 변환하여 반환하도록 오버라이딩 되는 것
- 객체를 출력하기만 하면, 객체가 가지고 있는 모든 정보를 확인할 수 있다. 우리가 직접 호출하지 않아도, 로깅을 하거나 에러메세지를 출력할 때에도 유용하게 사용할 수 있다.
- toString 은 객체가 가진 주요 정보를 모두 반환하는 것이 좋다. 이상적으로는 스스로를 완벽히 설명하는 문자열이어야 한다.
- 이렇게 재정의된 toString 메소드는 직접 호출하지 않더라도 문자열 출력시, 문자열 결합 연산자 등을 사용할 때 자동으로 호출된다.
```java
class Car {
    final String name;
    final int position;

    Car(String name, int position) {
        this.name = name;
        this.position = position;
    }

    @Override
    public String toString() {
        return String.format("자동차 (이름 = %s, 위치 = %s)", name, position);
    }
}
```
<br>

## String클래스
- 문자열을 저장하고 이를 다루는데 필요한 메서드를 함께 제공한다.
- **변경 불가능한(immutable) 클래스**
  - String클래스에는 문자열을 저장하기 위해서 문자형 배열 참조변수(char[]) value를 인스턴스 변수로 정의해놓고 있다.
  - 인스턴스 생서 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스 변수(value)에 문자형 배열(char[])로 저장되는 것이다.
  - 한번 생성된 String인스턴스가 갖고 있는 문자열은 **읽어 올 수 만 있고, 변경할 수는 없다.**
  - String클래스는 앞에 final이 붙어 있으므로 다른 클래스의 조상이 될 수 없다.
```java
public final class String implements java.io.Serializable, Comparable {
    private char[] vlaue;
    ...
}
```
<br>

#### 문자열(String)의 비교
- 문자열을 만들 때의 두 가지 방법
1. 문자열 리터럴을 지정하는 방법
2. String클래스의 생성자를 사용해서 만드는 방법
```java
String str1 = "abc";    // 문자열 리터럴 "abc"의 주소가 str1에 저장됨
String str2 = "abc";    // 문자열 리터럴 "abc"의 주소가 str2에 저장됨
String str3 = new String("abc");    // 새로운 String인스턴스를 생성
String str4 = new String("abc");    // 새로운 String인스턴스를 생성
```
<br>

#### 문자열 리터럴(String리터럴)
- String리터럴들은 컴파일 시에 클래스 파일에 저장된다. 그래서 아래 예제를 실행하면 "AAA"라는 문자열을 담고 있는 String인스턴스가 하나 생성된 후, 참조변수 s1, s2, s3는 모두 이 String인스턴스를 참조하게 된다.
- 클래스 파일이 클래스 로더에 의해 메모리에 올라갈 때, 클래스 파일의 리터럴들이 JVM내에 있는 '상수 저장소(constant pool)'에 저장된다.
```java
String s1 = "AAA";
String s2 = "AAA";
String s3 = "AAA";
String s4 = "BBB";
```
<br>

#### 빈 문자열(empty string)
- 내용이 없는 문자, 크기가 0인 char형 배열을 저장하는 문자열
- 크기가 0인 배열을 생성하는 것은 어느 타입이나 가능
- `String s = "";`은 가능해도 `char c = '';`는 불가능. 
  - **char형 변수에는 반드시 하나의 문자를 지정해야한다.(공백도 가능)**
- String은 참조형의 기본값인 null보다 **빈 문자열로 초기화** 하고, char형은 기본값인 '\u0000'보다 **공백으로 초기화** 한다.
```java
String s = "";   // 빈 문자열로 초기화
char c = '';   // 공백으로 초기화
```
<br>

#### join()과 StringJoiner()
**join()**
- join은 특정한 구분자를 문자열 사이사이에 넣어 하나로 결합하는 메서드이다.
```java
String a = "a";
String b = "b";
String c = "c";
System.out.println(String.join("-", a,b,c));
//a-b-c
```
배열로 만든 뒤 배열을 넣어도 가능하다.
```java
String[] arr = {"a", "b", "c"};
System.out.println(String.join("-", arr));
//a-b-c
```
<br>

**StringJoiner() 메서드**
- join과 비슷하지만, 양쪽 끝을 지정해줄 수 있다.
```java
String[] arr = {"a", "b", "c"};
StringJoiner stringJoiner = new StringJoiner(",", "[", "]");

for (String s : arr) {
    stringJoiner.add(s);
}

System.out.println(stringJoiner);
//[a,b,c]
```
<br>

#### 문자열과 기본형 간의 변환
- `valueOf()` : Wrapper Object인 Integer를 반환
- `parseInt()` : Primitive type인 int형을 반환
- 두 함수 모두 문자열을 숫자로 바꿔준다.
- 차이점은 반환값의 타입이 다르다는 것이다.
<br>

| 기본형 -> 문자열                       | 문자열->기본형                               |
|----------------------------------|----------------------------------------|
| String String.valueOf(boolean b) | boolean Boolean.parseBoolean(String s) |
| String String.valueOf(char c)    | byte Byte.parseByte(String s)          |
| String String.valueOf(int i)     | SHort Short.parseShort(String s)       |
| String String.valueOf(long l)    | int Integer.parseInt(String s)         |
| String String.valueOf(float f)   | long Long.parseLong(String s)          |
| String String.valueOf(double d)  | float Float.parseFloat(String s)       |
|                                  | double Double.parseDouble(String s)    |
<br>

## StringBuffer클래스
#### java.lang.StringBuffer 클래스
- String 클래스의 인스턴스는 한 번 생성되면 그 값을 읽기만 할 수 있고, 변경할 수는 없다.
- 하지만 StringBuffer 클래스의 인스턴스는 그 값을 변경할 수도 있고, 추가할 수도 있다.
- 이를 위해 StringBuffer 클래스는 내부적으로 버퍼(buffer)라고 하는 독립적인 공간을 가진다.
- 버퍼 크기의 기본값은 16개의 문자를 저장할 수 있는 크기이며, 생성자를 통해 그 크기를 별도로 설정할 수도 있다.
- 하지만 인스턴스 생성 시 사용자가 설정한 크기보다 언제나 16개의 문자를 더 저장할 수 있도록 여유 있는 크기로 생성된다.

#### 불변 클래스(immutable class)
- String 클래스와 같이 인스턴스가 한 번 생성되면 그 값을 변경할 수 없는 클래스 
- String 클래스와 같은 불변 클래스는 StringBuffer 클래스의 append()나 insert() 메소드와 같이 값을 변경하는 set 메소드를 포함하지 않는다.

#### append() 메서드
- String과 달리 StringBuffer는 내용을 변경할 수 있다.
- append() 메소드는 인수로 전달된 값을 문자열로 변환한 후, 해당 문자열의 마지막에 추가한다.
```java
StringBuffer str = new StringBuffer("Java");
System.out.println("원본 문자열 : " + str);

System.out.println(str.append("수업"));
System.out.println("append() 메소드 호출 후 원본 문자열 : " + str);

// 실행결과
// 원본 문자열 : Java
// Java수업
// append() 메소드 호출 후 원본 문자열 : Java수업
```
<br>

## StringBuffer와 StringBuilder
- StringBuffer/StringBuilder는 String과 다르게 동작한다.
- 문자열 연산 등으로 기존 객체의 공간이 부족하게 되는 경우, 기존의 버퍼 크기를 늘리며 유연하게 동작합니다. StringBuffer와 StringBuilder 클래스가 제공하는 메서드는 서로 동일하다.
<br>

**그럼 두 클래스의 차이점은 무엇일까? 바로 `동기화 여부이`다.**
- StringBuffer는 각 메서드별로 Synchronized Keyword가 존재하여, 멀티스레드 환경에서도 동기화를 지원한다.
- 반면, StringBuilder는 동기화를 보장하지 않는다.
- 그렇기때문에 멀티스레드 환경이라면 값 동기화 보장을 위해 StringBuffer를 사용하고, 단일스레드 환경이라면 StringBuilder를 사용하는 것이 좋습니다. 단일 스레드환경에서 StringBuffer를 사용한다고 문제가 되는 것은 아니지만, 동기화 관련 처리로 인해 StringBuilder에 비해 성능이 좋지 않다.
<br>

사용되는 상황
- String은 짧은 문자열을 더할 경우 사용 
- StringBuffer는 스레드에 안전한 프로그램이 필요할 때나, 개발 중인 시스템의 부분이 스레드에 안전한지 모를 경우 사용
- StringBuilder는 스레드에 안전한지 여부가 전혀 관계 없는 프로그램을 개발할 때 사용

## 래퍼 클래스(Wrapper class)
- 프로그램에 따라 기본 타입의 데이터를 객체로 취급해야 하는 경우가 있다. 예를 들어, 메소드의 인수로 객체 타입만이 요구되면, 기본 타입의 데이터를 그대로 사용할 수는 없다. 이때에는 기본 타입의 데이터를 먼저 객체로 변환한 후 작업을 수행해야 한다.
- 이렇게 8개의 **기본 타입에 해당하는 데이터를 객체로 포장해 주는 클래스**를 래퍼 클래스(Wrapper class)라고 한다.
- 래퍼 클래스는 각각의 타입에 해당하는 데이터를 인수로 전달받아, **해당 값을 가지는 객체로 만들어 준다.**
- 이러한 래퍼 클래스는 모두 java.lang 패키지에 포함되어 제공된다.
- 자바의 기본 타입에 대응하여 제공하고 있는 래퍼 클래스는 다음과 같다.
<br>

|기본 타입|래퍼 클래스|
|------|---|
|byte|Byte|
|short|Short|
|int|**Integer**|
|long|Long|
|int|Integer|
|float|Float|
|double|Double|
|char|**Character**|
|boolean|Boolean|

#### 박싱(Boxing)과 언박싱(UnBoxing)
- 래퍼 클래스(Wrapper class)는 산술 연산을 위해 정의된 클래스가 아니므로, 인스턴스에 저장된 값을 변경할 수 없다.
- 단지, 값을 참조하기 위해 새로운 인스턴스를 생성하고, 생성된 인스턴스의 값만을 참조할 수 있다.
- `박싱(Boxing)` : 기본 타입의 데이터를 래퍼 클래스의 인스턴스로 변환하는 과정
- `언박싱(UnBoxing)` : 반면 래퍼 클래스의 인스턴스에 저장된 값을 다시 기본 타입의 데이터로 꺼내는 과정
<img src = "http://www.tcpschool.com/lectures/img_java_boxing_unboxing.png">

#### 오토 박싱(AutoBoxing)과 오토 언박싱(AutoUnBoxing)
- JDK 1.5부터는 박싱과 언박싱이 필요한 상황에서 자바 컴파일러가 이를 **자동으로 처리해 준다.** 
- 이렇게 자동화된 박싱과 언박싱을 `오토 박싱(AutoBoxing)`과 `오토 언박싱(AutoUnBoxing)`이라고 부른다.
- 다음 예제는 박싱과 언박싱, 오토 박싱과 오토 언박싱의 차이를 보여주는 예제이다.
```java
Integer num = new Integer(17); // 박싱
int n = num.intValue();        // 언박싱
System.out.println(n);

Character ch = 'X';   // Character ch = new Character('X'); : 오토박싱
char c = ch;        // char c = ch.charValue();           : 오토언박싱
System.out.println(c);

// 실행결과
// 17
// X
```
- 래퍼 클래스도 객체이므로 동등 연산자(==)를 사용하게 되면, 두 인스턴스의 값을 비교하는 것이 아니라 두 인스턴스의 주소값을 비교하게 된다.
- 따라서 서로 다른 두 인스턴스를 동등 연산자(==)로 비교하게 되면, 언제나 false 값을 반환되게 된다.
- 그러므로 인스턴스에 저장된 값의 동등 여부를 정확히 판단하려면 equals() 메소드를 사용해야만 한다.
