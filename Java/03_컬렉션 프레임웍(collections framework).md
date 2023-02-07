### 컬렉션 프레임웍(collections framework)

- **컬렉션(collection)** : 여러 객체(데이터)를 모아 놓은 것
- **프레임웤(framework)** : 표준화, 정형화된 체계적인 프로그래밍방식, 생산성을 올려줌, 등장 배경을 이해하면 프레임웤을 사용하는 이유를 저절로 알게 됨
- **컬렉션 프레임웍** : 다수의 객체를 다루기 위한 표준화된 프로그래밍 방식 / 컬렉션을 다루는 다양한 클래스를 제공(1.2 이후)
- **라이브러리** : 다른 사람이 이미 만들어놓은 기능. 정보, 책, 오디오 라이브러리 등 -> 기능만 제공
- **컬렉션 클래스** : 다수의 데이터를 저장할 수 있는 클래스
<br>

## 컬렉션 프레임웍의 핵심 인터페이스
1. **`List`**
   - 순서O, 중복O
   - ex. 대기자명단
2. **`Set`**
   - 순서X, 중복X
   - ex. 집합
- 1, 2를 묶어서 **`collection`** 인터페이스로 정의함. 
3. **`Map`**
   - 키와 값의 쌍(서로 관련 데이터 묶음)
   - 순서X, 키(아이디) 중복X, 값(패스워드) 중복O
   - ex. 아이디패스워드 / 아이디는 중복이면 안되지만 패스워드는 괜찮음
<br>

## Collection, List, Set, Map
- **Collection interface의 메서드**
  - add, addAll(추가), clear(전체삭제), contains, containsAll(검색), remove, removeAll(삭제), retainAll(포함된 객체 제외 삭제), size(갯수반환)
<br>

- **`List`** - 순서O, 중복O
  - **ArrayList, LinkedList**
  - 메서드(추가) : get(읽어오기) set(변경하기), indexOf, lastIndexOf(찾기, 방향 차), subList(범위 내 객체반환)
- **`Set`** - 순서X, 중복X
  - **HashSet, TreeSet**
  - 메서드(추가) : addAll(합집합), containsAll(부분집합), removeAll(차집합), retainAll(교집합)
- **`Map`** - 순서X, 중복(키X,값O)
  - **HashMap, TreeMap, LinkedHashMap(순서가 생기는 해시맵)**
  - 메서드(추가) : put, putAll(객체추가), remove(삭제), get, containsKey/Value(검색), entrySet(키+값을 한 세트로 모든 값을 읽어옴), keySet(키만 읽어옴), values(값만 읽어옴), (k,v) <- entry
<br>

- values가 Collections 타입인 이유 : 순서와 중복이 있어도 되고 안되고를 뜻하기도 함.(List와 Set이 Collection의 자손이기 때문에 순서와 중복이 있어도 되고 없어도 되는 모든 것을 허용)
<br>


## ArrayList
- `ArrayList`
  - Vector를 개선한 것. 차이점은 동기화의 유무(Vector는 동기화O, ArrayList는 동기화X)
  - list 인터페이스 구현했기 때문에 **순서/중복 O**
  - 저장 공간으로 배열 사용하기 때문에 배열의 길이를 적절히 지정하는것이 좋음.
  - object[] 즉, 객체 배열이라서 **객체만 저장가능**
  - 예를 들어, 배열에 5라는 값을 넣을 때, `list1.add(new Integer(5))` 대신 `list1.add(5)`라고 써도 된다.
    - **autoboxing에 의해 기본형이 참조형으로 자동 변환**
- `remove()`
  - 해당 인덱스를 삭제, remove(new Integer(1)) -> 배열안의 1이라는 요소를 삭제.
  - 중간에 있는 데이터를 삭제하는 과정
    1. 삭제할 데이터 아래의 데이터를 **한 칸씩 위로 복사해서 삭제할 데이터를 덮어쓴다.**
    2. 데이터가 모두 한 칸씩 이동했으므로 **마지막 데이터는 null로 변경한다.**
    3. 데이터가 삭제되어 데이터의 개수가 줄었으므로 **size 값을 감소시킨다.**
  - 뒤에서 부터 삭제하면 배열 복사 발생하지 않는다. 위에서 부터 지우면 성능상에서도 안좋고, 자잘한 문제 발생
- ArrayList에 저장된 객체를 전부 삭제할 때, list.size()-1로 삭제해야 **배열복사가 안 생김** + 더 빠름.
    ```java
    for(int i = list.size() -1; i >= 0 ; i--)
      list.remove(i);
    ```
<br>


## LinkedList
- 배열의 장단점
  - **장점**
    - 구조가 간단함
    - 데이터를 읽는 데 걸리는 시간이 짧음
    - 순차적 데이터 추가, 삭제가 빠름
  - **단점**
    - 크기변경이 어려움
      - 크기를 변경해야 하는 경우 새로운 배열을 생성 후 데이터를 복사해야 함
      - 크기 변경을 피하기 위해 충분히 큰 배열을 생성하면, **메모리가 낭비됨**
    - 비순차적인 데이터 추가 삭제에 시간 많이 걸림
- 배열의 저장공간이 부족할 때 **새로운 배열을 만드는 순서**
  1. 더 큰 배열을 생성
  2. 기존 내용을 복사
  3. 참조를 변경

- **`LinkedList`**
  - 배열의 단점을 보완 -> 불연속적으로 존재하는 데이터를 연결
    - 데이터 **삭제** : 단 한번의 참조변경만으로 가능
    - 데이터 **추가** : 한 번의 Node객체 생성, 두 번의 참조변경으로 가능
- ArrayList vs LinkedList 비교 -> Array 와 LinkedList DS비교와 동일


## Stack과 Queue
- **`Stack`**
   - 밑이 막힌 상자
   - LIFO(Last In First Out), 마지막에 저장된 것을 제일 먼저 꺼내게 된다.
   - 저장(Push), 추출(pop)
   - 순차적 추가/삭제가 가능한 배열 사용해 구현
- **`Queue`**
   - 양 끝이 뚫린 상자
   - FIFO(First In First Out), 제일 먼저 저장한 것을 제일 먼저 꺼내게 된다.
   - 저장(offer), 추출(poll) 
   - 링크드리스트가 더 적합(삭제 - 추출에 더 유리, 자리이동이 없으므로)
- Stack & Queue에서 **peek** : 맨 위의 저장된 객체 반환 
- **search** : idx 1부터 시작
- Queue에서 **add, remove** : 예외발생o
- Queue에서 **offer, poll** : 예외발생x
- Queue는 interface로 정의 됨 -> 객체 생성 불가 : Queue직접 구현, 구현된 클래스 사용
- 인터페이스 Queue 구현
   1. Queue를 직접 구현 
   2. Queue를 구현한 클랙스 사용 
- Java API - All Known Implementing Classe : Queue 인터페이스를 구현한 추상 메서드
<br>


#### Stack과 Queue의 활용
- **Stack의 활용**
   - 수식 계산, 수식 괄호 검사, 워드프로세서의 undo/redo, 웹 브라우저의 뒤로/앞으로
- **Queue의 활용**
   - 최근 사용 문서(Recent Files), 인쇄 작업 목록, 버퍼
<br>


## Iterator, Enumeration, Map과 Iterator
#### Iterator, ListIterator, Enumeration
- 컬렉션에 저장된 데이터를 읽어오는데 사용되는 인터페이스
1. 확인 - Boolean hasNext() 읽어 올 요소가 남아있는지 확인한다
2. 출력 - Object Next() 다음 요소를 읽어온다.
다음 요소가 없으면 false 리턴.

#### iterator
- 쓰는 이유 
   - 컬렉션 마다 구조가 다른 List, Set, Map을 표준화 한 iterator
   - CF를 종종 바꿔줘야하는 이슈가 있기에 Iterator를 사용
- 사용법
   - iterator를 호출하여 iterator를 구현, 반환한 객체를 얻어 사용
- Iterator는 끝까지 가고나면 **다시 못돌아옴(1회용)** -> 다시 얻어와야 한다
- Map은 Iterator가 없다 : keySet(), entrySet(), values() 사용 (python의 dict와 비슷)
<br>


## Arrays
#### Arrays 클래스
- 배열을 다루기 편리한 메서드 제공 
- 메서드들은 모두 **Static**
- Math, Objects, Collections 등..
- Static 메서드들을 제공하며 유틸 메서드라고도 한다.
<br>

- 배열의 출력 : toString()
- 배열의 복사 : copyOf(), copyOfRange()
- 배열 채우기 : fill(), setAll() 

#### 배열의 정렬과 검색 sort(), binarySearch()
binarySearch()의 이진 탐색은 정렬된 배열에 가능하다 그러므로,
   1. sort() - 배열 arr 정렬
   2. binarySearch() - 탐색

#### 순차 검색과 이진 탐색 
- 순차 검색 - 앞(또는 뒤)에서부터 반복문으로 순서대로 검색
- 이진 탐색 - 배열을 반복해서 반으로 나누어 범위를 줄여가며 비교 탐색한다.
   - 이진 탐색의 단점 : 정렬해서 검색 해야한다.

#### 다차원 배열의 출력 - deepToString()
- {0,1,2,3,4}; - 1차원 배열
- {{11,12}, {21,22}}; - 2차원 배열

#### 다차원 배열의 비교 - deepEquals()
```Java
- String[][] str2D = new String[][] {{“aaa”,“bbb”}, {“AAA”,“BBB”}}:
- String[][] str2D2 = new String[][] {{“aaa”,“bbb”}, {“AAA”,“BBB”}}:

System.out.println(Array.equals(str2D, str2D2)); // false
System.out.println(Array.deepEquals(str2D, str2D2)); // true
```

#### asList()
- 배열을 다루기 편리한 메서드(static) 제공 
- 읽기전용



