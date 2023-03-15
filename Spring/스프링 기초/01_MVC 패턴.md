## MVC 패턴
#### MVC 패턴이란?
- Model-View-Controller
- **역할 분담** 패턴
- **데이터처리자**, **화면처리자**, **중재자**를 두어 프로그램을 개발하는 방식
    - `모델(Model)`: 데이터 담당
    - `뷰(View)`: 화면 담당
    - `컨트롤러(Controller)`: 모델과 뷰 사이의 중재자 역할을 한다.
- 스프링을 사용한 웹 서비스는 MVC 패턴으로 만들어진다.

<img src="https://i.imgur.com/V7CGG0Y.png">

<br>

#### MVC 패턴의 장점
MVC 패턴을 통해 각 모듈의 역할이 명확해짐에 따라,
1. 개발자-퍼블리셔간 **협업이 원활**해진다.
2. **코드 유지보수성 증가**한다.

<br>

## MVC 예제
#### 1. 뷰 페이지 작성
- 생성 경로: ../resources/templates/greetings.mustache
- greetings 뷰 페이지
    ```html
    <html>
    <head>
        <meta charset="UTF-8">
        <meta name="viewport"
              content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <h1>{{username}}님, 반갑습니다!</h1>
    </body>
    </html>
    ```

<br>

#### 2. 컨트롤러 만들기, 페이지에 변수 삽입, 모델 사용하기
- 생성 경로: 
    ../com.example.firstproject/controller/FirstController
- greetings 뷰 페이지
    ```java
    package com.example.firstproject.controller;
  
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.GetMapping;
  
    @Controller
    public class FirstController {
  
        @GetMapping("/hi")      // 루트 페이지명
        public String niceToMeetYou(Model model) {      // 뷰 페이지에서 설정한 {{username}}을 적용하려면, Model(데이터)를 매개변수로 받아야 한다.
        model.addAttribute("username", "은비");    // 해당값을 찾기위한 key 값, 두번째 인자는 value 값 입력
        return "greetings";     // templates/greetings.mustache -> 브라우저로 전송!
        }
    }
    ```
  - Spring MVC 에서 이 클래스가 컨트롤러 클래스임을 명시하려면 `@Controller`를 붙여줘야 한다.
  - 여기서 String 을 리턴타입으로 잡은것은, 이 예제에서 사용할 파일의 이름을 리턴해주기 위해서이다.
  - 유저가 **루트 페이지("/") 에 접속**하면, 컨트롤러가 그 요청을 받아서 **greetings 를 리턴**해주겠다는 의미가 된다.
  - 모델에 값을 추가할때는, `addAttribute` 메소드를 이용하고, 첫번째 인자는 해당값을 찾기위한 **key 값**, 두번째 인자는 **value 값**이다.

<br>

