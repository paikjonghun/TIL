## 개인화 표시 - 로그인된 계정의 게시물만 보기

- 세션에서 sMNo의 값이 넘어오면 params에 넣어준다.
    - `params.put("sMNo", String.valueOf(session.getAttribute("sMNo")));`
    - 세션에 값은 빅인트라는 클래스라서 (string) 캐스팅하면 안된다. 세션에서 값을 가져올 때는 String.valueOf를 사용해야 한다.
- 게시물 수를 가져오는 메소드인 getABCnt 메소드에 params 값을 보내준다.
    
    ```java
    	int cnt = iABService.getABCnt(params);
    			
    	PagingBean pb = iPagingService.getPagingBean(page, cnt, 5, 5);
    			
    	params.put("startCount", Integer.toString(pb.getStartCount()));
    	params.put("endCount", Integer.toString(pb.getEndCount()));
    			
    	List<HashMap<String, String>> list = iABService.getABList(params);
    ```
    
- IABService → ABService → IABDao → ABDao 에 getABCnt 메소드에 params 인자값을 추가한다.
- SQL.xml에서
    - getABCnt의 WHERE 절 수정 - M_NO가 params에서 받아온 sMNo와 같은 데이터만 취득
    
    ```xml
    	<select id="getABCnt" parameterType="hashmap" resultType="Integer">
    		SELECT COUNT(*) AS CNT
    		FROM AB INNER JOIN M
    						ON AB.M_NO = M.M_NO
    		WHERE AB.M_NO = #{sMNo}
    		AND TO_CHAR(AB_DT, 'YYYY-MM') = #{searchMon}
    		<if test="searchTxt != null and searchTxt != ''">
    			<choose>
    				<when test="searchGbn == 0">
    					AND AB_CON LIKE '%' || #{searchTxt} || '%'
    				</when>
    				<when test="searchGbn == 1">
    					AND M_NM LIKE '%' || #{searchTxt} || '%'
    				</when>
    			</choose>
    		</if>
    	</select>
    ```
    
    - getABList의 WHERE 절도 마찬가지로 수정
    
    ```xml
    	<select id="getABList" parameterType="hashmap" resultType="hashmap">
    		SELECT A.AB_NO, A.M_NM, A.PRICE, A.AB_CON, A.A_TYPE, A.AB_DT, A.REG_DT
    		FROM (	SELECT AB.AB_NO, M.M_NO, M.M_NM, AB.PRICE, AB.AB_CON, AB.A_TYPE, TO_CHAR(AB.AB_DT, 'YYYY-MM-DD') AS AB_DT, TO_CHAR(AB.REG_DT, 'YYYY-MM-DD') AS REG_DT, 
    				       ROW_NUMBER() OVER(ORDER BY AB.AB_NO DESC) AS RNUM
    				FROM AB INNER JOIN M
    				                ON AB.M_NO = M.M_NO
    				WHERE AB.M_NO = #{sMNo}
    				AND TO_CHAR(AB_DT, 'YYYY-MM') = #{searchMon}
    				<if test="searchTxt != null and searchTxt != ''">
    					<choose>
    						<when test="searchGbn == 0">
    							AND AB_CON LIKE '%' || #{searchTxt} || '%'
    						</when>
    						<when test="searchGbn == 1">
    							AND M_NM LIKE '%' || #{searchTxt} || '%'
    						</when>
    					</choose>
    				</if> ) A
    		 WHERE A.RNUM BETWEEN #{startCount} AND #{endCount}
    	</select>
    ```
    

- Controller에서 현재 로그인 여부에 따라 abList로 연결하거나 mLogin으로 연결
    
    ```java
    	if(session.getAttribute("sMNo") != null) {
    			params.put("sMNo", String.valueOf(session.getAttribute("sMNo")));
    
    			int page = 1;
    			
    			// 넘어오는 페이지가 있는 경우 해당 값으로 지정
    			if(params.get("page") != null && params.get("page") != "") {
    				page = Integer.parseInt(params.get("page"));
    			}
    			
    			// 총 게시글 수
    			int cnt = iABService.getABCnt(params);
    			
    			PagingBean pb = iPagingService.getPagingBean(page, cnt, 5, 5);
    			
    			params.put("startCount", Integer.toString(pb.getStartCount()));
    			params.put("endCount", Integer.toString(pb.getEndCount()));
    			
    			List<HashMap<String, String>> list = iABService.getABList(params);
    				
    			mav.addObject("page", page);
    			mav.addObject("pb", pb);
    			mav.addObject("list", list);
    			
    			mav.setViewName("test/abList");
    		} else {
    			mav.setViewName("redirect:mLogin");
    		}
    		return mav;
    	}
    ```
    

- 이제 abList와 ab는 로그인 후에만 접속 가능하기 때문에 abList, ab 에서 sNMo 여부 확인해서 글쓰기, 수정, 삭제 등을 표시하는 <c:if>, <c:choose> 삭제 가능

## 월별 표시

- Controller에 아래 추가 - 페이징이 처음부터 떠있어야 하듯이 월별 표시도 처음부터 떠있어야 함.
    
    따라서, 기본 searchMon 값을 오늘로 지정
    
    ```java
    		// 검색 월
    		Date today = new Date();
    			
    		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM");
    			
    		String searchMon = sdf.format(today);
    		
    		// searchMon 값이 넘어오지 않거나 비어있으면 현재 월을 params에 searchMon를 넣어준다.
    		if(params.get("searchMon") == null || params.get("searchMon") == "") {
    			params.put("searchMon", searchMon);
    		}
    
    		mav.addObject("searchMon", params.get("searchMon"));
    ```
    

- abList.jsp에 추가. (값 유지를 위해 ab.jsp, abAdd.jsp, abUpdate.jps, abAction.jsp에는 hidden으로 추가)
    
    ```html
    <input type="month" id="searchMon" name="searchMon" value="${searchMon}">
    ```
    

- abList.jsp에서 searchMon이 바뀔 때 바로 바뀌도록 아래처럼 추가
    
    ```java
    	$("#searchMon").on("change", function() {
    		$("#searchGbn").val($("#oldSearchGbn").val());
    		$("#searchTxt").val($("#oldSearchTxt").val());
    		
    		$("#searchBtn").click();
    	})
    ```
    

## 통계 데이터 표시

abList.jsp 하단에 통계 데이터를 표시하기.

- Controller → abList 메소드 부분에 아래 추가
    
    ```java
    HashMap<String, String> analy = iABService.getAnaly(params);
    
    mav.addObject("analy", analy);
    ```
    

- getAnaly 메소드 생겼으므로 IABService → ABService → IABDao → ABDao 메소드 생성 & 오버라이딩

- SQL.xml 에서 쿼리 작성
    
    ```xml
    <select id="getAnaly" parameterType="hashmap" resultType="hashmap">
    		SELECT M.INSUM, M.OUTSUM, T.TOT
    		FROM (  SELECT SUM(DECODE(AB_TYPE, 0, PRICE, NULL)) AS INSUM, 
    									 SUM(DECODE(AB_TYPE, 1, PRICE, NULL)) AS OUTSUM
    		        FROM AB
    		        WHERE M_NO = #{sMNo}
    		        AND TO_CHAR(AB_DT, 'YYYY-MM') = #{searchMon}) M INNER JOIN 
    							(   SELECT SUM(DECODE(AB_TYPE, 0, PRICE, PRICE * -1)) AS TOT
    		              FROM AB
    		              WHERE M_NO = #{sMNo}) T
    				                                                            ON 1 = 1
    </select>
    ```
    

- abList.jsp 에 테이블 작성해서 컨트롤러에서 받아온 analy 데이터 출력
    
    ```xml
    <table>
    	<tbody>
    		<tr>
    			<th rowspan="2">당월</th>
    			<th>입금</th>
    			<td>${analy.INSUM}</td>
    		</tr>
    		<tr>
    			<th>출금</th>
    			<td>${analy.OUTSUM}</td>
    		</tr>
    		<tr>
    			<th colspan="2">총 잔고</th>
    			<td>${analy.TOT}</td>
    		</tr>
    	</tbody>
    </table>
    ```
    

- 통계 테이블도 상세보기 클릭 이벤트가 적용되므로 게시물 목록 테이블에 클래스 부여하고(`class=*"list_table"*`) 스크립트 수정.
    
    ```jsx
    	$(".list_table tbody").on("click", "tr", function() {
    		$("#no").val($(this).attr("no"));
    		
    		$("#searchGbn").val($("#oldsearchGbn").val());
    		$("#searchTxt").val($("#oldSearchTxt").val());
    		
    		$("#actionForm").attr("action", "ab");
    		$("#actionForm").submit();
    	});
    ```