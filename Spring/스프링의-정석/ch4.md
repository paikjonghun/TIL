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

- int insert : insert문을 실행하고, insert된 행의 갯수를 반환
- int delete : delete문을 실행하고, delete된 행의 갯수를 반환
- int update : update문을 실행하고, update된 행의 갯수를 반환
- T selectOne : 하나의 행을 반환하는 select에 사용. parameter로 SQL에 binding될 값 제공
- List<E> selectList : 여러 행을 반환하는 select에 사용. parameter로 SQL에 binding될 값 제공
- Map<K, V> selectMap : 여러 행을 반환하는 select에 사용. keyCol에 Map의 key로 사용할 컬럼 지정.

### 4. Mapper XML의 작성

- SQL문을 Mapper XML에 작성
- namespace 설정해서 각 id를 구분
- parameterType : 입력으로 들어가는 인자값
- resultType : SQL문 실행 후 결과값

### 5. <typeAliases>로 이름 짧게 하기

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

### 3. #{}와 ${}의 차이

- #{}은 값에만 사용할 수 있어서 SQL Injection에 대응할 수 있다.
    - PreparedStatement 를 사용한다.
- ${}은 유연하게 사용 가능하다. SQL Injection 방지가 안된다. ${}는 내부적으로 사용해야 하고, 외부적으로 사용해야 한다면 철저하게 검증하고 사용해야 한다.
    - Statement를 사용한다

### 4. XML의 특수 문자 처리

- XML 내의 특수 문자(<. >, &, …)는 &lt; &gt;로 변환 필요
- 또는 특수문자가 포함된 쿼리를 <![CDATA[와 ]]>로 감싼다.(CDATA = character data)

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

### PageHandler

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