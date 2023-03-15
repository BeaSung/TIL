## RestAPI와 JSON
- 클라이언트는 브라우저 뿐만 아니라 스마트폰, 스마트워치, 테블릿 등 다양하게 존재 
- 웹 서버는 모든 클라이언트의 요청에 적절한 응답을 해야한다.

    --> RestAPI를 사용

### RestAPI
- 웹 서버의 자원을 클라이언트에 구애받지 않고 사용할 수 있게 하는 설계 방식 
- HTTP를 통해 서버의 자원을 다루게 하는 기술
- 서버의 응답은 특정 기기에 종속되지 않도록, 즉 모든 기기에서 통용될 수 있는 (화면이 아닌) 데이터를 반환 
- 반환하는 응답데이터는 과거에는 xml 형식으로 사용되었지만 최근에는 JSON 형식을 사용 
- 즉, 요청을 보내면 JSON 형식으로 데이터를 반환

### 전송 방식 ( Method )
- `GET`: 데이터 가져오기 
- `POST`: 데이터 전송하기 
- `PATCH(PUT)`: 데이터 수정 
- `DELETE`: 데이터 삭제

### 전송 형식
- `xml`: html처럼 태그 형식을 사용 
- `JSON`: 자바스크립트의 객체(object) 표기법을 사용 
- `YAML`: Python 스타일의 들여쓰기 방법으로 중첩을 표시

### HTTP 요청과 응답 분석
- JSON placeholder, Talend API 사용 
- API 요청을 보내서 응답으로 JSON 데이터를 받아볼 수 있음

<br>

## HTTP와 RestController
<img src="https://velog.velcdn.com/images/hj_/post/f6b6d574-1301-459e-92d0-ae1968fb7771/image.PNG">

- `RestController`: 요청을 받아 JSON으로 반환해주는 Controller
- `ResponseEntity 클래스`: 적절한 상태 코드 반환을 위해 사용

### 사용 방법
- 기본패키지/api에 Controller 클래스 생성 
- `@RestController 선언`: Rest API용 Controller, JSON 반환

### Controller 와 차이점
- 일반 Controller는 view page template을 반환 
- RestController는 데이터를 JSON 형식으로 반환해야함

### 유의점
- 확인은 Talend API에서 url 입력 후 확인 가능 
- url 입력 시 http:// 로 입력해야함 
- GET, POST, PATCH, DELETE 사용 가능 
- 각각 @GetMapping, @PostMapping, @PatchMapping, @DeleteMapping 사용 
- 요청 방식이 다르면 같은 url(uri?) 사용 가능

#### GET 방식
- 반환 제외하고 일반 Controller와 차이 없음

#### POST 방식
- `@RequestBody`: JSON 데이터 받기, 데이터를 받을 때 선언 필요 
- 일반 Controller 처럼 DTO 객체로 받고, Entity로 변환, Repository를 통해 DB에 저장

#### PATCH 방식
- 메소드의 반환형 : `ResponseEntity<Article>`
- 위처럼 하면 상태 코드와 body를 같이 return 할 수 있다 
- `return ResponseEntity.status(HttpStatus.OK).body(updated); `
- 데이터 수정 시 모든 데이터를 보내지 않아도 수정 가능하도록 하기 위한 메소드가 Entity 클래스에 추가로 필요

#### DELETE 방식
- PATCH 방식과 유사

#### 전체 코드
```java
<ArticleApiController>

@Slf4j
@RestController
public class ArticleApiController {

    @Autowired  // DI : 외부에서 가져온다
    private ArticleRepository articleRepository;
    // GET
    
    // 모든 article을 받기
    @GetMapping("/api/articles")
    public List<Article> index() {
        return articleRepository.findAll();
    }

    @GetMapping("/api/articles/{id}")
    public Article show(@PathVariable Long id) {
        return articleRepository.findById(id).orElse(null);
    }

    // POST
    @PostMapping("/api/articles")
    public Article create(@RequestBody ArticleForm dto) {

        Article article = dto.toEntity();

        return articleRepository.save(article);
    }

    // PATCH
    @PatchMapping("/api/articles/{id}")
    public ResponseEntity<Article> update(@PathVariable Long id, @RequestBody ArticleForm dto) {

        // 1. 수정용 Entity 생성
        Article article = dto.toEntity();
        log.info(article.toString());

        // 2. 대상 Entity 조회
        Article target = articleRepository.findById(id).orElse(null);

        // 3. 잘못된 요청 처리 (대상이 없거나, id가 다른 경우)
        if(target == null || id != article.getId()) {
            // 400, 잘못된 요청 응답
            log.info("잘못된 요청", article.toString());
            
            // 상태는 BAD_REQUEST, body에는 아무것도 없도록
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(null);
        }

        // 4. 업데이트 및 정상 응답(200)

        // 수정할 데이터만 받아도 (전체 데이터를 받지 않아도) 수정되도록 하는 메소드 (직접 작성한 메소드)
        target.patch(article);
        Article updated = articleRepository.save(target);
        return ResponseEntity.status(HttpStatus.OK).body(updated);
    }

    // DELETE
    @DeleteMapping("/api/articles/{id}")
    public ResponseEntity<Article> delete(@PathVariable Long id) {

        // 대상 찾기
        Article target = articleRepository.findById(id).orElse(null);

        // 잘못된 요청 처리
        if (target == null) {
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(null);
        }

        // 대상 삭제
        articleRepository.delete(target);

        // 데이터 반환
        return ResponseEntity.status(HttpStatus.OK).build();
    }
}
```

<br>
<br>

### 레퍼런스
- [스프링 강의](https://youtu.be/Q_B5c6eChDo "홍팍의 스프링 강의")

