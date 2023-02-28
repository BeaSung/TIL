## form 태그 주 속성
데이터 전송을 위한 form 태그의 주요 속성은 `method`와 `action`이다.
```html
<!-- method: HTTP의 요청 방식을 결정.  get/post 가능  -->
<!-- action: 데이터가 전송 될 도착지 URL -->
<form method="post", action="/articles"> 
  ...
</form>
```
#### HTTP 프로토콜의 개념
- `GET`: 데이터 조회 요청 
- `POST`: 데이터 생성 요청 
- `PUT/PATCH`: 데이터 수정 요청 
- `DELETE`: 데이터 삭제 요청

<br>

## 폼 데이터 주고 받기
`CRUD`: **Create** Read Update Delete
- Create는 Client > Server > DB 전달되는 과정이있다. 이번 예제는 Client > Server 까지 과정을 정리한다.

폼태그 `<form>`은 택배에 비유할수있다. `form`에는 어디로 보낼지, 어떻게 보낼지를 적어야한다.

적혀진대로 `form`데이터는 전송이되고 `Controller`는 이것을 객체에 담아 전송을 받는다.
### 이때 `form`데이터를 받는 객체를 `DTO`라고 한다.

<br>

## DTO란?
**DTO(Data Transfer Object)** 란 계층간 데이터 교환을 위해 사용하는 객체(Java Beans)이다.

DTO를 사용하는 이유는, 자바 domain 객체를 바로 접근하지 않기 위함이다.

고로! 가볍게 생각헤서, 테이블을 조작하기 위해 한단계더 거쳐가는 완충제라고 생각하면 된다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbhAT51%2Fbtq9KnMcdkO%2FmOZeUbsMsAkX1mbfCJl6JK%2Fimg.png">

✅ 테이블을 **직접적으로 접근하지 않음**으로써, **데이터를 보호**하기 위함

<br>

### 예제 실습
1. 사용 파일

    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcl4heH%2Fbtq9JMyGjjZ%2FRhGECYWK6IKwZflB6vgkyK%2Fimg.png">
<br>

2. 뷰 페이지 
- from 태그 정보 추가 : articles/new.mustache
    ```html
    {{>layouts/header}}
    
    <form class="container" action="/articles/create" method="post">
        <div class="form-group">
            <label for="title">제목</label>
            <input type="text" class="form-control" id="article-title" placeholder="제목을 입력하세요" name="title">
        </div>
        <div class="form-group">
            <label for="content">내용</label>
            <textarea class="form-control" id="article-content" placeholder="내용을 입력하세요" rows="3" name="content"></textarea>
        </div>
        <button type="submit" class="btn btn-primary">제출</button>
    </form>
    
    {{>layouts/footer}}
    ```
    - 여기서 form 태그 안에 action과 method는, 쉽게 말하면 **action은 어디로?** **method는 어떻게?** 라고 생각하면 편하다.

<br>

3. 컨트롤러 구현
- 폼 태그 처리 메소드 추가 : controller/ArticleController
    ```java
    package com.example.FirstProject.controller;
    
    import com.example.FirstProject.dto.ArticleForm;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.PostMapping;
    
    @Controller
    public class ArticleController {
    
        @GetMapping("/articles/new")
        public String newArticleForm(){
            return "articles/new"; //templates 를 기준으로 경로를 적어준다.
        }
    
        @PostMapping("/articles/create")  //post 방식으로 /articles/create 요청이 들어오면, 아래의 메소드 실행
        public String createArticle(ArticleForm form){  //폼 태그의 데이터가 ArticleForm 객체로 만들어 진다
            System.out.println(form.toString()); // ArticleForm 객체 정보를 확인!
            return "";
        }
    }
    ```
<br>

4. DTO생성
- 데이터 전송 객체 만들기 : dto/ArticleForm
    ```java
    package com.example.FirstProject.dto;
    
    public class ArticleForm {
    
        private String title;
        private String content;
    
        public ArticleForm(String title, String content) {
            this.title = title;
            this.content = content;
        }
    
        @Override
        public String toString() {
            return "ArticleForm{" +
                    "title='" + title + '\'' +
                    ", content='" + content + '\'' +
                    '}';
        }
    }
    ```
<br>

5. 결과 확인
- 폼 데이터 전송
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbxRRiJ%2Fbtq9HMsD1XT%2FEYcxEK8i6qiqKMbK930hxk%2Fimg.png">

- 서버 로그 확인
    ```java
    ArticleForm{title='spring', content='hello spring'}
    ```

<br>
<br>

#### 참고
[스프링 강의](https://www.youtube.com/watch?v=rzjudEZ8bt0 "홍팍의 스프링 강의")
