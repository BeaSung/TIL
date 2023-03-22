# Comparable를 구현할지 고려하라
자바에서는 `Comparable`과 `Comparator`이라는 정렬 인터페이스를 제공한다. <br>
**Comparable**은 **기본 정렬기준을 구현**하는 데 사용하고, **Comparator**는 기본 정렬기준 외에 다른 기준으로 정렬하고자 할 때 사용한다. 여기서는 Comparable의 하나 밖에 없는 `compareTo`메서드에 대해서 알아보자.

Comparable을 구현했다는 것은 그 클래스의 인스턴스들에는 `natural order`가 있음을 의미한다.</br>
그래서 Comparable을 구현한 객체들의 배열은 다음과 같이 정렬할 수 있다.
``` java
Arrays.sort(a);
```

### compareTo 메서드의 규약
이 객체가 주어진 객체(매개변수로 받는)보다 작으면 음의 정수를, 같으면 0을, 크면 양의 정수를 반환한다. </br> 
이 객체와 비교할 수 없는 타입이면 ClassCastException을 던진다.

다음 설명에서는 sgn(표현식) 표기는 수학에서 말하는 부호 함수를 뜻하며, 표현식의 값이 **음수, 0, 양수일 때 -1, 0, 1을 반환**하도록 정의했습니다.
- Comparable을 구현한 클래스는 모든 x, y에 대해 sgn(x.comparaTo(y)) == -sgn(y.comparaTo(x))여야 한다.(따라서 x.compareTo(y))는 y.compareTo(x)가 예외를 던질 때에 한해 예외를 던져야 한다.
- Comparable을 구현한 클래스는 추이성을 보장해야 한다. x.comparaTo(y) > 0 && y.comparaTo(z) > 0이면 x.compareTo(z) > 0이다.
- Comparable을 구현한 클래스는 모든 z에 대해 x.compareTo(y) == 0이면 sgn(x.compareTo(z)) == sgn(y.compareTo(z))이다.
- 이번 권고가 필수는 아니지만 꼭 지키는 게 좋다. (x.compareTo(y) == 0) == (x.equals(y))여야 한다.

hashCode 규약을 지키지 못하면 해시를 사용하는 클래스에서 오작동이 날 수 있듯이 compareTo 규약을 지키지 못하면 **TreeSet, TreeMap**과 같이 비교를 활용하는 클래스에서 오작동이 날 수 있다.

Compareble 인터페이스는 타입 인수를 받는 제네릭 인터페이스므로 compareTo 메서드의 인수 타입은 컴파일타임에 정해진다. <br>
즉, 매개변수 타입이 잘못 됐다면 컴파일 자체가 되지 않으므로 형변환 할 필요가 없다는 뜻이다.</br>
객체 참조 필드를 비교하려면 compareTo 메서드를 재귀적으로 호출한다. **Compareble을 구현하지 않은 필드나 표준이 아닌 순서로 비교해야 한다면 `Comparator`를 사용하면 된다.**

클래스의 핵심 필드가 여러개라면 **가장 핵심적인 필드부터 비교**하는 것을 추천한다. 비교 결과가 0이 아니라면 순서는 거기서 셜정되기 떄문이다.</br>

#### 자바8에서 compareTo 메서드를 구현하기
``` java
    List<Student> students = Arrays.asList(
        new Student(30,  "kim"),
        new Student(50,  "jake"),
        new Student(50,  "foo")
    );
    students.sort(
        Comparator.comparingInt(Student::getAge)
            .thenComparing(Student::getName)
    );
    // 이뿐만 아니라 다양한 방식으로 정렬을 할 수 있습니다.
```