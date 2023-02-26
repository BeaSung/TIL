## Gradle 환경에서 lombok 라이브러리 추가

1. Lombok gradle plugin 추가
    * https://projectlombok.org/setup/gradle
      * 롬복 공식 가이드를 보면 플러그인이 설치된 경우 롬복을 간단하게 추가할 수 있다고 나온다.(인텔리제이에는 롬복 플러그인이 이미 설치되어 있다. 없는 경우 설치 필요)
        ```groovy
        plugins {
           id 'java'
        
           id "io.freefair.lombok" version "6.6.3"
        }
        ```
2. Enable annotation processing 활성화
    * IntelliJ Settings(Command + ,) > Build, Execution, Deployment > Compiler > Annotation Processors
      * Enable annotation processing 의 체크박스를 체크한다.
