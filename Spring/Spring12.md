## 목차
- [목차](#목차)
- [동기화와 비동기화](#동기화와-비동기화)
- [비동기 방식 로그인 + 게시판 만들기](#비동기-방식-로그인--게시판-만들기)

---

## 동기화와 비동기화

- **동기화** - 주소가 할당되면 화면이 같이 이동
    
    ![스크린샷 2022-02-14 오후 5.08.06.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e1fb11a3-c1c8-4c10-8963-5ccf823ae9d2/스크린샷_2022-02-14_오후_5.08.06.png)
    

- **비동기화** - 주소가 할당되면 그 결과로 취득.
    - JavaScript 에서 JavaScript로 돌아옴.
    - view를 부르지 않는다.
    
    ![스크린샷 2022-03-02 오전 10.01.57.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8183557c-3478-419b-bfab-96788c37e27b/스크린샷_2022-03-02_오전_10.01.57.png)
    
- **ajax - 비동기 방식의 데이터 전송방식**
    - xml, json - 데이터 전송의 대표적 2가지 형태
        - `{”name”:”john”}` = json
        - `<name value=”john”/>`
            
            `<name>john</name>`
            

- testa 패키지 안에 controller, dao, service 패키지 만들고, controller에 **TestAController.java** 만들기
    
    ```java
    @Controller
    public class TestAcontroller {
    	
    	@RequestMapping(value = "/testa1")
    	public ModelAndView testa1(ModelAndView mav) {
    		mav.setViewName("testa/testa1");
    		
    		return mav;
    	}
    }
    ```
    

- testa1.jsp 만들기
    
    ```jsx
    <script type="text/javascript">
    $(document).ready(function() {
    	$("#sendBtn").on("click", function() {
    
    		// serialize : 전송될 데이터들을 문자열로 변환
    		var params = $("#actionForm").serialize();
    	
    		console.log(params);
    
    	});	
    });
    </script>
    </head>
    <body>
    
    <form action="#" id="actionForm" method="post">
    	<input type="text" name="msg">
    	<input type="button" value="전송" id="sendBtn">
    </form>
    
    </body>
    </html>
    ```
    
    ![스크린샷 2022-03-02 오후 11.50.01.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2dd40048-b3b3-4ba9-b828-ce69a20c566b/스크린샷_2022-03-02_오후_11.50.01.png)
    
    ![serialize된 값이 브라우저 콘솔에 찍힌다.](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5ea3bd81-3835-444a-b295-a0379861240f/스크린샷_2022-03-02_오후_11.50.03.png)
    
    serialize된 값이 브라우저 콘솔에 찍힌다.
    

- 비동기 방식으로 값 전송 - script에 ajax 추가.
    
    ```jsx
    $(document).ready(function() {
    	$("#sendBtn").on("click", function() {
    		// serialize : 전송될 데이터들을 문자열로 변환
    		var params = $("#actionForm").serialize();
    	
    		console.log(params);
    		
    		$.ajax({
    			type : "post", // 전송 형태
    			url : "testa1Ajax", // 통신 주소
    			dataType : "json", // 받을 데이터 형태
    			data : params, // 보낼 데이터. 보낼 것이 없으면 안 씀
    			success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    				console.log(res);
    				$("body").append(res.msg);
    			},
    			error : function(request, status, error) { // 문제 발생 시 실행 함수
    				console.log(request.responseText); // 결과 텍스트
    				console.log(status); // 상태코드
    				console.log(error); // 에러메시지
    			}
    		});
    	});
    	
    	
    });
    ```
    

- 새로운 주소 생겼으므로 컨트롤러로 이동 후 새로운 주소에 대한 메소드 작성
    
    ```java
    // RequestMapping의 method : 접근 방법 제한
    // RequestMapping의 produces : 반환 형태 지정
    @RequestMapping(value = "/testa1Ajax", method = RequestMethod.POST, 
    								produces = "text/json;charset=UTF-8")
    @ResponseBody // View 로 인식 시킴
    public String testa1Ajax(@RequestParam HashMap<String, String> params) 
    																												throws Throwable {
    	// {키:값, 키:값}
    	// ObjectMapper : 객체를 문자열로 변환(json)
    	ObjectMapper mapper = new ObjectMapper();
    		
    	// 데이터를 담을 객체
    	Map<String, Object> modelMap = new HashMap<String, Object>();
    		
    	System.out.println(params); // 넘어온 데이터 확인
    		
    	modelMap.put("res", "성공");
    	modelMap.put("msg", params.get("msg") + "받았음");
    		
    	// writeValueAsString : 객체를 문자열로 변환
    	// 변환 시 문제가 발생할 수 있기 때문에 예외 처리 필요
    		
    	System.out.println(mapper.writeValueAsString(modelMap));
    	return mapper.writeValueAsString(modelMap);
    }
    ```
    

## 비동기 방식 로그인 + 게시판 만들기

- TestAMController 생성
    
    ```jsx
    	@Controller
    	public class TestAMController {
    
    	@Autowired
    	public IMemberService iMservice;
    	
    	@RequestMapping(value = "/amLogin")
    	public ModelAndView amLogin(ModelAndView mav) {
    		mav.setViewName("testa/amLogin");
    		
    		return mav;
    	}
    ```
    

- mLogin.jsp 복사해서 amLogin.jsp 만들고 loginBtn 클릭 이벤트에 ajax 추가
    
    ```jsx
    <script type="text/javascript">
    $(document).ready(function() {
    	$("#id, #pw").on("keypress", function(event) {
    		if(event.keyCode == 13) {
    			$("#loginBtn").click();
    			return false;
    		}
    	});
    	
    	$("#loginBtn").on("click", function() {
    		if(checkEmpty("#id")) {
    			alert("아이디를 입력하세요.");
    			$("#id").focus();
    		} else if(checkEmpty("#pw")) {
    			alert("비밀번호를 입력하세요.");
    			$("#pw").focus();
    		} else {
    			var params = $("#loginForm").serialize();
    			
    			$.ajax({
    				type : "post", // 전송 형태
    				url : "amLoginAjax", // 통신 주소
    				dataType : "json", // 받을 데이터 형태
    				data : params, // 보낼 데이터. 보낼 것이 없으면 안 씀
    				success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    					if(res.res == "success") {
    						location.href = "tbList";
    					} else {
    						alert("아이디나 비밀번호가 틀립니다.")
    					}
    
    				},
    				error : function(request, status, error) { // 문제 발생 시 실행 함수
    					console.log(request.responseText); // 결과 텍스트
    
    				}
    			});
    		}
    	});
    });
    
    function checkEmpty(sel) {
    	if($.trim($(sel).val()) == "") {
    		return true;
    	} else {
    		return false;
    	}
    }
    </script>
    </head>
    <body>
    	<!-- ajax를 태울 것이기 때문에 form이 필요는 없다. -->
    	<form action="#" id="loginForm" method="post">
    		아이디 <input type="text" id="id" name="id"> 비밀번호 <input
    			type="password" id="pw" name="pw"> <input type="button"
    			id="loginBtn" value="로그인">
    	</form>
    
    </body>
    ```
    

- amLoginAjax 주소가 생겼으므로 Controller에서 메소드 매핑
    - HttpSession session 인자 추가해서 활용.
    
    ```jsx
    @RequestMapping(value = "/amLoginAjax", method = RequestMethod.POST, produces = "text/json;charset=UTF-8")
    
    	@ResponseBody // View 로 인식 시킴
    	public String amLoginAjax(@RequestParam HashMap<String, String> params, 
    																			HttpSession session) throws Throwable {
    		ObjectMapper mapper = new ObjectMapper();
    
    		Map<String, Object> modelMap = new HashMap<String, Object>();
    		
    		// 구현 내용
    		// 비밀번호 암호화
    		params.put("pw", Utils.encryptAES128(params.get("pw")));
    				
    		// 사용자 정보 취득
    		HashMap<String, String> data = iMservice.getLogin(params);
    				
    		// 정보취득유무
    		if(data != null) { // 값이 있으면 true
    			// setAttribute(키, 값) : session에 정보 추가
    			session.setAttribute("sMNo", data.get("M_NO"));
    			session.setAttribute("sMNm", data.get("M_NM"));
    					
    			modelMap.put("res", "success");
    					
    		} else { // 로그인 실패
    			modelMap.put("res", "failed");
    				
    		}
    		
    		
    		return mapper.writeValueAsString(modelMap);
    	}
    ```
    

- ATBController 만들기
    
    ```jsx
    @Controller
    public class ATBController {
    
    	@Autowired
    	public ITestService iTestService;
    	
    	@Autowired
    	public IPagingService iPagingService;
    	
    	@RequestMapping(value = "/atbList")
    	public ModelAndView atbList(ModelAndView mav) {
    		
    		
    		
    		mav.setViewName("testa/atbList");
    		
    		return mav;
    	}
    ```
    

- tbList.jsp 복사해서 atbList.jsp 만들기
    - page만 1로 바꾸기
    
    ```java
    <input type="hidden" id="page" name="page" value="1" />
    ```
    
    - tbody 안에 지우기
    
    ```java
    <table>
    	<thead>
    		<tr>
    			<th>번호</th>
    			<th>제목</th>
    			<th>작성자</th>
    			<th>작성일</th>
    			<th>조회수</th>
    		</tr>
    	</thead>
    	<tbody></tbody>
    </table>
    ```
    
    - function reloadList 만들기
    
    ```java
    function reloadList() { // 목록 조회용 + 페이징 조회용
    	var params = $("#actionForm").serialize();
    	
    	$.ajax({
    		type : "post", // 전송 형태
    		url : "atbListAjax", // 통신 주소
    		dataType : "json", // 받을 데이터 형태
    		data : params, // 보낼 데이터. 보낼 것이 없으면 안 씀
    		success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    			
    		},
    		error : function(request, status, error) { // 문제 발생 시 실행 함수
    			console.log(request.responseText); // 결과 텍스트
    		}
    	});
    }
    ```
    

- 새로운 주소가 생겼으므로 컨트롤러
    
    ```java
    @RequestMapping(value = "/atbListAjax", method = RequestMethod.POST, produces = "text/json;charset=UTF-8")
    	@ResponseBody
    	public String atbListAjax(@RequestParam HashMap<String, String> params) throws Throwable {
    		ObjectMapper mapper = new ObjectMapper();
    		
    		Map<String, Object> modelMap = new HashMap<String, Object>();
    		
    		return mapper.writeValueAsString(modelMap);
    	}
    ```
    

- 총 게시글 수 + 페이징 계산 추가
    
    ```java
    		// 총 게시글 수
    		int cnt = iTestService.getTbCnt(params);
    		
    		// 페이징 계산
    		PagingBean pb = iPagingService.getPagingBean(Integer.parseInt(params.get("page")), cnt, 10, 5);
    		
    		params.put("startCount", Integer.toString(pb.getStartCount()));
    		params.put("endCount", Integer.toString(pb.getEndCount()));
    		
    		List<HashMap<String, String>> list = iTestService.getTbList(params);
    
    		modelMap.put("list", list); 
    		modelMap.put("pb", pb);
    ```
    

- atbList.jsp - ajax에서 성공 시 브라우저 콘솔에 출력
    
    ```java
    		success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    			console.log(res);
    		},
    ```
    

- script에 추가
    
    ```java
    	// 목록 조회
    	reloadList();
    ```
    

- 콘솔에 list와 pb의 데이터가 찍힌다.

- 게시판 비동기 처리 - drawList 함수 만들기 - 코어 태그 작업한 것을 스크립트로 전환
    
    ```java
    function drawList(list) {
    	var html = "";
    	
    	for(var data of list) {
    		html += "<tr no=\""data.TB_NO"\">";
    		html += "<td>" + data.TB_NO + "</td>";
    		html += "<td>" + data.TB_TITLE + "</td>";
    		html += "<td>" + data.M_NM + "</td>";
    		html += "<td>" + data.TB_HIT + "</td>";
    		html += "<td>" + data.TB_DT + "</td>";
    		html += "</tr>";
    	}
    	$("tbody").html(html);
    }
    ```
    

- 페이징 비동기 처리 - drawPaging 함수 만들기 - 코어 태그 작업한 것을 스크립트로 전환
    
    ```java
    function drawPaging(pb) {
    	var html = "";
    	
    	
    	html += "<span page=\"1\">처음</span>";
    	if($("#page").val() == "1") {
    		html += "<span page=\"1\">이전</span>";
    	} else {
    		html += "<span page=\"" + $("#page").val() - 1 + "\">이전</span>";
    	}
    	
    	for(var i = pb.startPcount; i <= pb.endPcount; i++) {
    		if($("#page").val() == i) {
    			html += "<span page=\"" + i + "\"><b>" + i + "</b></span>"
    		} else {
    			html += "<span page=\"" + i + "\">" + i + "</span>"
    		}
    	}
    	if($("#page").val() == pb.maxPcount) {
    		html += "<span page=\"" + pb.maxPcount + "\">다음</span>"
    	} else {
    		html += "<span page=\"" + ($("#page").val() * 1 + 1) + "\">다음</span>"
    	}
    	html += "<span page=\"" + pb.maxPcount + "\">마지막</span>"
    	
    	
    	$("#paging_wrap").html(html);
    }
    ```
    

- reloadList 함수에 추가
    
    ```java
    	success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    			console.log(res);
    			drawList(res.list);
    			drawPaging(res.pb);
    		}
    ```
    
- 검색 버튼 눌렀을 때 페이지 이동 없이 검색하기
    
    ```java
    	$("#searchTxt").on("keypress", function(event) {
    		if(event.keyCode == 13) {
    			
    			$("#searchBtn").click();
    			
    			return false;
    		}
    	});
    ```
    
    ```java
    	$("#searchBtn").on("click", function() {
    		$("#page").val("1");
    		
    		// 목록 조회
    		reloadList();
    	});
    ```
    

- 비동기를 사용하는 이유에는 사용자의 피로감을 줄여주기 위함도 있다.

- 페이징 버튼 눌렀을 때 페이지 이동 없이 페이징 이동하기
    
    ```java
    $("#paging_wrap").on("click", "span", function() {
    		$("#page").val($(this).attr("page"));
    		
    		$("#searchGbn").val($("#oldSearchGbn").val());
    		$("#searchTxt").val($("#oldSearchTxt").val());
    		
    		// 목록 조회
    		reloadList();
    	});
    ```
    

- 페이징 이동할 때 검색 구분과 검색어 유지가 안됨.
- 검색 구분 유지하는 방법
    
    ```java
    if("${param.searchGbn}" != "") {
    		$("#searchGbn").val('${param.searchGbn}');
    	} else {
    		$("#oldSearchGbn").val("0");
    	}
    ```
    

- Test_SQL.xml 수정 - CNT가 안 맞아서 paging 버튼이 안 뜨던 오류 해결
    
    ```xml
    <select id="getTbCnt" resultType="Integer" parameterType="hashmap">
    		SELECT COUNT(*) AS CNT
    		FROM TB INNER JOIN M
    						ON TB.TB_WRITER = M.M_NO
    		WHERE 1 = 1
    		<!-- 검색어가 있다면 쿼리를 입력하겠다. -->
    		<if test="searchTxt != null and searchTxt != ''">
    			<choose>
    				<when test="searchGbn == 0">
    					AND TB.TB_TITLE LIKE '%' || #{searchTxt} || '%'
    				</when>
    				<when test="searchGbn == 1">
    					AND M.M_NM LIKE '%' || #{searchTxt} || '%'
    				</when>
    			</choose>
    		</if>
    	</select>
    ```