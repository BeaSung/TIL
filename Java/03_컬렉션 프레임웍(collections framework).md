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







