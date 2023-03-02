## 1. 데이터 조회하기 with JPA

### 데이터 조회 과정
1. 사용자가 브라우저를 통해 요청 
2. 요청 url을 Controller가 받아서 찾고자 하는 데이터의 정보를 Repository에게 전달 
3. Repository는 DB에게 요청해서 데이터를 받아옴 
4. DB는 해당 데이터를 찾아서 Entity로 반환 
5. 이 Entity는 Model을 통해 View template로 전달 
6. 결과 페이지가 완성되어 클라이언트에게 전달됨

<br>

### 데이터 조회 수행
데이터를 조회하기 위해서는 URL을 입력해야 하는데, 아래와 같이 입력하여 데이터를 가져온다.

localhost:8080/articls/1 이런식으로 요청 했을때 1번 값을 반환 하도록 작성한다.

<br> 

### 적용하기

<img src="https://velog.velcdn.com/images/hj_/post/b24d334a-bfe6-4561-8bc1-077cb23ca5f3/image.PNG">

```java
@GetMapping("/articles/{id}")   // {id}로 명시하면 이 id는 변하는 값임을 의미
    public String show(@PathVariable Long id, Model model) {
        log.info("id = " + id);

        // 1. id로 DB에서 데이터를 가져옴 (Entity로)
        Article articleEntity =  articleEntity = articleRepository.findById(id).orElse(null);

        // 2. 가져온 데이터를 모델에 등록
        model.addAttribute("article", articleEntity);

        // 3. 보여줄 페이지를 설정
        // articles/show에서 article이라는 Model을 사용 가능
        return "articles/show";
```

<br>

#### (Controller에서) 요청 url 받기
- `@GetMapping("/articles/{id}")`: {id}로 명시하면 id는 변하는 값임을 의미
- `@PathVariable`: 값이 url 주소로부터 입력이 된다는 것을 의미, 파라미터에 작성

#### id로 데이터 찾기
- 데이터를 가져오는 주체 : Repository 
- `findById()`는 CrudRepository가 제공하는 함수
- `findById()`로 값을 반환할 때, return type은 `Optional<Article>`
  - 강의에서 이 방식 말고 아래 방식을 사용 
- `Article articleEntity = articleEntity = articleRepository.findById(id).orElse(null);`
  - id 값을 통해 데이터를 조회
  - `.orElse(null)` : 해당 id값이 없다면 null을 반환
  - 데이터는 Entity 객체에 저장

<br>

### Model에 데이터(Entity) 등록하기
- 메소드에 Model을 파라미터로 추가 
- `model.addAttribute()`를 이용해서 Model에 DB에서 받아온 데이터가 저장된 Entity를 등록

<br>

### 뷰 페이지 만들기
```java
<show.mustache>

   <!-- model에 등록된 데이터는 #attributeName 으로 선언 후 사용 가능 -->
   {{#article}}
       <tr>
           <!-- model에 등록된 데이터 -->
           <th>{{id}}</th>
           <td>{{title}}</td>
           <td>{{content}}</td>
       </tr>
   {{/article}}
```
- return "파일명"

<br>
<br>

## 2. 데이터 목록 조회
### 적용하기

<img src="https://velog.velcdn.com/images/hj_/post/05a2690a-f81b-4262-96e0-73d3908d7cfe/image.PNG">

```java
@GetMapping("/articles")
    public String index(Model model) {

        // 1. 모든 Article을 가져온다
        ArrayList<Article> articleEntityList = articleRepository.findAll();

        // 2. 가져온 Article 리스트를 view로 전달 (Model을 사용)
        model.addAttribute("articleList", articleEntityList);

        // 3. view 페이지를 설정
        return "articles/index";
    }
```

<br>

### 모든 Article 가져오기
```java
List<Article> articleEntityList = articleRepository.findAll();
```

- DB에서 데이터를 가져오는건 repository가 수행 
- 위의 코드는 에러가 존재 
- findAll()은 Iterable을 반환하는데 `List<Article>` 로 받으려고 하기 때문
- 해결방법
  - `List<Article> articleEntityList = (List<Article>) articleRepository.findAll();`
  - `Iterable<Article> articleEntityList = articleRepository.findAll();`
  - `@Override ArrayList<Article> findAll();`: ArticleRepository에서 findAll()을 오버라이딩

<br>

### 가져온 Article을 model을 통해 view로 전달
- `model.addAttribute("articleList", articleEntityList);`

<br>
<br>

### 레퍼런스
- [스프링 강의](https://www.youtube.com/watch?v=E0YO0XqpBIY&list=PLyebPLlVYXCiYdYaWRKgCqvnCFrLEANXt&index=15 "홍팍의 스프링 강의")


