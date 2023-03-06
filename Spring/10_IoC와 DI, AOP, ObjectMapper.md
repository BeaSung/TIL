## IoC와 DI ( 객체 주입 )
### Ioc

<img src="https://velog.velcdn.com/images/hj_/post/c0d1ba03-8bbc-464f-9a30-2f56c5f82d6b/image.PNG">

- **IoC** : 프로그램의 흐름이 개발자의 코드가 아닌 외부에 의해 제어되는 개념
- **IoC Container**
  - 핵심 객체를 관리하는 창고 
  - Controller, Service, Repository 등의 다양한 객체들을 만들고 관리 
  - IoC에 담긴 객체들은 필요에 따라 또 다른 객체들로 주입될 수 있음 
  - 이러한 객체 생성과 관리, 주입은 개발자의 코드가 아닌 IoC 컨테이너에 의해 통제

### DI
- **DI** : 의존성 주입하기
- 필요한 객체를 외부에서 또 다른 객체로 꽂아주는(주입하는) 방식
- 동작에 필요한 객체를 외부에서 받아옴

### 관련 어노테이션
- `@SpringBootTest` : IoC Container를 테스트에서 사용하기 위해 사용
- `@Autowired` : IoC Container가 객체를 DI 하도록 함
  - new 를 이용하여 직접 객체를 생성하지 않아도 됨
- `@Component` : 해당 클래스를 객체로 만들고, 이를 IoC Container에 등록하도록 함




<br>
<br>

### 레퍼런스
- [스프링 강의](https://youtu.be/5d3wEN0_MLY "홍팍의 스프링 강의")

