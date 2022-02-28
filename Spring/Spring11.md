세션에 값은 빅인트라는 클래스라서 (string) 캐스팅하면 안된다. 세션에서 값을 가져올 때는 String.valueOf를 사용해야 한다.

달력도 처음부터 떠있어야 하고, 페이징도 처음부터 있어야 한다.

## 개인화 표시

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