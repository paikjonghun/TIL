## Oracle Cloud DB - jdbc 연결하기

- jdbc.properties에서 아래와 같이 작성

```java
jdbc.driverClassName=oracle.jdbc.OracleDriver
jdbc.url=jdbc:oracle:thin:@DB접속명?TNS_ADMIN=지갑압축풀린경로(경로구분은/로)
jdbc.username=USERNAME(해당 데이터베이스 사용자 이름)
jdbc.password=PASSWORD(해당 데이터베이스 사용자 비밀번호)
```

- DB접속명은 Wallet 압축 풀고 tnsnames.ora 에서 확인 가능
    
    ![스크린샷 2022-02-15 오후 10.45.15.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a9ecc47f-28a0-4db6-aaed-604494be7a92/스크린샷_2022-02-15_오후_10.45.15.png)
    
- 또는 Oracle Cloud 관리 페이지에서 확인 가능
    
    ![스크린샷 2022-02-15 오후 10.47.09.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/724647be-c6b6-4cef-bc58-c2ff0246b9a7/스크린샷_2022-02-15_오후_10.47.09.png)
    
- 그리고 pom.xml에서 아래 추가

```java
		<dependency>
		    <groupId>com.oracle.database.security</groupId>
		    <artifactId>oraclepki</artifactId>
		    <version>21.5.0.0</version>
		</dependency>
		
		<dependency>
		    <groupId>com.oracle.database.security</groupId>
		    <artifactId>osdt_cert</artifactId>
		    <version>21.5.0.0</version>
		</dependency>
		
		<dependency>
		    <groupId>com.oracle.database.security</groupId>
		    <artifactId>osdt_core</artifactId>
		    <version>21.5.0.0</version>
		</dependency>
```

## 검색 기능 만들기

- tbList.jsp 에서 body에 form 안에 셀렉트 박스, 텍스트 입력칸, 버튼 추가하기

```java
<form action="#" id="actionForm" method="post">
	<input type="hidden" id="no" name="no" />
	
	<select id="searchGbn" name="searchGbn">
		<option value="0">제목</option>
		<option value="1">작성자</option>
	</select>
	<input type="text" name="searchTxt" id="searchTxt" />
	<input type="button" value="검색" id="searchBtn" />
</form>
```

- 검색 버튼을 눌렀을 때 값 전송

```java
	$("#searchBtn").on("click", function() {
		$("#actionForm").attr("action", "tbList");
		$("#actionForm").submit();
	});
```

- TestController.java 에서 `@RequestParam HashMap<String, String> params`  & getTbList 메소드에 params 추가

```java
	@RequestMapping(value = "/tbList")
	public ModelAndView tbList(@RequestParam HashMap<String, String> params,
								ModelAndView mav) throws Throwable { // DB 사용 시 예외처리 필수
		
		List<HashMap<String, String>> list = iTestService.getTbList(params);
			
		mav.addObject("list", list);
		
		mav.setViewName("test/tbList");
		
		return mav;
	}
```

- i[TestService.java](http://TestService.java) 에 getTbList 인자값 추가 (컨트롤러에서 + Change method 사용하면 자동으로 추가됨)
    
    ![스크린샷 2022-02-15 오후 11.41.16.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5819e140-6951-4996-bde2-be24a5852974/스크린샷_2022-02-15_오후_11.41.16.png)
    

- [TestService.java](http://TestService.java) 에서 getTbList 인자값 추가
    
    ![스크린샷 2022-02-15 오후 11.48.04.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1a23df61-7cdf-485f-8dab-7aa7886afe33/스크린샷_2022-02-15_오후_11.48.04.png)
    

- [iTestDao.java](http://iTestDao.java) 에서 getTbList 인자값 추가
    
    ![스크린샷 2022-02-15 오후 11.47.22.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8a7ddb21-da59-4b16-b4dd-48a3d12f6cdd/스크린샷_2022-02-15_오후_11.47.22.png)
    

- [TestDao.java](http://TestDao.java) 에서 getTbList 인자값 추가
    
    ![스크린샷 2022-02-15 오후 11.49.12.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ab63c25b-1cfa-4604-802e-24c36db5cba9/스크린샷_2022-02-15_오후_11.49.12.png)
    

- Test_SQL.xml 와서 WHERE 추가 - 검색어가 들어와야 검색을 할 것이다.
    - Dynamic Query : 주어진 데이터에 따라 쿼리가 가변적인 것
    - parameterType : 인자를 받는 타입
    
    ```java
    	<select id="getTbList" resultType="hashmap" parameterType="hashmap">
    		SELECT T.TB_NO, T.TB_TITLE, T.TB_WRITER, T.TB_HIT, T.TB_DT
    		FROM (SELECT TB_NO, TB_TITLE, TB_WRITER, TB_HIT,
    								 TO_CHAR(TB_DT, 'YYYY-MM-DD') AS TB_DT,
       			         ROW_NUMBER() OVER(ORDER BY TB_NO DESC) AS RNUM
       		    FROM TB
      	    	WHERE 1 = 1
      	    	<if test="searchTxt != null and searchTxt != ''">
    						<choose>
    							<when test="searchGbn == 0">
    								AND TB_TITLE LIKE '%' || #{searchTxt} || '%'
    							</when>
    							<when test="searchGbn == 1">
    								AND TB_WRITER LIKE '%' || #{searchTxt} || '%'
    							</when>
    						</choose>
    				   </if>
      	    	 ) T
    		WHERE T.RNUM BETWEEN 1 AND 3
    	</select>
    ```
    

- 검색하고 무엇을 입력했는지 안 남아있는 문제
    - 입력칸에 검색어 남아있게 하기 - value 추가
    
    ```java
    <input type="text" name="searchTxt" value="${param.searchTxt}"/>
    ```
    
    - 선택칸에 선택 옵션 남아있게 하기 - Script에 if 추가
    
    ```java
    	if("${param.searchGbn}" != "") {
    		$("#searchGbn").val('${param.searchGbn}');
    	}
    ```
    

- 검색하고 엔터 쳤을 때 주소창에 # 출력되지 않게 하기 - 추가
    
    ```java
    	$("#searchTxt").on("keypress", function(event) {
    		if(event.keyCode == 13) {
    			
    			$("#actionForm").attr("action", "tbList");
    			$("#actionForm").submit();
    			
    			return false;
    		}
    	});
    ```
    

## Paging : 데이터 분할 취득

- 몇 개씩 데이터를 취득할 것인가 : 10건
- 페이징은 몇 개씩 보여줄 것인가 : 10개
- ‘현재 페이지’ & ‘전체 글 개수’ 필수
    - 현재 페이지 : 1p
    - 전체 글 개수 : 183건
- 1p - 01 ~ 10
- 2p - 11 ~ 20
- 3p - 21 ~ 30
    - 총 19p
    - 시작 번호 : (현재페이지 - 1) * 보여질 개수 + 1
    - 종료 번호 : 현재페이지 * 보여질 개수
    - 총 페이지 개수 : 전체 글 개수 / 보여질 개수 만약, 나머지가 존재하면 +1
        - 단, 전체 글 개수가 0이면 총 페이지 개수는 1

- 01p - 1p ~ 10p
- 02p - 1p ~ 10p
- 10p - 1p ~ 10p
- 13p 0 11p ~ 19p
    - 보여질 페이징 시작번호 : (현재p / 보여질 페이징 개수) * 보여질 페이징 개수 + 1
        - 만약, ‘현재p / 보여질 페이징 개수’의 나머지가 0이면, 현재p - 보여질 페이징 개수 + 1
    - 보여질 페이징 종료번호 : 페이징 시작번호 + 보여질 페이징 개수 - 1
        - 단, 페이징 종료번호가 총 페이지보다 크면 종료번호는 총 페이지 개수

- tbList.jsp 에 body 안에 아래 내용 추가

```java
<div id="paging_wrap">
	<span page="1">처음</span>
	<c:choose>
		<c:when test="${page eq 1}">
			<span page="1">이전</span>
		</c:when>
		<c:otherwise>
			<span page="${page - 1}">이전</span>
		</c:otherwise>
	</c:choose>
	<c:forEach var="i" begin="${pb.startPcount}" end="${pb.endPcount}" step="1">
		<c:choose>
			<c:when test="${page eq i}">
				<span page="${i}"><b>${i}</b></span>
			</c:when>
			<c:otherwise>
				<span page="${i}">${i}</span>
			</c:otherwise>
		</c:choose>
	</c:forEach>
	
	<c:choose>
		<c:when test="${page eq pb.maxPcount}">
			<span page="${pb.maxPcount}">다음</span>
		</c:when>
		<c:otherwise>
			<span page="${page + 1}">다음</span>
		</c:otherwise>
	</c:choose>
	<span page="${pb.maxPcount}">마지막</span>
</div>
```

- 페이징 버튼 클릭 시 이벤트 처리 - 페이징 버튼을 클릭하면 id가 page인 input type=”hidden”의 value 값을 클릭한 span의 page속성 값으로 바꾸고 값 전송

```java
	$("#paging_wrap").on("click", "span", function() {
		$("#page").val($(this).attr("page"));
		
		$("#actionForm").attr("action", "tbList");
		$("#actionForm").submit();
	});
```

- [TestController.java](http://TestController.java) 에서 iPagingService에 객체 주입.  page, cnt 변수 만들고 iPagingService.getPagingBean 메소드 활용해서 페이징 설정하기

```java
	@Autowired
	public IPagingService iPagingService;

	@RequestMapping(value = "/tbList")
	public ModelAndView tbList(@RequestParam HashMap<String, String> params,
								ModelAndView mav) throws Throwable { // DB 사용 시 예외처리 필수
		
		int page = 1;
		// 넘어오는 페이지가 있는 경우 해당 값으로 지정
		if(params.get("page") != null) {
			page = Integer.parseInt(params.get("page"));
		}
		
		int cnt = iTestService.getTbCnt(params);
		
		PagingBean pb = iPagingService.getPagingBean(page, cnt, 3, 10);
		
		params.put("startCount", Integer.toString(pb.getStartCount()));
		params.put("endCount", Integer.toString(pb.getEndCount()));
```

- iTestService → TestService → iTestDao → TestDao 에 getTbCnt 메소드 만들고 오버라이딩

- Test_SQL.xml 에서 getTbCnt 쿼리 만들기 - 그러면 컨트롤러로 페이지 개수 총합을 가져올 수 있다.

```java
	<select id="getTbCnt" resultType="Integer" parameterType="hashmap">
		SELECT COUNT(*) AS CNT
		FROM TB
		WHERE 1 = 1
		<if test="searchTxt != null and searchTxt != ''">
			<choose>
				<when test="searchGbn == 0">
					AND TB_TITLE LIKE '%' || #{searchTxt} || '%'
				</when>
				<when test="searchGbn == 1">
					AND TB_WRITER LIKE '%' || #{searchTxt} || '%'
				</when>
			</choose>
		</if>
	</select>
```

- getTbList 쿼리에서 WHERE절 수정

```java
WHERE T.RNUM BETWEEN #{startCount} AND #{endCount}
```

- iPagingService.getPagingBean 메소드 인자에 현재 페이지(page), 데이터 총 개수(cnt), 한 페이지에 보여질 데이터 개수(3), 보여질 페이징 총 개수(10) 넣어주면 자동으로 PagingBean pb에 돌려줌

```java
PagingBean pb = iPagingService.getPagingBean(page, cnt, 3, 10);
```

- [PagingService.java](http://PagingService.java)

```java
@Override
	public PagingBean getPagingBean(int page, int maxCount, int viewCnt, int pageCnt) {
		PagingBean pb = new PagingBean();
		
		pb.setStartCount(getStartCount(page, viewCnt));
		pb.setEndCount(getEndCount(page, viewCnt));
		pb.setMaxPcount(getMaxPcount(maxCount, viewCnt));
		pb.setStartPcount(getStartPcount(page, pageCnt));
		pb.setEndPcount(getEndPcount(page, maxCount, viewCnt, pageCnt));
		
		return pb;
	}
```

- params에 startCount와 endCount를 키값으로 페이지 게시글 시작번호와 페이지 게시글 종료번호를 설정할 수 있다.

```java
params.put("startCount", Integer.toString(pb.getStartCount()));
params.put("endCount", Integer.toString(pb.getEndCount()));
```

- 이렇게 구한 pb와 page를 view에 넘겨준다

```java
mav.addObject("pb", pb);
mav.addObject("page", page);
```

- 검색했을 때 기본적으로 1페이지로 오도록 만들기
    - `$("#page").val("1");` 을 넣어준다.

```java
$("#searchTxt").on("keypress", function(event) {
		if(event.keyCode == 13) {
			$("#page").val("1");
			
			$("#actionForm").attr("action", "tbList");
			$("#actionForm").submit();
			
			return false;
		}
	});
	
	$("#searchBtn").on("click", function() {
		$("#page").val("1");
		
		$("#actionForm").attr("action", "tbList");
		$("#actionForm").submit();
	});
```