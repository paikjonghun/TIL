## 페이지 1 수정 - 글쓰기나 수정하고 목록 왔을 때 페이지 유지하기

- ATBController - RequestParam 추가, if문 작성, mav.addObject 추가
    
    ```jsx
    @RequestMapping(value = "/atbList")
    	public ModelAndView atbList(@RequestParam HashMap<String, String> params, ModelAndView mav) {
    		
    		if(params.get("page") == null || params.get("page") == "") {
    			params.put("page", "1");
    		}
    		
    		mav.addObject("page", params.get("page"));
    		
    		mav.setViewName("testa/atbList");
    		
    		return mav;
    	}
    ```
    

- atbList.jsp - page의 value 1 수정
    
    ```jsx
    <input type="hidden" id="page" name="page" value="${page}" />
    ```
    

- atbList.jsp - loginBtn, logoutBtn 클릭 이벤트 수정
    
    ```jsx
    	$("#logoutBtn").on("click", function() {
    		location.href ="amLogout";
    	});
    	
    	$("#loginBtn").on("click", function() {
    		location.href = 'amLogin';
    	});
    ```
    

- TestAMController - amLogout 메소드 추가
    
    ```jsx
    	@RequestMapping(value = "/amLogout")
    	public ModelAndView amLogout(HttpSession session, ModelAndView mav) {
    		
    		session.invalidate();
    		
    		mav.setViewName("redirect:amLogin");
    		
    		return mav;
    	}
    ```
    

- amLogin.jsp - abList → atbList로 수정
    
    ```jsx
    	success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    					if(res.res == "success") {
    						location.href = "atbList";
    					} else {
    						alert("아이디나 비밀번호가 틀립니다.")
    					}
    
    	}
    ```
    

- atbList.jsp - tbWrite → atbWrite로 수정
    
    ```jsx
    $("#writeBtn").on("click", function() {
    		$("#searchGbn").val($("#oldSearchGbn").val());
    		$("#searchTxt").val($("#oldSearchTxt").val());
    		
    		$("#actionForm").attr("action", "atbWrite");
    		$("#actionForm").submit();
    });
    ```
    

- ATBController - atbWirte 메소드 추가
    
    ```jsx
    	@RequestMapping(value = "/atbWrite")
    	public ModelAndView atbWrite(ModelAndView mav) {
    		
    		mav.setViewName("testa/atbWrite");
    		
    		return mav;
    	}
    ```
    

- tbWrite 복사해서 atbWrite 만들기

- atbWrite <body> 쪽 action 부분 수정
    
    ```jsx
    <form action="atbList" id="backForm" method="post">
    	<input type="hidden" name="no" value="${param.no}">
    	<input type="hidden" name="page" value="${param.page}">
    	<input type="hidden" name="searchGbn" value="${param.searchGbn}">
    	<input type="hidden" name="searchTxt" value="${param.searchTxt}">
    </form>
    
    <form action="#" id="writeForm" method="post">
    제목<input type="text" id="title" name="title"><br>
    작성자 : ${sMNm}<input type="hidden" name="writer" value="${sMNo}"><br>
    내용<br>
    <textarea rows="20" cols="50" id="con" name="con"></textarea>
    <input type="button" value="작성" id="writeBtn">
    <input type="button" value="취소" id="cancleBtn">
    </form>
    ```
    

- atbWirte.jsp - ajax 추가
    
    ```jsx
    	var params = $("#writeForm").serialize();
    			
    			$.ajax({
    				type : "post", // 전송 형태
    				url : "atbAction/insert", // 통신 주소
    				dataType : "json", // 받을 데이터 형태
    				data : params, // 보낼 데이터. 보낼 것이 없으면 안 씀
    				success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    					
    				},
    				error : function(request, status, error) { // 문제 발생 시 실행 함수
    					console.log(request.responseText); // 결과 텍스트
    				}
    	});
    ```
    

- ATBController - 메소드 추가
    - `@PathVariable` : 주소의 {키} 부분을 변수로 취득
    
    ```java
    @RequestMapping(value = "/atbAction/{gbn}", method = RequestMethod.POST, 
    								produces = "text/json;charset=UTF-8")
    	@ResponseBody
    	public String atbAction(@RequestParam HashMap<String, String> params, 
    													@PathVariable String gbn) throws Throwable {
    		ObjectMapper mapper = new ObjectMapper();
    		
    		Map<String, Object> modelMap = new HashMap<String, Object>();
    		
    		try {
    			switch(gbn) {
    			case "insert":
    				iTestService.tbWrite(params);
    				break;
    			case "update":
    				iTestService.tbUpdate(params);
    				break;
    			case "delete":
    				iTestService.tbDelete(params);
    				break;
    			}
    			modelMap.put("res", "success");
    		} catch (Throwable e) {
    			e.printStackTrace();
    			modelMap.put("res", "failed");
    		}
    		
    		return mapper.writeValueAsString(modelMap);
    	}
    ```
    

- atbWrite.jsp - ajax - success 부분에 추가
    
    ```jsx
    		success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    					if(res.res == "success") {
    						location.href = "atbList";
    					} else {
    						alert("작성 중 문제가 발생했습니다.");
    					}
    		},
    ```
    

- 서버 실행하면 글쓰기(atbWirte) 잘 됨.

- atbList.jsp -  tb → atb 로 수정
    
    ```jsx
    $("tbody").on("click", "tr", function() {
    		$("#no").val($(this).attr("no"));
    		
    		$("#searchGbn").val($("#oldSearchGbn").val());
    		$("#searchTxt").val($("#oldSearchTxt").val());
    		
    		$("#actionForm").attr("action", "atb");
    		$("#actionForm").submit();
    	});
    ```
    

- tb.jsp 복사해서 atb.jsp 만들기

- atb.jsp - body 부분 수정
    
    ```jsx
    $("#listBtn").on("click", function() {
    		$("#actionForm").attr("action", "atbList");
    		$("#actionForm").submit();
    	});
    	
    	$("#updateBtn").on("click", function() {
    		$("#actionForm").attr("action", "atbUpdate");
    		$("#actionForm").submit();
    	});
    	
    	$("#deleteBtn").on("click", function() {
    		if(confirm("삭제하시겠습니까?")) {
    			var params = $("#actionForm").serialize();
    			
    			$.ajax({
    				type : "post", // 전송 형태
    				url : "atbAction/delete", // 통신 주소
    				dataType : "json", // 받을 데이터 형태
    				data : params, // 보낼 데이터. 보낼 것이 없으면 안 씀
    				success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    					if(res.res == "success") {
    						location.href = "atbList";
    					} else {
    						alert("삭제 중 문제가 발생했습니다.");
    					}
    				},
    				error : function(request, status, error) { // 문제 발생 시 실행 함수
    					console.log(request.responseText); // 결과 텍스트
    				}
    			});
    		}
    	});
    ```
    

- 컨트롤러에서 수정은 상세보기 조회와 같아서 atb 복사해서 atbUpdate로 수정
    
    ```jsx
    	@RequestMapping(value = "/atbUpdate")
    	public ModelAndView atbUpdate(@RequestParam HashMap<String, String> params,
    							ModelAndView mav) throws Throwable {
    		
    		HashMap<String, String> data = iTestService.getTb(params);
    		
    		mav.addObject("data", data);
    		
    		mav.setViewName("testa/atbUpdate");
    		
    		return mav;
    	}
    ```
    

- tbUpdate.jsp 복사해서 atbUpdate.jps 만들기

- atbUpdate.jsp - updateForm 에서 page, searchGbn, searchTxt 지우기
    
    ```jsx
    <form action="atb" id="backForm" method="post">
    	<input type="hidden" name="no" value="${param.no}">
    	<input type="hidden" name="page" value="${param.page}">
    	<input type="hidden" name="searchGbn" value="${param.searchGbn}">
    	<input type="hidden" name="searchTxt" value="${param.searchTxt}">
    </form>
    
    <form action="#" id="updateForm" method="post">
    	<input type="hidden" name="no" value="${param.no}">
    	번호 : ${data.TB_NO}<br>
    	제목<input type="text" id="title" name="title" value="${data.TB_TITLE}"><br>
    	작성자 : ${data.M_NM}<br>
    	내용<br>
    	<textarea rows="20" cols="50" id="con" name="con">${data.TB_CON}</textarea>
    	<input type="button" value="수정" id="updateBtn">
    	<input type="button" value="취소" id="cancleBtn">
    </form>
    ```
    

- atbUpdate.jsp - updateBtn 클릭 했을 때 비동기화 처리
    
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
    			var params = $("#updateForm").serialize();
    			
    			$.ajax({
    				type : "post", // 전송 형태
    				url : "atbAction/update", // 통신 주소
    				dataType : "json", // 받을 데이터 형태
    				data : params, // 보낼 데이터. 보낼 것이 없으면 안 씀
    				success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    					if(res.res == "success") {
    						$("#backForm").submit();
    					} else {
    						alert("수정 중 문제가 발생했습니다.");
    					}
    				},
    				error : function(request, status, error) { // 문제 발생 시 실행 함수
    					console.log(request.responseText); // 결과 텍스트
    				}
    			});
    		}
    	});
    ```