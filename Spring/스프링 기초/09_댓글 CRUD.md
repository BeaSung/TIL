## 댓글 엔티티와 리파지터리

### 개념
- 게시글과 댓글은 1:n(일대다) 관계 
  - 게시글 기준: **One-to-Many** 
  - 댓글 기준: **Many-to-One**
- JpaRepository 사용: 데이터 CRUD 뿐만 아니라 일정 페이지의 데이터 조회 및 정렬 기능 제공

### 댓글 Entity
- `@ManyToOne`: 다대일 관계 설정
- `@JoinColumn(name = "column_name")`: column_name(FK)에 선언된 Entity의 대푯값(PK)을 저장

### 댓글 Repository
- 조회 방법
  1. @Query(value = "sql 코드", nativeQuery = true)
  2. 네이티브 쿼리 XML
     - resources/META-INF 디렉토리 생성
     - orm.xml 작성

### Test
- `@DataJpaTest`: JPA와 연동한 테스트 
- `@DisplayName`: 테스트 결과에 보여줄 이름
- 입력 데이터 준비 / 실제 수행 / 예상하기 / 검증 순으로 진행


