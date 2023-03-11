# 생성자 대신 정적 팩토리 메서드를 고려하라

### 일반적으로 사용하는 public 생성자 대신, 별도로 **정적 팩토리 메소드**를 이용할 수 있다.

예제 코드
````java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Booelan.FALSE;
}
````

<br>

## 장점
### 1. 이름을 가질 수 있다.
생성자에 넘기는 매개변수만으로 반환된 객체의 특성을 제대로 설명하지 못한다.

반면 정적 팩터리 메서드는 이름을 잘 지으면 반환될 객체의 특성을 쉽게 묘사할 수 있다.


### 2. 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.
불변 클래스(immutable class)는 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다.

따라서 객체가 자주 요청되는 상황이라면 성능을 상당히 끌어 올릴 수 있다.

Boolean.valueOf(boolean) 메소드도 그 경우에 해당된다.


### 3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
API를 만들때 유연성을 이용하여 구현 클래스를 공개하지 않고도 그 객체를 반환할 수 있어 API를 작게 유지할 수 있다.


### 4.입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
장점 3에서 말했듯이 하위 타입이기만 하면 어떤 클래스의 객체를 반환하던 상관 없다.

가령 EumSet 클래스는 public 생성자 없이 오직 정적 펙토리 메서드만 제공한다. `nonOf` 등..

nonOf 메서드를 살펴보면 원소 수에 따라 `RegularEnumSet`이나 `JumbEnumSet`인스턴스를 반환한다.
```java
public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
        Enum<?>[] universe = getUniverse(elementType);
        if (universe == null)
            throw new ClassCastException(elementType + " not an enum");

        if (universe.length <= 64)
            return new RegularEnumSet<>(elementType, universe);
        else
            return new JumboEnumSet<>(elementType, universe);
    }
```


### 5.정적 팩토리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
대표적인 프레워크로는 JDBC가 있습니다. 구현체들은 클라이언트에 제공하는 역할을 프레임워크가 통제하여, 클라이언트를 구현체로부터 분리해준다.

서비스 제공자 프레임워크는 **3개의 핵심 컴포넌트**로 이루어진다. (제공자는 서비스의 구현체를 의미)
- 서비스 인터페이스 (구현체의 동작을 정의)
- 제공자 등록 API (제공자가 구현체를 등록할 때 사용)
- 서비스 접근 API (클라이언트가 서비스의 인스터스를 얻을 때 사용)

서비스 접근 API를 사용할 때 원하는 구현체의 조건을 명시할 수 있다. 조건을 명시하지 않으면 기본 구현체를 반화하거나 지원하는 구현체들을 하나씩 돌아가면서 반환한다.

<br>

## 단점
### 1.상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩토리 메서드만 제공하면 하위 클래스를 만들 수 없다.
이 제약은 상속보다(is a) 컴포지션을(has) 사용하도록 유도하고 불변 타입으로 만들려면 이 제약을 지켜야 한다는 점에서 오히려 장점으로 받아들일 수도 있다.

### 2.정적 팩토리 메서드는 프로그래머가 찾기 어렵다.
생성자처럼 API설명에 명확히 드러나지 않으므로 사용자는 정적 팩토리 메서드 사용 방식 클래스를 인스턴스화 시킬 방법을 알아내야한다. 그러므로 API 문서를 잘 정리하고 메서드 이름도 널리 알려진 규약을 따르는 게 좋다.


<br>
<br>

### 레퍼런스
- Bloch, Joshua. (2018). *Effective Java*, 3rd ed. Press.
