## 객체지향 프로그래밍 I

## 목차
[1. 객체지향 언어](#객체지향-언어)

[2. 클래스와 객체](#클래스와-객체)

[3. 변수](#변수)

[4. 메서드란?](#메서드란?)


## 객체지향 언어

1. 코드의 재사용성이 높다.
    2. 새로운 코드를 작성할 때 기존의 코드를 이용하여 쉽게 작성할 수 있다.
2. 코드의 관리가 용이하다.
    3. 코드간의 관계를 이용해서 적은 노력으로 쉽게 코드를 변경할 수 있다.
3. 신뢰성이 높은 프로그래밍을 가능하게 한다.
    4. 제어자와 메서드를 이용해서 데이터를 보호하고 올바른 값을 유지하도록 하며,
       코드의 중복을 제거하여 코드의 불일치로인한 오동작을 방지할 수 있다.

## 클래스와 객체

> **객체의 정의** : 실제로 존재하는것. 사물 또는 개념 <br>
**객체의 용도** : 객체가 가지고 있는 기능과 속성에 따라 다름 <br>
**유형의 객체** : 책상, 의자, 자동차, TV와 같은 사물 <br>
**무형의 객체** : 수학공식, 프로그램 에러와 같은 논리나 개념

<br>

#### 객체의 구성요소

객체는 `속성`과 `기능`의 집합이다. <br>
> 속성(property) -> 멤버변수(variable) <br>
> 기능(function) -> 메서드(method) <br><br>
> 채널 -> int channel <br>
> 채널높이기 -> channelUp() {...} <br>

<br>

TV 예시)

- **속성** : 크기, 길이, 높이, 색상, 볼륨, 채널 등
- **기능** : 켜기, 끄기, 볼륨 높이기, 볼륨 낮추기, 채널 변경하기 등

```java
class TV {
    // 변수
    String color; // 색깔
    boolean power; //전원상태
    int channel; //채널

    //메서드
    void power() {
        this.power = !power;
    }

    void channelUp() {
        this.channel++;
    }

    void channelDown() {
        this.channel--;
    }
}
```

<br>

#### 객체와 인스턴스

클래스로부터 객체를 만드는 과정을 클래스의 인스턴스화(instantiate)라고 하며, `어떤 클래스로부터 만들어진 객체`를 그 클래스의 `인스턴스(instance)`라고 한다.

<br>

#### 클래스

> **클래스의 정의** : 객체를 정의해 놓은 것 <br>
**클래스의 용도** : 객체를 생성하는데 사용

사물 하나 하나를 기능별로 묶어서 사용하는 것. <br>
각 클래스 안에서 역할에 따라 각 클래스의 기능을 서술해 나간다.
사람에 따라, 같은 프로그램을 만들더라도, 여러가지 관점으로 만들 수 있다.
자바에서는 어떤 프로그래밍이든 클래스 안에 속해있다.<br>
(public static void main(String[] args) 기능도 class안에 속해 있다)
다른 클래스에서 다른 클래스를 선언 하여 사용할 수 있다.

```java
public class 클래스이름 {
    public static void main(String[] args) {
        //프로그램 시작 시점
    }
}
```

<br>

#### 클래스의 구조

- **멤버변수**
    - 클래스 안의 기능을 끄집어 내서 사용할 때 사용.
    - 저장할 공간.
    - (ex. 화면에 출력 시 사용했던 System.out.println()에서 out은 System이라는 클래스의 out이라는 멤버변수를 사용한 것이다.)
- **메서드(method)**
    - 기능을 나타낸다.
    - ( )가 항상 붙어있다.
    - (ex. System.out.println에서 println()이다.)
- **생성자(constructor)**
    - 처음에 값을 넣어줄 때 사용(default 값)

```java
public class 클래스이름 {

    // 멤버 변수
    int a;
    int b;

    // 생성자
    클래스이름() {
        a = 10;
        b = 15;
    }

    // 메서드
    public static void main(String[] args) {
        //프로그램 시작 시점 
    }
}
```

**Tip)** 멤버 변수에 static을 붙여줄 경우, 메모리를 정적으로 사용하겠다는 의미(메모리 주소 위치가 바뀌지 않는다)

```java
static int a=10;
```

<br>

#### 클래스 사용해보기

Person이라는 클래스 만들어 메인 클래스에서 사용해보기.<br>
사람은 뼈, 혈관 등 엄청나게 복잡하고 많은 기능들을 가지고 있지만, 우리가 필요한 기능만 단순화 해서 사용 -> `추상화`

```java
package practice1;

public class Practice {

    public static void main(String[] args) {
        Person bea = new Person(); // Person이라는 타입으로 메모리 요청. 이 구문이 실행시 Person 클래스의 생성자가 실행됨

        Person olivia = new Person("olivia", 35); // 매개변수가 있는 생성자가 호출됨.

        olivia.hello(); // Person 클래스의 hello() 메소드가 실행됨(1번 메소드)
        olivia.hello("bea"); // 2번 메소드
        System.out.println(olivia.hello1()); // 3번 메소드
        System.out.println(olivia.hello1("bea")); // 4번 메소드
    }
}
```

```java
package practice1;

public class Person {

    // 멤버 변수
    int age;
    String name;

    // 메서드(1번 경우)
    public void hello() {
        System.out.println("hello! my name is " + this.name);
    }

    // 메서드(2번 경우)
    public void hello(String to) {
        System.out.println("hello! " + to + " my name is " + this.name);
    }

    // 리턴 값이 있는 메서드(3번 경우)
    public String hello1() {

        String hello = "hello! my name is " + this.name;
        return hello;
    }

    // 리턴 값이 있는 메서드(4번 경우)
    public String hello1(String to) {

        String hello = "hello! " + to + " my name is " + this.name;
        return hello;
    }

    // 기본 생성자
    Person() {
        name = "bea";
        age = 32;
    }

    // 생성자 인자(매개변수)가 존재하는 경우
    Person(String name1, int age1) {
        name = name1;
        age = age1;
    }
}
```

**Tip)** 생성자를 여러개 둘 수 있다. 클래스 선언 시, 항상 default 값으로 클래스 변수를 선언하지 않고, 다른 값이 필요한 경우가 있기 때문.
<br>

**Tip)** this를 통해 멤버 변수끼리 서로 참조를 할 수 있다.(멤버 변수를 static으로 선언하지 않아도, 메소드 안에서 참조 할 수 있다.) this사용시, 클래스의 멤버 변수를 사용한다는 것을
나타낸다.(메소드 안에 멤버변수와 똑같은 이름의 변수가 존재할 경우, this를 통해 이 변수가 클래스의 멤버 변수인지 아니면 메소드 안의 일반 변수인지를 구분)

**Tip)** 메소드의 4가지 형태

1. 화면에 출력만 하고 끝나는 기능 (ex. hello())
2. 매개변수를 받는 경우 (ex. hello(String to))
3. return 값을 돌려주는 경우 (ex. hello1()) <br>
   메소드 타입과 리턴 값의 타입을 맞춰줘야함
4. 매개변수를 받아서 return 하는 경우 (ex. hello1(String to))

## 변수

#### 변수의 종류
변수는 클래스 변수, 인스턴스 변수, 지역변수 모두 세 종류가 있다. <br>
변수의 종류를 파악하기 위해서는 `변수가 어느 영역에 선언되었는지`를 확인하는 것이 중요하다.

```java
class Variables {
    // 클래스 영역
    int iv;          // 인스턴스 변수
    static int cv;   // 클래스 변수(static변수, 공유변수)

    void method()
    // 메서드 영역
    {
        int lv = 0;  // 지역변수
    }
}
```
| 변수의 종류                     | 선언위치                                    | 생성시기            |
|----------------------------|-----------------------------------------|-----------------|
| 클래스 변수<br/>(class variable)     | 클래스 영역                                  | 클래스가 메모리에 올라갈 때 |
| 인스턴스 변수<br/>(instance variable) | 클래스 영역                                  | 인스턴스가 생성되었을 때   |
| 지역변수<br/>(local variable)       | 클래스 영역 이외의 영역<br/>(메서드, 생성자, 초기화 블럭 내부) | 변수 선언문이 수행되었을 때 |

<br>

#### 클래스 변수와 인스턴스 변수
클래스 변수와 인스턴스 변수의차이를 이해하기 위한 예로 카드 게임에 사용되는 카드를 클래스로 정의해보자.
```java
class Card {
    // 인스턴스 변수(개별 속성)
    String kind;   // 무늬
    int number;    // 숫자

    // 클래스 변수(공통 속성)
    static int width = 100;    // 폭
    static int height = 250;   // 높이
}
```
인스턴스 변수는 인스턴스가 생성될 때 마다 생성되므로 인스턴스마다 각기 다른 값을 유지할 수 있지만, `클래스 변수는 모든 인스턴스가 하나의 저장공간을 공유하므로, 항상 공통된 값을 갖는다.`

## 메서드란?
'메서드(method)'는 특정 작업을 수행하는 일련의 문장들을 하나로 묶은 것이다. 
#### 메서드의 구성
- 선언부(header, 머리)
  - 반환타입 메서드 이름 (타입 변수명, 타입 변수명, ...)
- 구현부(body, 몸통)
  - { // 메서드 호출시 수행될 코드 }
```java
class method {
    // 선언부
    int add(int a, int b) {
        
        // 구현부
        int result = a+b;
        return result;  //호출한 메서드로 결과를 반환한다.
    }
}
```

<br>

#### 메서드의 선언부
선언부의 구성요소
1. 메서드의 이름 
2. 매개변수 선언 
3. 반환타입

```java
class method{
    // int : 반환타입(출력)
    // add : 메서드 이름
    // int x, int y : 매개변수 선언(입력)
    int add(int x, int y) {
        int result = x + y;
        
        return result;  // 결과를 반환
    }
}
```

<br>

#### 메서드의 구현부
메서드의 선언부 다음에 오는 괄호{}를 `메서드의 구현부`라고 하는데, 여기에 메서드를 호출했을 때 수행될 문장들을 넣는다.
<br>

**return문** <br>
```java
class Return {
    // 'int'와 'result'의 타입이 일치해야 한다.
    int add(int x, int y) {
        int result = x + y;
        return result;  // 작업 결과(반환값)를 반환한다.
    }
}
```
이 문장은 작업을 수행한 결과인 반환값을 호출한 메서드로 전달하는데, 이 값의 타입은 `반환타입과 일치하거나 적어도 자동 형변환이 가능한 것`이어야 한다.
```java
class Return {
    void printGugudan(int dan) {
        for (int i = 0; i <= 9; i++) {
            System.out.printf("%d * %d = %d%n", dan, i, dan * i);
        }
        return;     // 반환 타입이 void이므로 생략가능. 컴파일러가 자동추가
    }
}
```
반환타입이 void인 경우, return문 없이도 아무런 문제가 없었던 이유는 컴파일러가 메서드의 마지막에 'return'을 자동적으로 추가해주었기 때문이다.

#### 기본형 매개변수
> **기본형 매개변수** : 변수의 값을 읽기만 할 수 있다.(read only)<br>
> **참조형 매개변수** : 변수의 값을 읽고 변경할 수 있다.(read & write)

###### Call by value vs Call by reference
- Java는 항상 Call by value이다.
- 기본형 매개변수는 `그 값 자체를 받기 때문에` 읽을 수만 있고
- 참조형 매개변수는 `레퍼런스 주소를 값으로 전달해주기 때문에` Call by value임에도 불구하고 해당 레퍼런스에 저장되어 있는 인스턴스에 접근하여 값을 수정할 수도 있다.
