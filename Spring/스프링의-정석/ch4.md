> 패스트캠퍼스 - 스프링의 정석(남궁성) 강의를 보며 배운 것들 정리

# Ch. 04 - MyBatis로 게시판 만들기

# 01. MyBatis의 소개와 설정

### 1. MyBatis란?

- SQL Mapping Framework - Easy & Simple
    - SQL Mapping은 자바 코드와 SQL을 매핑해준다는 뜻. SQL을 별도의 XML로 분리해놓고 자바 코드에서 XML에 있는 SQL을 간단한 코드로 사용할 수 있게 해준다.
- 자바 코드로부터 SQL문을 분리해서 관리
- 매개변수 설정과 쿼리 결과를 읽어오는 코드를 제거
- 작성할 코드가 줄어서 생산성 향상 & 유지 보수 편리

### 2. SqlSessionFactoryBean과 SqlSessionTemplate

- SqlSessionFactory - SqlSession을 생성해서 제공
    - SqlSession객체를 제공해주기 때문에 사용. MyBatis 모듈이 제공.
- SqlSession - SQL명령을 수행하는데 필요한 메소드 제공

- SqlSessionFactoryBean - SqlSessionFactory를 Spring에서 사용하기 위한 빈
    - SqlSessionFactory 인터페이스를 구현한 것
- SqlSessionTemplate - SQL명령을 수행하는데 필요한 메소드 제공. thread-safe : 여러 Dao들이 공유 가능. 여러 스레드가 동시에 접근해도 괜찮다. SqlSessionTemplate들이 관리해줌.
    - SqlSession 인터페이스를 구현한 것
    - 이것들은 mybatis-spring이 제공해준다.
    - 이것들을 bean으로 등록해야 한다. root-context.xml에 저장.

### 3. SqlSession의 주요 메소드

- `int insert` : insert문을 실행하고, insert된 행의 갯수를 반환
- `int delete` : delete문을 실행하고, delete된 행의 갯수를 반환
- `int update` : update문을 실행하고, update된 행의 갯수를 반환
- `T selectOne` : 하나의 행을 반환하는 select에 사용. parameter로 SQL에 binding될 값 제공
- `List<E> selectList` : 여러 행을 반환하는 select에 사용. parameter로 SQL에 binding될 값 제공
- `Map<K, V> selectMap` : 여러 행을 반환하는 select에 사용. keyCol에 Map의 key로 사용할 컬럼 지정.

### 4. Mapper XML의 작성

- SQL문을 Mapper XML에 작성
- namespace 설정해서 각 id를 구분
- parameterType : 입력으로 들어가는 인자값
- resultType : SQL문 실행 후 결과값

### 5. `<typeAliases>`로 이름 짧게 하기

- parameterType 에 들어가는 타입을 다 쓰지 않게 하는 방법
- 별명을 지어서 그것으로 대신 사용하는 것
- `<typeAlias alias=”BoardDto” type=”com.fastcampus.ch4.domain.BoardDtod”/>`


# 02. MyBatis로 DAO작성하기

### 1. BoardDao의 작성

1. DB 테이블 생성
2. Mapper XML & DTO 작성(CRUD)
3. DAO 인터페이스 작성
4. DAO 인터페이스 구현 & 테스트

### 2. DTO란? - Data Transfer Object

- 계층간의 데이터를 주고 받기 위해 사용되는 객체
- @Controller
    - 요청과 응답을 처리
    - 데이터 유효성 검증
    - 실행 흐름을 제어
- @Service
    - 비즈니스 로직 담당
    - 트랜잭션 처리
- @Repository
    - 순수 Data Access 기능
    - 조회, 등록, 수정, 삭제
- 관심사와 역할에 따라 분리되어 있는 각 계층들은 DTO를 통해 데이터를 주고 받는다.
- @Repository 계층에서는 예외처리를 하지 않고 무조건 @Service 계층으로 던진다.
- @Service 계층에서 예외처리를 할 수도 있고, @Controller 계층에서 예외처리를 할 수도 있고, 둘 다에서 예외처리를 할 수도 있다.

### 3. `#{}`와 `${}`의 차이

- `#{}`은 값에만 사용할 수 있어서 SQL Injection에 대응할 수 있다.
    - PreparedStatement 를 사용한다.
- `${}`은 유연하게 사용 가능하다. SQL Injection 방지가 안된다. `${}`는 내부적으로 사용해야 하고, 외부적으로 사용해야 한다면 철저하게 검증하고 사용해야 한다.
    - Statement를 사용한다

### 4. XML의 특수 문자 처리

- XML 내의 특수 문자(<, >, &, …)는 `&lt;` `&gt;`로 변환 필요
- 또는 특수문자가 포함된 쿼리를 `<![CDATA[`와 `]]>`로 감싼다.(CDATA = character data)

# 03. 게시물 목록 만들기와 페이징 - TDD

### 1. 게시물 목록 페이징

- 페이징이란 page navigation을 사용해서 게시물 목록을 편리하게 볼 수 있게 하는 것
- pageSize : 10
- beginPage - 페이징 시작
- endPage - 페이징 끝
- page - 현재 페이지
- naviSize : 10

### 2. LIMIT [offset,] row_count

- table에 들어있는 data를 페이지별로 가져오려면 select문에 LIMIT이라는 구문을 사용해야 한다.
- offset : 맨 처음부터 얼마나 떨어져 있는가?
- row_count : 읽어올 row의 수
- 1페이지를 읽어올 때에는 offset은 0
- 2페이지를 읽어올 때에는 offset은 10
- 3페이지를 읽어올 때에는 offset은 20
- page - 1 * 10 하면 offset이 나온다.

### 3. PageHandler

- 게시물 전체 개수, 현재 페이지, 한 페이지의 크기를 가지고 페이징에 필요한 값들을 구한다.

```java
private int totalCnt; // 총 게시물 갯수
private int pageSize; // 한 페이지의 크기
private int naviSize = 10; // 페이지 내비게이션의 크기
private int totalPage; // 전체 페이지의 갯수
private int page; // 현재 페이지
private int beginPage; // 내비게이션의 첫 번째 페이지
private int endPage; // 내비게이션의 마지막 페이지
private boolean showPrev; // 이전 페이지로 이동하는 링크를 보여줄 것인지의 여부
private boolean showNext; // 다음 페이지로 이동하는 링크를 보여줄 것인지의 여부
```

```java
public PageHandler(int totalCnt, int page, int pageSize) {
    this.totalCnt = totalCnt;
    this.page = page;
    this.pageSize = pageSize;

    totalPage = (int)Math.ceil(totalCnt / (double)pageSize);
    beginPage = (page - 1) / naviSize * naviSize + 1;
    endPage = Math.min(beginPage + naviSize - 1, totalPage);
    showPrev = beginPage != 1;
    showNext = endPage != totalPage;

}
```

# 04. 게시판 읽기, 쓰기, 수정, 삭제의 구현
### 1. 기능별 URI 정의

| 작업 | URI | HTTP 메소드 | 설명 |
| --- | --- | --- | --- |
| 읽기 | /board/read?bno=번호 | GET | 지정된 번호의 게시물을 보여준다. |
| 삭제 | /board/remove | POST | 게시물을 삭제한다. |
| 쓰기 | /board/write | GET | 게시물을 작성하기 위한 화면을 보여준다. |
| 쓰기 | /board/write | POST | 작성한 게시물을 저장한다. |
| 수정 | /board/modify?bno=번호 | GET | 게시물을 수정하기 위해 읽어온다. |
| 수정 | /board/modify | POST | 수정된 게시물을 저장한다. |

- URL
    - 처음에는 URL만 있었다. URL은 리소스 경로. 그러다 URN이 등장.
    - URL과 URN을 통칭하는 것이 URI(Identifier)
    - URL과 URI가 거의 비슷한 것이라고 볼 수 있다.
    - URL은 전체 경로를 말하기 때문에 URL 일부를 말할 때는 URI라고도 한다.

### 2. 게시물 읽기 기능의 구현

- 게시물 목록에서 게시물을 클릭했을 때, 해당 게시물 번호를 가지고 요청을 보내서 게시물을 받아온다.
- boardList.jsp → 서버에 요청을 보낸다. /board/read?bno=533 GET → BoardController.java가 요청을 받는다. → boardService.read(bno) 로 DB에서 boardDto를 받는다. → board.jsp에 boardDto를 넘겨준다. → 화면에 출력한다.
- 게시판 읽기(상세보기)에서 목록 버튼을 누르면 boardController로 /board/list GET 요청을 보낸다. 하지만 보고 있던 목록 페이지로 돌아가기 위해서 GET 요청을 보낼 때 page와 pageSize 값을 함께 보낸다.

### 3. 게시물 삭제 기능의 구현

- 게시판 읽기 페이지에서 삭제 버튼을 누르면 BoardController.java의 remove() 메소드로 DB에서 해당 게시물을 삭제한다. boardService.remove(bno, writer) 해당 게시물의 작성자가 현재 로그인한 사용자와 일치해야 삭제 가능해야 한다.
- 수정, 삭제 버튼은 보는 사람이 작성자일 때만 보여줘야함.
- 삭제가 완료된 뒤에는 다시 목록으로 이동.

### 4. 게시물 쓰기 기능의 구현

- 게시물 목록에서 글쓰기 버튼을 클릭하면 BoardController.java 의 write() 메소드 호출(요청은 /board/write GET). 이 메소드가 할 일은 결국 board.jsp 보여주기.
- 대신 board.jsp를 게시글 읽기와 글쓰기 두 가지 용도로 사용하기 때문에. 읽기일 때는 readonly로 내용 변경 못하게. 글쓰기 일 때는 내용을 변경할 수 있게 해야함.
- 그래서 mode값을 설정해서 mode에 따라 readonly를 컨트롤할 수 있게끔 하는 것.
- 유지보수 편하게 하기 위해 하나의 jsp로 구현
- 글쓰기를 하고서 글쓰기 버튼을 누르면 /board/write POST 요청을 보낸다. 그리고 BoardController.java 에 요청은 write() 메소드로 연결되어 실행된다. boardService.write(boardDto) 를 통해 DB에 저장된다. 그리고 목록의 첫 페이지로 돌아와야 한다.

### 5. 게시물 수정 기능의 구현

- 게시판 읽기 페이지에서 수정 버튼을 누르면 게시물 수정페이지로 바뀌면 됨.
- 같은 board.jsp를 사용하기 때문에 제목과 내용의 readonly 속성을 false로 변경하면 됨.
- HTML에서는 attribute와 property를 명확하게 구분함.
    - HTML 태그 안에서는 attribute가 생성된 객체의 속성은 property
- 수정이 완료된 뒤에 등록 버튼을 누르면 /board/modify POST 요청을 컨트롤러에 보냄. boardController.java 에서 modify() 메소드와 연결되고, DB에 수정이 완료된 뒤에 다시 list 요청으로 연결.

# 05. 게시판 검색 기능 추가하기
### 1. 게시판 검색

- 동적 쿼리를 사용해 검색어에 맞는 검색을 해야함.

### 2. MyBatis의 동적 쿼리(1) - `<sql>`과 `<include>`

- 공통 부분을 `<sql>`로 정의하고 `<include>`를 포함시켜 재사용

### 2. MyBatis의 동적 쿼리(2) - `<if>`

- else 는 없고 if만 있다.
- 검색 옵션에 따라 조건을 다르게 할 때 사용할 수 있지만, `<if>` 는 조건이 맞다면 계속 붙을 수 있기 때문에 검색 옵션이 유일해야 한다면 부적합

### 2. MyBatis의 동적 쿼리(3) - `<choose><when>`

- `<if>` 보다 효율적.
- `<when>` 조건에 맞는 문장 하나만 선택 됨. 하나만 선택되기 때문에. 앞의 `<when>`이 조건에 맞다면 뒤 `<when>`은 체크하지 않는다.
- `<when>`에 맞는 조건이 하나도 없다면 `<otherwise>`안의 문장이 실행됨.
- 와일드 카드
    - 와일드 카드는 두 개가 있다.
        - MySQL : % / _
        - Oracle : % / ?
    - _ / ? 는 한 글자(1)만 들어올 수 있다.
    - % 는 여러 글자(0+)가 들어올 수 있다.
    - ‘title%’ 는 ‘title’, ‘title1’ 둘 다 허용
        - ‘title_’ 는 ‘title’ 허용하지 않음. ‘title1’만 허용. ‘_’ 는 반드시 1개가 있어야 함.

### 2. MyBatis의 동적 쿼리(4) - `<foreach>`

- IN : `where bno IN (1, 2, 3)` - 조건에 맞는 여러 개를 가져올 때 사용
    
    ```sql
    SELECT bno, title, content, view_cnt, comment_cnt, reg_date
    FROM board
    WHERE bno IN
    <foreach collection="array" item="bno" open="(" close=")" separator=",">
    	#{bno}
    </foreach>
    ORDER BY reg_date DESC, bno DESC
    ```
    
- `<foreach>` 를 활용하면 Dao에서 배열을 넘겨주고, List를 반환받을 수 있다.
    
    ```java
    public List<BoardDto> getSelected(Integer[] bnoArr) throws Exception {
    	return session.selectList(namespace + "getSelected", bnoArr);
    }
    ```

# 06.REST API와 Ajax
### 1. JSON이란?

- Java Script Object Notation - 자바 스크립트 객체 표기법
- 데이터 주고 받을 때 XML을 많이 썼는데, 실제 data보다 tag가 더 많아서 더 간단하게 만들기 위해 JSON 사용
- {속성명: 속성값1, 속성명2: 속성값2, …}
- 객체 배열 - [{속성명: 속성값, …}, {속성명: 속성값, …}, …]
- Map - [키1:{속성명: 속성값, …}, 키2:{속성명: 속성값, …} …]

### 2. stringify()와 parse()

- JS 객체를 서버로 전송하려면, 직렬화(문자열로 변환)가 필요
- 객체를 만들면 메모리에 객체가 만들어지고 객체의 저장공간에 값을 저장하는데, 저장하려면 값을 하나씩 저장하는 것.
- 전송도 가능하다. HTTP가 Text 기반 프로토콜. 요청, 응답 모두 Text로 주고 받는다.
- JS 객체를 문자열로 바꾸는 것.
- 서버가 보낸 데이터(JSON문자열)를 JS 객체로 변환할 때, 역직렬화가 필요
- JSON.stringify() - 객체를 JSON 문자열로 변환(직렬화, JS객체 → 문자열)
    - {name: “John”, age:30} → ‘{”name”:”John”, “age”:30}’(string으로)
- JSON.parse() - JSON 문자열을 객체로 변환(역직렬화, 문자열 → JS객체)
    - ‘{”name”:”John”, “age”:30}’ → {name: “John”, age:30}(JS객체로)

### 3. Ajax란?

- Asynchronous javascript and XML - 요즘은 JSON을 주로 사용
- 비동기 통신으로 데이터를 주고 받기 위한 기술
- 웹 페이지 전체(data+UI)가 아닌 일부(data)만 업데이트 가능

### 4. jQuery를 이용한 Ajax

### 5. Ajax요청과 응답 과정

- JSON.stringify() 호출하면 객체가 Text로 바뀜.
- 서버에게 객체를 그냥 전송 못하니까 문자열로 바꿔서 요청 전송.
- jackson-databind가 문자열을 Java객체로 변환. 그러면 컨트롤러에서 받을 수 있음.
- Java 객체를 처리하고, jackson-databind가 JSON 문자열로 바꿔서 응답 전송.
- 클라이언트가 응답을 받고, JSON.parse()를 이용해 JS 객체로 변환.
- 전송하려면 문자열로 주고 받아야 한다.

### 6. @RestController

- @ResponseBody 대신, 클래스에 @RestController 사용 가능

### 7. REST란?

- Roy Fielding이 제안한 웹서비스 디자인 아키텍쳐 접근 방식
- 프로토콜에 독립적이며, 주로 HTTP를 사용해서 구현
- 리소스 중심의 API 디자인 - HTTP 메소드로 수행할 작업을 정의
- 리소스 부분은 심플하게 - 유지보수 편리, 알아보기도 쉽다. 리소스는 명사로만 구성

| 리소스 | POST | GET | PUT | DELETE |
| --- | --- | --- | --- | --- |
| /customers | 새 고객 만들기 | 모든 고객 검색 | 고객 대량 업데이트 | 모든 고객 제거 |
| /customers/1 | Error | 고객 1에 대한 세부 정보 검색 | 고객 1이 있는 경우 고객 1의 세부 정보 업데이트 | 고객 1 제거 |
| /customers/1/orders | 고객 1에 대한 새 주문 만들기 | 고객 1에 대한 모든 주문 검색 | 고객 1의 주문 대량 업데이트 | 고객 1의 모든 주문 제거 |

### 8. REST API란?

- Representational State Transfer API - REST 규약을 준수하는 API
- REST is a set of architectural constraints, not a protocol or a standard. API developers can implement REST in a variety of ways.
- API 는 서로 간의 약속. 웹 서비스를 제공하는 쪽과 사용자 쪽 서로 간의 약속.

### 9. RESTful API 설계

| 작업 | URI | HTTP 메소드 | 설명 |
| --- | --- | --- | --- |
| 읽기 | /comment/read?cno=번호 | GET | 지정된 번호의 댓글을 보여준다. |
| 쓰기 | /comment/write | POST | 작성한 댓글을 저장한다. |
| 삭제 | /comment/remove | POST | 댓글을 삭제한다. |
| 수정 | /comment/modify | POST | 수정된 댓글을 저장한다. |
- 위의 설계를 아래처럼 바꾸는 것이 RESTful API 설계

| 작업 | URI | HTTP 메소드 | 설명 |
| --- | --- | --- | --- |
| 읽기 | /comments | GET | 모든 댓글을 보여준다. |
| 읽기 | /comments/{cno} | GET | 지정된 번호의 댓글을 보여준다. |
| 쓰기 | /comments | POST | 새로운 댓글을 저장한다. |
| 삭제 | /comments/{cno] | DELETE | 지정된 번호의 댓글을 삭제한다. |
| 수정 | /comments/{cno} | PUT / PATCH | 수정된 댓글을 저장한다. |
- 동사들을 HTTP 메소드들이 담당하고, URI에는 명사(리소스)만 남기는 것.

# 07. 댓글 기능 구현(1) - DAO 작성

### 1. 댓글 기능 구현 순서

1. DB 테이블 생성
2. Mapper XML 작성(SQL 문으로 CRUD 작성)
3. DAO 작성 & 테스트
4. Service 작성 & 테스트
5. 컨트롤러 작성 & 테스트
6. 뷰(UI) 작성 & 테스트(HTML, CSS, JS, jQuery)

# 08. 댓글 기능 구현(2) - Controller 작성
