## 세션 활용해서 로그인 정보 활용 및 검사

- TB 테이블 데이터 다 지우기

- TB_WRITER의 데이터 타입 NUMBER로 바꾸기

- 제약조건 외래키 추가 → M테이블의 PK를 연결 → 로컬 컬럼 TB_WRITER 클릭
    - 이름이 달라도 괜찮다.
    
- Test_SQL.xml에서 update 쿼리에 `TB_WRITER = #{writer},` 지우기

- getTb 쿼리 수정
    
    ```sql
    SELECT TB.TB_NO, TB.TB_TITLE, TB.TB_CON, M.M_NO, M.M_NM AS TB_WRITER, 
    			 TB.TB_HIT, TO_CHAR(TB.TB_DT, 'YYYY-MM-DD') AS TB_DT
    FROM TB INNER JOIN M
                    ON TB.TB_WRITER = M.M_NO
    WHERE TB.TB_NO = #{no}
    ```
    

- getTbList 쿼리 수정
    
    ```sql
    		SELECT T.TB_NO, T.TB_TITLE, T.M_NM, T.TB_HIT, T.TB_DT
    		FROM (SELECT TB.TB_NO, TB.TB_TITLE, M.M_NM, TB.TB_HIT, TO_CHAR(TB.TB_DT, 'YYYY-MM-DD') AS TB_DT,
    				     ROW_NUMBER() OVER(ORDER BY TB.TB_NO DESC) AS RNUM
    			  FROM TB INNER JOIN M
    				                ON TB.TB_WRITER = M.M_NO
    			  WHERE 1 = 1
      	    	  <if test="searchTxt != null and searchTxt != ''">
    				<choose>
    					<when test="searchGbn == 0">
    						AND TB_TITLE LIKE '%' || #{searchTxt} || '%'
    					</when>
    					<when test="searchGbn == 1">
    						AND M.M_NM LIKE '%' || #{searchTxt} || '%'
    					</when>
    				</choose>
    			   </if>
      	    	  ) T
    		WHERE T.RNUM BETWEEN #{startCount} AND #{endCount}
    ```
    
    - TB_WRITER 없애고
    
- tbList.jsp - TB_WRITE를 M_NM으로 바꾸기
    
    ```html
    			<tr no="${data.TB_NO}">
    				<td>${data.TB_NO}</td>
    				<td>${data.TB_TITLE}</td>
    				<td>${data.M_NM}</td>
    				<td>${data.TB_DT}</td>
    				<td>${data.TB_HIT}</td>
    			</tr>
    ```
    

- 세션에서 넘어온 sMNo 값이 있을 때 글쓰기 버튼이 보이게 하기
    
    ```html
    <c:if test="${!empty sMNo}">
    		<input type="button" value="글쓰기" id="writeBtn" />
    </c:if>
    ```
    
- 로그인하지 않았을 때와 로그인 되었을 때(세션 정보에 따라서) 화면 표현을 다르게 해줌
    
    ```html
    <c:choose>
    	<%-- 비로그인 시 --%>
    	<c:when test="${empty sMNo}">
    		로그인이 필요합니다. <input type="button" value="로그인" id="loginBtn">
    	</c:when>
    	<%-- 로그인 시 --%>
    	<c:otherwise>
    		${sMNm}님 어서오십시오. <input type="button" value="로그아웃" id="logoutBtn">	
    	</c:otherwise>
    </c:choose>
    ```
    

- tbWrite.jsp
    
    ```html
    작성자 : ${sMNm}<input type="hidden" name="writer" value="${sMNo}"><br>
    ```
    

- tb.jsp
    - data.TB_WRITER → data.M_NM 으로 바꾸기
    
    ```html
    작성자 : ${data.M_NM}<br>
    ```
    
    - 값 비교해서 버튼 표시하기
    
    ```html
    <c:if test="${data.TB_WRITER eq sMNo}">
    	<input type="button" value="수정" id="updateBtn">
    	<input type="button" value="삭제" id="deleteBtn">
    </c:if>
    ```
    
- tbUpdate.jsp
    - 작성자 표시 바꾸기
    
    ```html
    작성자 : ${data.M_NM}<br>
    ```
    
    - 스크립트에서 작성자 체크 없애기
    
    ```jsx
    $("#updateBtn").on("click", function() {
    		// instances[이름] : 해당 이름으로 CKEDITOR 객체 취득
    		// getData() : 입력된 데이터 취득
    		$("#con").val(CKEDITOR.instances['con'].getData());
    		if(checkEmpty("#title")) {
    			alert("제목을 입력하세요.")
    			$("#title").focus();
    		} else if(checkEmpty("#con")) {
    			alert("내용을 입력하세요.")
    			$("#con").focus();
    		} else {
    			$("#updateForm").submit();
    		}
    	});
    ```
    

- 로그인 된 상태로 mLogin 이동하면 tbList로 연결되도록 하기
    
    ```java
    	@RequestMapping(value = "/mLogin")
    	public ModelAndView mLogin(HttpSession session, ModelAndView mav) {
    		if(session.getAttribute("sMNo") != null) {
    			mav.setViewName("redirect:tbList");			
    		} else {			
    			mav.setViewName("test/mLogin");			
    		}
    		return mav;
    	}
    ```