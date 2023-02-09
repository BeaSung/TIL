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
- Box<T> : 제네릭 클래스. 'T의 Box' / "T Box'라고 읽는다.
- T : 타입 변수 / 타입 매개변수.(T는 타입 문자)
- Box : 원시 타입(raw type)
```Java
Box<String> b = new Box<String>():
```
