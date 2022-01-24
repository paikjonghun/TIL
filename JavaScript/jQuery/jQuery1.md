## 목차
- [목차](#목차)
- [jQuery](#jquery)
- [jQuery 설치](#jquery-설치)
- [jQuery 사용법](#jquery-사용법)


## jQuery

- jQuery는 "적게 작성하고 더 많이 수행하는" 경량 JavaScript 라이브러리입니다.
- jQuery의 목적은 웹사이트에서 JavaScript를 훨씬 더 쉽게 사용할 수 있도록 하는 것입니다.
- jQuery는 많은 자바스크립트 코드 라인을 필요로 하는 많은 일반적인 작업을 수행하고 이를 한 줄의 코드로 호출할 수 있는 메서드로 래핑합니다.

## jQuery 설치

1. 홈페이지 이동(jquery.com) → 다운로드 
2.  맨 아래 Past Releases 에서 jQuery CDN 클릭
3. 1.x 버전에 uncompressed 클릭(minified는 탭이나 띄어쓰기를 다 붙여서 데이터를 줄인 것) → [https://code.jquery.com/jquery-1.12.4.js](https://code.jquery.com/jquery-1.12.4.js) 복사 후 주소창에 입력
4.  js파일 저장하기
5.  이클립스 프로젝트 익스플로러에서 jquery라는 새로운 폴더 만들기  
6.  jquery 안에 jquery 폴더를 하나 더 만들기 
7.  jquery 폴더 안에 다운받은 파일 복사하기


## jQuery 사용법

- head 부분에 다음과 같이 추가
    
    ```jsx
    <script type="text/javascript" src="./jquery/jquery-1.12.4.js"></script>
    <script type="text/javascript"></script>
    ```
    
    - `src=` 파일 외부 자바스크립트 취득. 해당 내용으로 entity를 교체(덮어씌우기 하는 것이라 내용 교체됨)
    - 외부 스크립트 파일을 가지고 오면 스크립트 한 줄이 추가된다고 생각하면 됨.

- `$`가 나오면 기본적으로 jquery 라고 보면됨.
- `$(셀렉터).행위();` : 행위 중 값을 다루는 형태의 경우 항상 2개씩 존재. 값을 가져오는 것과 넣는 것의 인자 개수는 1개 차이.
- ex) `$(셀렉터).val();` -> 값 취득
       `$(셀렉터).val(값);` -> 값 할당

- `$(document).ready(function() {}`
    - 문서가 준비된 후 실행. 복수 선언 시 순차적 실행. window.onload보다 유동적이고 원활하게 쓰기 편하다.
    - `console.log(documnet.getElementById("txt").value);` 랑 같은데 화면 객체에 접근하는 방법이 쉬워졌다.
    
    ```jsx
    console.log($("#txt").val()); // id가 txt인 객체의 value값을 취득
    // 셀렉터를 어떻게 쓰느냐에 따라서 결과가 달라짐.
    
    // id가 txt인 객체의 value값 할당. id는 고유하므로 첫번째 객체만 적용됨
    $("#txt").val("바꾼값"); 
    
    // input[type='text']인 객체의 value값 할당
    $("input[type='text']").val("바꾼값");
    
    // id가 txt인 세 번째 객체의 value값 할당
    $("#txt:nth-child(3)").val("바꾼값"); 
    ```
    

- **이벤트 할당**
    - 이벤트 할당 - 주체가 있고 대상이 있다.
    - `$(셀렉터).on(이벤트타입, 함수);` -> 해당 객체에 이벤트 타입의 이벤트 발생 시 함수 실행
    - `$(셀렉터).on(이벤트타입, 셀렉터2, 함수);` -> 셀렉터 안에 존재하는 셀렉터2들에서 이벤트 타입의 이벤트 발생 시 함수 실행
    - 주체의 객체가 존재하는 한 이벤트는 사라지지 않음
    - 브라우저 개발자 도구의 Event Listners로 요소에 이벤트가 달려있는지 확인 가능
    
    ```jsx
    // id가 btn1인 객체에서 click 이벤트 발생하면 함수 실행.
    // 이렇게 실행할 때에는 함수 이름을 쓰지 않아도 실행됨.
    $("#btn1").on("click", function() { 
    	console.log($("#box").width()); // id가 box인 객체의 width값을 취득
    	$("#box").width($("#box").width() * 2); // id가 box인 객체의 width값 2배로.
    	$("#box").height($("#box").height() * 2); // id가 box인 객체의 height 값 2배로.
    });
    ```
    

- **CSS 속성도 할당 가능**
    
    ```jsx
    $("#box").on("click", function() {
    		pos++;
    		
    		if(pos == colors.length) {
    			pos = 0;
    		}
    		
    		// $(this) : 이벤트 발생 대상.
    		// css(속성, 값) : 해당 속성에 값을 할당.
    		// css(속성) : 해당 속성의 값을 가져온다.
    		
    		console.log($(this).css("background-color"));
    		$(this).css("background-color", colors[pos]);
    	});
    ```
    

- **이벤트 주체와 대상**
    
    ```jsx
    $(document).ready(function() {
    
    	// 이벤트 주체 : #wrap
    	// 이벤트 대상 : #wrap 안에 존재하는 input[type='button']
    
    	$("#wrap").on("click", "input[type='button']", function() {
    		console.log($(this).val()); // 누른 버튼(이벤트 발생 대상)의 value 취득
    
    		// id가 txt인 객체의 value와 이벤트 발생 대상의 value를 더해서 다시 id가 txt인 객체
    		// 의 value에 할당
    		$("#txt").val($("#txt").val() + $(this).val());
    		}
    	}); // wrap click input[type='button'] end
    }); // document ready end - 어떤 함수가 끝나는 것인지 적어주는 게 좋다.
    ```