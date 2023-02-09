## 제네릭스(Gnerics)란?
- 컴파일 시 타입을 체크해주는 기능(compile-time type check) - JDK 1.5
- Error : `ClassCastException` -> 형변환 에러
- 장점
  - **타입 안정성**을 제공한다.
  - **타입체크**와 **형변환을 생략**할 수 있으므로 코드가 간결해진다.
- class안에 Object 타입을 쓰는 곳은 제네릭스로 전환됨
- **Object쓰면 레거시 처럼 여러 객체 사용 가능**
```Java
//Tv객체만 저장할 수 있는 ArrayList를 생성
ArrayList<Tv> tvList = new ArrayList<Tv>();

tvList.add(new Tv());      // OK.
tvList.add(new Audio());   // 컴파일 에러. Tv 외에 다른 타입은 저장 불가
```
<br>


#### 타입변수
- 제네릭 클래스 작성 시 **Object타입 대신 타입 변수(E)** 를 선언해서 사용
- 타입변수 대입
  - 실제 객체를 타입변수에 넣기, 참조변수와 생성자에 넣어줘야 하며 일치해야 함

#### 제네릭스 용어
```Java
class Box<T> {}
```
- Box<T> : 제네릭 클래스. 'T의 Box' / "T Box'라고 읽는다.
- T : 타입 변수 / 타입 매개변수.(T는 타입 문자)
- Box : 원시 타입(raw type)
<br>

```Java
Box<String> b = new Box<String>():
```
- String : 대입된 타입(매개변수화된 타입, parameterized type)
- Box<String> : 지네릭 타입 호출
<br>

- 참조변수와 생성자의 대입된 타입(타입 매개변수)은 일치(상속관계여도 안됨. 무조건 일치해야함)
- 지네릭 ‘클래스’간의 다형성은 성립(다형성: 조상 클래스의 참조변수로 자손 클래스의 객체를 다룰수 있다)
  ```Java
  List<Tv> tvList = new ArrayList<Tv>();  // OK. 다형성
  ```
- 매개변수의 다형성도 성립.
<br>


## Iterator<E>
- 클래스를 작성할 때, Object타입 대신 **T**와 같은 타입 변수를 사용
```Java
public interface Iterator<E> {
  boolean hasNext();
  E next();
  void remove();
```

## HashMap<k,v>
- 여러 개의 타입 변수가 필요한 경우, **콤마(,)**를 구분자로 선언
```Java
HashMap<String, Student> map = new HashMap<String, Student>();  //생성
map.put("자바왕, new Student("자바왕,1,1,100,100,100));  // 데이터 저장
```
<br>


## 제한된 제네릭 클래스
- extends로 대입할 수 있는 클래스 제한
  ```Java
  class FruitBox<T extends Fruit> { ... // Fruit의 자손만 타입으로 대입 가능  
    ArrayList<T> List = new ArrayList<T>();
    ...
  }
  FruitBox<Apple> appleBox = new FruitBox<Apple>();  // OK.
  FruitBox<Toy> toyBox = new FruitBox<Toy>();  // 에러. Toy는 Fruit의 자손이 아님.
  ```
- **interface인 경우에도 extends 사용**
  ```Java
  interface Eatable {}
  class FruitBox<T extends Eatable> { ... }
  ```
- interface와 class 같이 제한된 지네릭으로 쓸 경우 : 
  ```Java
  class FruitBox<T extends Fruit & Eatable> extends Box<T> {}
  ```
<br>


## 제네릭스의 제약
- 타입 변수의 대입은 인스턴스 별로 다르게 가능
  ```Java
  Box<Apple> appleBox = new Box<Apple>();  // OK. Apple객체만 저장 가능
  Box<Grape> grapeBox = new Box<Grape>();  // OK. Grape객체만 저장 가능
  ```
- **static 멤버**에 타입 변수 **사용 불가** -> 모든 인스턴스에 공통
  ```Java
  class Box<T> {
    static T item; // 에러
    static int compare(T t1, T t2) { ... }  // 에러
  ```
- 배열 생성할 때 타입 변수 사용 불가. 타입 변수로 배열 선언은 가능 
  -> new T(불가 x) - 객체생성 x, 배열생성 x(타입이 확정적이기 못하기 때문에)
  ```Java
  class Box<T> {
    T[] itemArr;  // OK. T타입의 배열을 위한 참조변수
      ...
    T[] toArray() {
       T[] tmpArr = new T[itemArr.length];  // 에러. 제네릭 배열 생성불가
      ...
  ```

