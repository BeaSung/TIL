# private 생성자나 열거 타입으로 싱글턴임을 보증하라

## 싱글턴(Singleton) 패턴

#### 싱글턴(Singleton)이 무엇인가?
싱글턴은 어떤 클래스가 최초 한 번만 메모리에 할당하고(Static) 그 메모리에 대해서 객체를 만들어 사용하는 디자인 패턴이다. <br>
생성자 호출이 반복적으로 발생한다고 하더라도, 새로운 인스턴스를 생성하는 것이 아니라 최초 생성된 인스턴스를 반환해주는 것을 말한다.

#### 싱글턴 패턴의 사용 목적
예를 들어 '회사'라는 클래스를 생성하고 '회사명', '회사 위치' 등 회사의 정보성 데이터를 변수로 생성하고 관리하고자 한다면, <br>
정보를 보관하고 공유하고자 하는 클래스(예시의 '회사'클래스)가 한 번의 메모리만 할당하고 그 할당한 메모리에 대해 객체로 관리하기 위함이다.<br>
이렇게 되면 여러 클래스에서 각자 '회사'클래스의 생성자를 호출하더라도 처음 한 번 생성된 인스턴스를 반환해주기 때문에 정보 공유 차원에서의 변수 관리 즉 동기화에 용이하다.

#### 싱글턴 사용 시 조심해야 되는 부분
싱글턴에게 너무 많은 일을 하거나, 많은 데이터를 공유시키면 다른 클래스의 인스턴스 간 결합도가 높아지면서 개방-폐쇄 원칙을 위배하는 문제가 있다.<br>
수정이 어려워지고 테스트하기가 어려워지기도 하고, Multi-Thread에서 동기화 처리를 해주지 않는 경우 인스턴스가 두 개 생성되는 등 Thread-Safe 문제가 발생할 수 있다.

<br>

## 싱글턴을 만드는 방식

### 1. public static 멤버가 final인 방식
```java
public class Elvis {

  public static final Elvis INSTANCE = new Elvis();

  private void Elvis() { ... }

  private void leaveTheBuilding() { ... }
}
```
public이나 protected 생성자가 없으므로 Elvis 클래스가 초기화 될 때 만들어진 인스턴스는 하나 뿐이다.<br> 
단, 권한이 있는 클라이언트는 리플렉션 API인 AccessibleObject.setAccessible을 사용해 private 생성자를 호출할 수 있다.<br>
public 필드 방식의 첫번째 장점은 해당 클래스가 싱글턴임이 API에 명백히 드러나는 것이다.<br>
두 번째 장점은 간결하다는 것입니다. 이 점은 생성자를 수정하여 두 번째 객체가 생성되려고 하면 예외를 던지면 된다.

### 2. 정적 팩토리 방식의 싱글톤
````java
public class Elvis {

  private static final Elvis INSTANCE = new Elvis();

  private void Elvis() { ... }
  public static Elvis getInstance() { 
      return INSTANCE; 
  }

  private void leaveTheBuilding() { ... }
}
````

#### 장점
- API를 바꾸지 않고 싱글턴이 아니게 변경할 수 있다.
- 유일한 인스턴스를 반환하던 팩터리 메서드가 호출하는 스레드별로 다른 인스턴스를 넘겨주게 할 수 있다.
- 정적 팩토리 메서드 참조를 공급자(supplier)로 사용할 수 있다.

위에서 살펴본 두 방법 모두 직렬화하려면 모든 인스턴스 필드를 일시적(transient)이라고 선언하고 readResolve 메서드를 다음과 같이 제공해야 한다.<br>
이렇게 하지 않으면 역직렬화 할때마다 새로운 인스턴스가 생성된다.
```java
private Object readResolve() {
    returm INSTANCE;
}
```

### 3. 열거 타입의 방식의 싱글턴
````java
public enum Elvis {
    INSTANCEl

    public void leaveTheBuilding() { ... }
}
````

직렬화 상황 그리고 리플렉션 공격에서도 싱글턴임을 보장할 수 있다.
**대부분 상황에서는 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법이다.<br>**
하지만 이 방법은 Enum 외의 클래스를 상속해야한다면 사용할 수 없다.(인터페이스를 구현할 수는 있다)
