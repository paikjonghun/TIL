## 게시판 상세보기 수정 기능 만들기

- tb.jsp 에 수정과 삭제 버튼 만들기

```java
<input type="button" value="수정" id="updateBtn">
<input type="button" value="삭제" id="deleteBtn">
```

- 수정 버튼이 동작하도록 script에 클릭 이벤트 추가

```java
	$("#updateBtn").on("click", function() {
		$("#actionForm").attr("action", "tbUpdate");
		$("#actionForm").submit();
	});
```

- 데이터 수정 : 데이터 조회( = 상세보기 데이터 조회, = 작성 화면) → 수정

- 새로운 주소가 생겼으므로 controller에 와서 `@RequestMapping(value = "/tb")` 내용 복붙해서 아래처럼 수정

```java
	@RequestMapping(value = "/tbUpdate")
	public ModelAndView tbUpdate(@RequestParam HashMap<String, String> params,
							ModelAndView mav) throws Throwable {
		
		HashMap<String, String> data = iTestService.getTb(params);
		
		mav.addObject("data", data);
		
		mav.setViewName("test/tbUpdate");
		
		return mav;
	}
```

- tbWrite.jsp 복붙해서 tbUpdate.jsp 로 이름 바꾸고 타이틀 `<title>TB Update</title>` 바꾸기
    - backForm action을 tb로 바꾸기
    
    ```java
    <form action="tb" id="backForm" method="post">
    	<input type="hidden" name="no" value="${param.no}">
    	<input type="hidden" name="page" value="${param.page}">
    	<input type="hidden" name="searchGbn" value="${param.searchGbn}">
    	<input type="hidden" name="searchTxt" value="${param.searchTxt}">
    </form>
    ```
    
    - writeForm id를 updateForm으로 바꾸고 action은 tbUpdates로 바꾸기
    
    ```java
    <form action="tbUpdates" id="updateForm" method="post">
    <input type="hidden" name="no" value="${param.no}">
    <input type="hidden" name="page" value="${param.page}">
    <input type="hidden" name="searchGbn" value="${param.searchGbn}">
    <input type="hidden" name="searchTxt" value="${param.searchTxt}">
    번호 : ${data.TB_NO}<br>
    제목<input type="text" id="title" name="title" value="${data.TB_TITLE}"><br>
    작성자<input type="text" id="writer" name="writer" value="${data.TB_WRITER}"><br>
    내용<br>
    <textarea rows="20" cols="50" id="con" name="con">${data.TB_CON}</textarea>
    <input type="button" value="수정" id="updateBtn">
    <input type="button" value="취소" id="cancleBtn">
    </form>
    ```
    
    - 데이터를 유지해야 하기 때문에 글번호, 페이지, 검색창 데이터를 받아와 hidden에 넣는다.
    - 제목, 작성자, 내용에 이전 페이지에서 받아온 데이터를 추가.

- updateBtn 클릭 이벤트 수정

```java
$("#updateBtn").on("click", function() {
		// instances[이름] : 해당 이름으로 CKEDITOR 객체 취득
		// getData() : 입력된 데이터 취득
		$("#con").val(CKEDITOR.instances['con'].getData());
		if(checkEmpty("#title")) {
			alert("제목을 입력하세요.")
			$("#title").focus();
		} else if(checkEmpty("#writer")) {
			alert("작성자를 입력하세요.")
			$("#writer").focus();
		} else if(checkEmpty("#con")) {
			alert("내용을 입력하세요.")
			$("#con").focus();
		} else {
			$("#updateForm").submit();
		}
	});
```

- 새 주소가 생겼으니 controller 와서 추가

```java
	@RequestMapping(value = "/tbUpdates")
	public ModelAndView tbUpdates(@RequestParam HashMap<String, String> params,
							ModelAndView mav) throws Throwable {
		
		try {
			int cnt = iTestService.tbUpdate(params);
			
		} catch (Exception e) {
			e.printStackTrace();
			mav.setViewName("test/tbUpdates");
		}
		
		return mav;
	}
```

- ITestService 메소드 추가하기

```java
public int tbUpdate(HashMap<String, String> params)throws Throwable;
```

- TestService 메소드 오버라이딩 하기

```java
	@Override
	public int tbUpdate(HashMap<String, String> params) throws Throwable {
		return iTestDao.tbUpdate(params);
	}
```

- ITestDao 메소드 추가하기

```java
public int tbUpdate(HashMap<String, String> params)throws Throwable;
```

- TestDao 메소드 오버라이딩 하기
    - 업데이트 할 것이기 때문에 .update(~~) 사용
    - `update(~~)` : 수정 쿼리 실행

```java
	@Override
	public int tbUpdate(HashMap<String, String> params) throws Throwable {
		// update(~~) : 수정 쿼리 실행
		return sqlSession.update("test.tbUpdate", params);
	}
```

- Test_SQL.xml 에 와서

```java
	<!-- result 지정할 필요가 없다. -->
	<update id="tbUpdate" parameterType="hashmap">
		UPDATE TB SET TB_TITLE = #{title},
              TB_WRITER = #{writer},
              TB_CON = #{con}
		WHERE TB_NO = #{no}
	</update>
```

 

- controller에서 수정

```java
	@RequestMapping(value = "/tbUpdates")
	public ModelAndView tbUpdates(@RequestParam HashMap<String, String> params,
							ModelAndView mav) throws Throwable {
		
		try {
			int cnt = iTestService.tbUpdate(params);
			
			if(cnt == 1) {
				mav.addObject("res", "success");
			} else {
				mav.addObject("res", "failed");				
			}
		} catch (Exception e) {
			e.printStackTrace();
			mav.addObject("res", "error");
		}
		
		mav.setViewName("test/tbUpdates");
		
		return mav;
	}
```

- tbUpdates 주소가 생겼으니 tbUpdates.jsp 생성하고 jquery 작성.
    - body에 값 유지를 위한 hidden 작성.
    
    ```java
    <form action="tb" id="actionForm" method="post">
    	<input type="hidden" name="no" value="${param.no}">
    	<input type="hidden" name="page" value="${param.page}">
    	<input type="hidden" name="searchGbn" value="${param.searchGbn}">
    	<input type="hidden" name="searchTxt" value="${param.searchTxt}">
    </form>
    ```
    
    - script에 아래 내용 추가.
    
    ```java
    <script type="text/javascript">
    $(document).ready(function() {
    	switch('${res}') {
    	case "success" :
    		$("#actionForm").submit();
    		break;
    	case "failed" :
    		alert("수정에 실패하였습니다.");
    		history.back();
    		break;
    	case "error" :
    		alert("수정 중 문제가 발생하였습니다.");
    		history.back();
    		break;
    	}
    });
    </script>
    ```
    

## 게시판 상세보기 삭제 기능 만들기

삭제는 화면이 따로 필요하지 않고 삭제 여부 확인만 한다.

- tb.jsp에서 삭제 버튼 클릭 이벤트 추가

```java
	$("deleteBtn").on("click", function() {
		if(confirm("삭제하시겠습니까?")) {
			$("#actionForm").attr("action", "tbDeletes");
			$("#actionForm").submit();
		}
	});
```

- 주소가 생겼으니 컨트롤러로

- 서비스 → Dao

- tbUpdates.jsp 복붙해서 tbDeletes.jsp 만들기
    - 삭제시에는 검색 관련 정보 전송할 필요가 없으므로 form은 지우기
    - script만 작성해주면 된다.

```java
<script type="text/javascript">
$(document).ready(function() {
	switch('${res}') {
	case "success" :
		location.href = "tbList";
		break;
	case "failed" :
		alert("삭제에 실패하였습니다.");
		history.back();
		break;
	case "error" :
		alert("삭제 중 문제가 발생하였습니다.");
		history.back();
		break;
	}
});
```

- `@RequestMapping(value = "/tbList")` 에 아래 추가. 값이 넘어오지 않는 경우를 위해서 안전 장치 작성

```java
int page = 1;
		// 넘어오는 페이지가 있는 경우 해당 값으로 지정
		if(params.get("page") != null || params.get("page") != "") {
			page = Integer.parseInt(params.get("page"));
		}
```

## 조회수 기능 만들기

- 컨트롤러에서 `iTestService.updateTbHit(params);` 추가

```java
@RequestMapping(value = "/tb")
	public ModelAndView tb(@RequestParam HashMap<String, String> params,
							ModelAndView mav) throws Throwable {
		

		iTestService.updateTbHit(params);
		

		HashMap<String, String> data = iTestService.getTb(params);
		
		mav.addObject("data", data);
		
		mav.setViewName("test/tb");
		
		return mav;
	}
```

- ITestService 에 `updateTbHit(params)` 메소드 추가
    
    ```java
    public void updateTbHit(HashMap<String, String> params) throws Throwable;
    ```
    

- TestService 에 `updateTbHit(params)` 메소드 오버라이딩
    
    ```java
    	@Override
    	public void updateTbHit(HashMap<String, String> params) throws Throwable {
    		iTestDao.updateTbHit(params);
    	}
    ```
    

- ITestDao 에 `updateTbHit(params)` 메소드 추가
    
    ```java
    public void updateTbHit(HashMap<String, String> params) throws Throwable;
    ```
    

- TestsDao 에 `updateTbHit(params)` 메소드 오버라이딩
    
    ```java
    	@Override
    	public void updateTbHit(HashMap<String, String> params) throws Throwable {
    		sqlSession.update("test.updateTbHit", params);
    		
    	}
    ```
    

- SQL.xml 에서 쿼리 추가

```java
	<update id="updateTbHit" parameterType="hashmap">
		UPDATE TB SET TB_HIT = TB_HIT + 1
		WHERE TB_NO = #{no}
	</update>
```

업데이트가 겟보다 나중에 있으면 증가되기 전 값을 가져오게 됨. 그래서 항상 조회수 증가는 세부 내용 가져오기 전에 증가시켜야 함.

- 동기화 방식의 게시판 만들기 프로세스 요약
    
    ![스크린샷 2022-02-17 오후 2.12.35.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/319d54c9-2fb5-4619-9ca1-2f1499fd9d5a/스크린샷_2022-02-17_오후_2.12.35.png)