## 개인화 표시

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
    

```java
if(session.getAttribute("sMNo") != null) {
			params.put("sMNo", String.valueOf(session.getAttribute("sMNo")));
			
			
			// 검색 월
			Date today = new Date();
			
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM");
			
			String searchMon = sdf.format(today);
			
			if(params.get("searchMon") == null || params.get("searchMon") == "") {
				params.put("searchMon", searchMon);
			}
			
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
			
			// 통계 데이터
			HashMap<String, String> analy = iABService.getAnaly(params);
			
			
			mav.addObject("page", page);
			mav.addObject("pb", pb);
			mav.addObject("list", list);
			mav.addObject("searchMon", params.get("searchMon"));
			mav.addObject("analy", analy);
			
			mav.setViewName("test/abList");
		} else {
			mav.setViewName("redirect:mLogin");
		}
```

## 월별 표시

- 추가
    
    ```html
    <input type="month" id="searchMon" name="searchMon" value="${searchMon}">
    ```
    

## 통계 표시

```html
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