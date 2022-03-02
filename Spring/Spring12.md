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