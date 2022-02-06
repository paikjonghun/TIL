## JSP - Java Server(Servlet) Page

- HTML + Java ⇒ JSP

사용자                   서버                                                                                                                사용자

호출 ——————> JSP 실행 → Java 실행 → 실행결과를 포함하여 HTML로 전송 —————> HTML출력

                                                    Servlet 엔진이 실행. 

서버쪽에서 프로그램을 실행하고 사용자는 실행결과를 받는다.

- JSP-library
    - `jstl-1.2.jar` / `standard-1.1.2.jar` 을 WEB-INF → lib 폴더에 넣어준다.
    - 아래와 같이 추가해주면 Core 태그 사용 가능
    
    ```java
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    ```
    

- `c:out` : 출력용
    
    ```java
    <c:out value="Hi"></c:out>
    ```
    

- `c:set` : 변수 선언
    
    ```java
    <c:set var="a" value="11"></c:set>
    ```
    

- `c:if test` : test에 있는 조건이 true이면 실행. 단건에 대한 조건 처리.
- `${ }` : EL Tag. 값 취득 및 계산용도.
    
    ```java
    <c:if test="${a > 10}">
    ^_^
    </c:if>
    ```
    

- `c:choose` : 복수 조건에 대한 조건 처리
    
    ```java
    <c:choose>
    	<c:when test="${a > 10}">
    		a는 10보다 크다
    	</c:when>
    	<c:otherwise>
    		a는 10보다 작거나 같다
    	</c:otherwise>
    </c:choose>
    ```
    

- core 태그나 el 태그를 html 주석에 넣으면 터지기 때문에 JSP주석을 써야함
    
    ```java
    <%-- JSP 주석 --%>
    ```
    

- `c:forEach` : 반복문.
    - `var` : 변수선언, `begin` : 시작값, `end` : 종료값, `step` : 증감값
    - varStatus의 index : 순차데이터일 경우 인덱스번호, 아닐 경우 var의 숫자
    - el에서 같다 : `eq`, 다르다 : `ne`
    
    ```java
    <c:forEach var="i" begin="1" end="10" step="1" varStatus="status"> 
    	<c:choose>
    		<c:when test="${status.count % 2 eq 0}">
    			<!-- html태그도 사용 가능 -->
    			<b>${i}, ${status.index}, ${status.count}</b><br/>
    		</c:when>
    		<c:otherwise>
    			${i}, ${status.index}, ${status.count}<br/>
    		</c:otherwise>
    	</c:choose>
    </c:forEach>
    ```
    

- `c:import` : 해당 페이지의 내용(HTML. 결과만 가져오겠다.)을 해당 위치에 넣겠다.
    - `c:param` : import 페이지에 해당 키(name)로 값(entity 또는 value 속성)을 전송
    
    ```java
    <c:import url="test2.jsp">
    	<c:param name="msg">메시지전송!</c:param>
    </c:import>
    ```
    

- el에서의 param : 넘어오는 값을 받아올 때 사용.
    - `param.키` : 넘어오는 값들 중 키가 일치하는 것을 가져온다.
    
    ```java
    ${param.msg}
    ```
    
- `form` : 값 전달 및 이동용 태그
    - `action` : 목적지
    - `method` : 전송방식
    - 전송방식 get : 주소창에 키와 값을 포함하여 전송
    - 전송방식 post : 주소 데이터 헤더에 키와 값을 포함하여 전송.
    - 주소데이터 헤더 : 서버에 요청 시 사용자의 브라우저, ip등 접속 정보를 포함하여 넘어가는 데이터로 사용자에게는 보이지 않음
    - URL : 주소
    - URI : 주소 + 정보
    
    ```java
    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
    <script type="text/javascript" src="./jquery/jquery-1.12.4.js"></script>
    <script type="text/javascript">
    $(document).ready(function() {
    	$("#sendBtn").on("click", function() {
    		if($.trim($("[name='msg']").val()) == "") {
    			alert("입력하세요.");
    		} else if(isNaN($("[name='msg']").val() * 1)) {
    			alert("숫자를 입력하세요.");
    		} else {
    			// submit() : 폼 실행
    			$("#sendForm").submit();	
    		}
    	});
    });
    </script>
    </head>
    <body>
    <form action="test4.jsp" id="sendForm" method="post">
    	<!-- name : 값 전달시 키값으로 사용 -->
    	<input type="text" name="msg">
    	<input type="button" value="전송" id="sendBtn">
    </form>
    </body>
    </html>
    ```
    
- `history.back()` : 방문기록 바로 전 단계로 이동
- `history.go()` : 해당 개수만큼 방문 기록 이동
- `history.go(-1)` : back하고 똑같음

```java
$(document).ready(function() {
	$("#backBtn").on("click", function() {
		history.back();
	});
});
```

- test3.jsp에서 받아온 숫자를 이용하여 구구단 출력

```java
<c:forEach var="i" begin="1" end="9" step="1">
${param.msg} * ${i} = ${param.msg * i}
<br/>
</c:forEach>
```