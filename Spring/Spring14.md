## 업로드

사용자가 서버에 File을 던짐

업로드는 서버에서는 File 정보를 기반으로 해서 서버에 물리적 저장을 함.

?

## 다운로드

사용자가 동일하게 서버에 File을 요청함.

서버에서는 요청을 기반으로해서 물리적 저장 경로에 저장된 파일을 사용자에게 전달.

DB에 저장해야하는 부분은 물리적 경로(file name)

스트림은 통로. 버퍼는 누적하는 것.  버퍼링은 누적하는 중.

- fileUpload.jsp

`enctype` : form 전송 데이터 형태. 기본적으로는 text/form-data 임. 하지만 파일 전송을 해야하기 때문에 multipart 로 표현함. 파일 데이터를 포함해서 form-data를 가져가겠다. action을 지정해줘야 함.

`multipart` : 파일 데이터를 포함한 형태

*`multipart/form-data`*는 serialize()가 안됨.

`ajaxForm` : form의 동작을 동기화에서 비동기화 방식으로 변경 - ajaxForm 사용하려면, jquery.form.js를 추가해야함.

`beforeSubmit` - 이거 부르기 전에 필터들이 들어가기 때문에 별로 쓸 일은 없다. 유효값 체크하기.

```jsx
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>FileUpload Test</title>
<script type="text/javascript" 
		src="resources/script/jquery/jquery-1.12.4.min.js"></script>
<script type="text/javascript" 
		src="resources/script/jquery/jquery.form.js"></script>

<script type="text/javascript">
$(document).ready(function() {
	$("#uploadBtn").on("click", function(){

		// ajaxForm : form의 동작을 동기화에서 비동기화 방식으로 변경
		// *multipart/form-data*는 serialize()가 안됨.
		var fileForm = $("#fileForm");
		
		fileForm.ajaxForm({ // 보내기전 validation check가 필요할경우 
			beforeSubmit: function (data, frm, opt) { 
				alert("전송전!!"); return true;
			}, // submit 이후의 처리
			success: function(responseText, statusText){
				if(responseText.result =="SUCCESS"){
					alert("저장완료");
					
					console.log(responseText);

				} else {
					alert("저장실패");
				} 
			}, //ajax error
			error: function(){
				alert("에러발생!!"); 
			}
		});
		
		fileForm.submit();
	});
});

</script>
</head>
<body>
<!-- 
	enctype : form 전송 데이터 형태
	multipart : 파일 데이터를 포함한 형태
-->
<form id="fileForm" name="fileForm" action="fileUploadAjax" method="post" 
																					enctype="multipart/form-data">
<h3> 첨부파일</h3>
<table width="770"border="0" cellspacing="0" cellpadding="0" class="table_1">
	<colgroup>
		<col width="150px" />
		<col width="600px" />
	</colgroup>
	<tr>
		<th>첨부파일1</th>
		<td><input type="file" name="attFile1" size="85" /></td>
	</tr>
	<tr>
		<th>첨부파일2</th>
		<td><input type="file" name="attFile2" size="85" /></td>
	</tr>
	<tr>
		<th class="th_bot">첨부파일3</th>
		<td class="th_bot"><input type="file" name="attFile3" size="85" /></td>
	</tr>
</table>
</form>
<input type="button" value="저장" id="uploadBtn" />
</body>
</html>
```

- FileUploadController.java 에서
    - fileUploadAjax 주소가 들어오면
    - multipart 데이터는 @RequestParam으로 받지 못함. HttpServletRequest로 받아오고, multipart이기 때문에 MultipartHttpServletRequest로 캐스팅

```java
	@RequestMapping(value = "/fileUploadAjax", method = RequestMethod.POST, 
									produces = "text/json;charset=UTF-8")
	@ResponseBody
	// RequestParam은 텍스트 데이터만 받을 수 있기 때문에 request를 써야 함. 
	public String fileUploadAjax(HttpServletRequest request,
															 ModelAndView modelAndView) throws Throwable {
		ObjectMapper mapper = new ObjectMapper();
		HashMap<String, Object> modelMap = new HashMap<String, Object>();

		/* File Upload Logic */
		// 옆그레이드. enctype 멀티파트를 받아오기 위해 request를 
		// MultipartHttpServletRequest로 캐스팅 
		MultipartHttpServletRequest multipartRequest 
					= (MultipartHttpServletRequest) request;

		String uploadExts = CommonProperties.FILE_EXT; // 허용 확장자
		String uploadPath = CommonProperties.FILE_UPLOAD_PATH; // 파일 업로드 경로
		String fileFullName = "";
		
		File folder = new File(uploadPath);

		if (!folder.exists()) { // 폴더 존재 여부 확인
			folder.mkdir(); // 폴더 생성 - 안전장치 만들기
		}

		List<String> fileNames = new ArrayList<String>(); // 저장될 파일명을 보관
		try { // 파일 업로드는 물리 저장. 
					// 자바가 컴퓨터가 정상적으로 돈다는 것을 보장할 수 없기 때문에 
					// 파일 업로드 할 때는 항상 예외처리
			@SuppressWarnings("rawtypes")
			// 맵으로 넘어옴. 변질되면 안되기 때문에 final.
			final Map files = multipartRequest.getFileMap();
			// 파일 이름들을 가져오겠다.
			Iterator<String> iterator = multipartRequest.getFileNames();

			while (iterator.hasNext()) {
				String key = iterator.next();
				MultipartFile file = (MultipartFile) files.get(key);

				if (file.getSize() > 0) { // 파일의 내용이 존재하는지 확인
					String fileRealName = file.getOriginalFilename(); // 실제 파일명
					String fileTmpName = Utils.getPrimaryKey(); // 고유 날짜키 받기
					String fileExt = FilenameUtils.getExtension(
										file.getOriginalFilename()).toLowerCase(); // 파일확장자 추출

					if (uploadExts.toLowerCase().indexOf(fileExt) < 0) { // 확장자 확인
						throw new Exception("Not allowded file extension : " 
												+ fileExt.toLowerCase());
					} else { // 저장
						// 물리적으로 저장되는 파일명(실제파일명을 그대로 저장할지 
						// rename해서 저장할지는 협의 필요)
						fileFullName = fileTmpName + fileRealName;
						// File(경로) - 폴더
						// File(경로, 파일명) - 파일
						// new File(new File(uploadPath), fileFullName)
						// uploadPath경로의 폴더에 fileFullName 이름의 파일
						// 파일.transferTo(새파일) - 새파일에 파일의 내용을 전송
						file.transferTo(new File(new File(uploadPath), fileFullName));

						fileNames.add(fileFullName);
					}
				}
			}

			modelMap.put("result", CommonProperties.RESULT_SUCCESS);
		} catch (Exception e) {
			// 공통 Exception 처리
			e.printStackTrace();
			modelMap.put("result", CommonProperties.RESULT_ERROR);
		}

		modelMap.put("fileName", fileNames);

		return mapper.writeValueAsString(modelMap);
	}
```

- CommonProperties.java
    - 파일 업로드 경로를 설정해줘야함.
    - 허용 확장자를 설정하면 기재되어 있는 확장자 명의 파일만 업로드 가능하다.
    
    ```java
    /**
     * 파일 업로드
     */
    // 파일 업로드 경로
    public static final String FILE_UPLOAD_PATH = "/Users/paikjonghun/Desktop/MyWork/workspace/.metadata/.plugins/org.eclipse.wst.server.core/tmp0/wtpwebapps/SampleSpring/resources/upload";
    												
    	
    // 파일 다운로드 경로
    public static final String FILE_DOWNLOAD_PATH = "http://localhost:8080/sample";
    	
    // 허용파일 확장자
    public static final String FILE_EXT = "xls|ppt|doc|xlsx|pptx|docx|hwp|csv|jpg|jpeg|png|gif|bmp|tld|txt|pdf|zip|alz|7z";
    public static final String IMG_EXT = "jpg|jpeg|png|gif|bmp";
    ```
    

## 게시판 목록과 상세보기에 첨부파일 기능 추가하기

- SQL Developer에서 TB 테이블 편집 → ATT_FILE 컬럼 추가
    
    ![스크린샷 2022-03-08 오전 12.17.37.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/151ac0ec-f245-4dc2-aeb6-a0712ff2c666/스크린샷_2022-03-08_오전_12.17.37.png)
    

- atbWrite.jsp - body 안에 두 줄 추가
    
    ```java
    첨부파일 <input type="file" name="att" /><br>
    <input type="hidden" id="attFile" name="attFile">
    ```
    

- 작성 <form>의 action 변경, enctype 추가
    
    ```java
    <form action="fileUploadAjax" id="writeForm" method="post" enctype="multipart/form-data">
    ```
    

- <script> else 부분 수정
    
    ```java
    $("#writeBtn").on("click", function() {
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
    			var writeForm = $("#writeForm");
    			
    			writeForm.ajaxForm({
    				success : function(res) {
    					// 물리파일명 보관
    					if(res.fileName.length > 0) {
    						$("#attFile").val(res.fileName[0]);						
    					}
    					
    					// 글 저장
    					var params = $("#writeForm").serialize();
    					
    					$.ajax({
    						type : "post", // 전송 형태
    						url : "atbAction/insert", // 통신 주소
    						dataType : "json", // 받을 데이터 형태
    						data : params, // 보낼 데이터. 보낼 것이 없으면 안 씀
    						success : function(res) { // 성공 시 실행 함수. 인자는 받아온 데이터
    							if(res.res == "success") {
    								location.href = "atbList";
    							} else {
    								alert("작성 중 문제가 발생했습니다.");
    							}
    						},
    						error : function(request, status, error) { // 문제 발생 시 실행 함수
    							console.log(request.responseText); // 결과 텍스트
    						}
    					});
    				},
    				error : function(req) {
    					console.log(req.responseText);
    					
    				}
    			});
    			writeForm.submit(); // ajaxForm 실행	
    		}
    });
    ```
    

- Test_SQL.xml - ATT_FILE 추가
    - tbWrite 수정
    
    ```xml
    	<insert id="tbWrite" parameterType="hashmap">
    		INSERT INTO TB(TB_NO, TB_TITLE, TB_WRITER, TB_CON, ATT_FILE)
    		VALUES(TB_SEQ.NEXTVAL, #{title}, #{writer}, #{con}, #{attFile})
    	</insert>
    ```
    
    - tbUpdate 수정
    
    ```xml
    	<update id="tbUpdate" parameterType="hashmap">
    		UPDATE TB SET TB_TITLE = #{title},
                  		  TB_CON = #{con},
                  		  ATT_FILE = #{attFile}
    		WHERE TB_NO = #{no}
    	</update>
    ```
    
    - getTb 수정
    
    ```xml
    	<select id="getTb" resultType="hashmap" parameterType="hashmap">
    		SELECT TB.TB_NO, TB.TB_TITLE, TB.TB_CON, TB.TB_WRITER, M.M_NM,
    			   TB.TB_HIT, TO_CHAR(TB.TB_DT, 'YYYY-MM-DD') AS TB_DT,
    			   TB.ATT_FILE
    		FROM TB INNER JOIN M
    		                ON TB.TB_WRITER = M.M_NO
    		WHERE TB.TB_NO = #{no}
    	</select>
    ```
    
    - getTbList 수정
    
    ```xml
    	<select id="getTbList" resultType="hashmap" parameterType="hashmap">
    		SELECT T.TB_NO, T.TB_TITLE, T.M_NM, T.TB_HIT, T.TB_DT, T.ATT_FILE
    		FROM (SELECT TB.TB_NO, TB.TB_TITLE, M.M_NM, TB.TB_HIT, TO_CHAR(TB.TB_DT, 'YYYY-MM-DD') AS TB_DT, TB.ATT_FILE,
    				     ROW_NUMBER() OVER(ORDER BY TB.TB_NO DESC) AS RNUM
    			  FROM TB INNER JOIN M
    				              ON TB.TB_WRITER = M.M_NO
    			  WHERE 1 = 1
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
      	    	  ) T
    		WHERE T.RNUM BETWEEN #{startCount} AND #{endCount}
    	</select>
    ```
    

- atbList.jsp - 아이콘 img 추가하기 - ATT_FILE 값이 있으면 아이콘 붙이기
    
    ```jsx
    function drawList(list) {
    	var html = "";
    	
    	for(var data of list) {
    		html += "<tr no=\"" + data.TB_NO + "\">";
    		html += "<td>" + data.TB_NO + "</td>";
    		html += "<td>";
    		html += data.TB_TITLE;
    		if(data.ATT_FILE != null) {
    			html += "<img src=\"resources/images/attFile.png\" >";
    		}
    		html += "</td>";
    		html += "<td>" + data.M_NM + "</td>";
    		html += "<td>" + data.TB_HIT + "</td>";
    		html += "<td>" + data.TB_DT + "</td>";
    		html += "</tr>";
    	}
    	$("tbody").html(html);
    }
    ```
    

- css 추가하기 - 크기 조정
    
    ```jsx
    <style type="text/css">
    tbody img {
    	width: 12px;
    }
    </style>
    ```
    

- atb.jsp - 첨부파일 존재시 첨부파일 표시
    
    ```jsx
    <!-- 첨부파일 존재시 -->
    <c:if test="${!empty data.ATT_FILE}">
    첨부파일 : <a href="resources/upload/${data.ATT_FILE}"></a>
    </c:if>
    ```
    

- functions 태그 추가
    
    ```jsx
    <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
    ```
    

- c:set 추가
    - `c:set` => 변수 선언
    - el에서 `fn:length` => 문자열의 길이나 배열의 크기를 가져옴
    - 파일 이름을 substring 활용해서 원복시키기.
    
    ```jsx
    <c:set var="fileLength" value="${fn:length(data.ATT_FILE)}"></c:set>
    첨부파일 : <a href="resources/upload/${data.ATT_FILE}">${fn:substring(data.ATT_FILE, 20, fileLength)}</a><br>
    </c:if>
    ```
    

- download 추가
    - <a> 속성 중 `download` : 파일을 다운로드하게 한다. 만약 값이 존재하면 파일명을 값으로 변경하여 다운로드. 확장자 없이 이름이 올 경우 자동으로 파일의 확장자를 붙여줌
    
    ```jsx
    <c:if test="${!empty data.ATT_FILE}">
    <c:set var="fileLength" value="${fn:length(data.ATT_FILE)}"></c:set>
    <c:set var="fileName" value="${fn:substring(data.ATT_FILE, 20, fileLength)}"></c:set>
    첨부파일 : <a href="resources/upload/${data.ATT_FILE}" download="${fileName}">${fileName}</a><br>
    </c:if>
    ```
    

## 게시판 수정에서 첨부파일 기능 넣기

- atbUpdate.jsp - 코어태그, 펑션태그 넣기
    
    ```jsx
    <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
    <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
    ```
    

- <body>에 첨부파일 내용 추가
    
    ```jsx
    첨부파일 :
    <c:choose>
    	<c:when test="${empty data.ATT_FILE}">
    		<input type="file" name="att" ><br>
    		<input type="hidden" id="attFile" name="attFile">
    	</c:when>
    	<c:otherwise>
    		<c:set var="fileLength" value="${fn:length(data.ATT_FILE)}"></c:set>
    		<c:set var="fileName" value="${fn:substring(data.ATT_FILE, 20, fileLength)}"></c:set>
    		<span id="attName">${fileName}</span>
    		<input type="button" value="첨부파일 삭제" id="fileDelBtn">
    		<input type="hidden" id="attFile" name="attFile" value="${fileName}">
    	</c:otherwise>
    </c:choose>
    ```
    

- style 추가 - 첨부파일 삭제했을 때 보이게 하기 위해. 기본적으로 안보이게 함
    
    ```jsx
    <style type="text/css">
    #att {
    	display: none;
    }
    </style>
    ```
    

- jquery.form.js 추가
    
    ```jsx
    <script type="text/javascript" src="resources/script/jquery/jquery.form.js"></script>
    ```
    

- 클릭 이벤트 추가
    
    ```jsx
    	$("#fileDelBtn").on("click", function() {
    		$("#attName").remove();
    		$(this).remove();
    		$("#att").show();
    	});
    ```
    

- 수정 <form> 수정
    
    ```jsx
    <form action="fileUploadAjax" id="updateForm" method="post" enctype="multipart/form-data">
    ```
    

- 수정 완료 부분 수정
    
    ```jsx
    $("#updateBtn").on("click", function() {
    		$("#con").val(CKEDITOR.instances['con'].getData());
    		if(checkEmpty("#title")) {
    			alert("제목을 입력하세요.")
    			$("#title").focus();
    		} else if(checkEmpty("#con")) {
    			alert("내용을 입력하세요.")
    			$("#con").focus();
    		} else {
    			var updateForm = $("#updateForm");
    			
    			updateForm.ajaxForm({
    				success : function(res) {
    					// 물리 파일명 보관
    					if(res.fileName.length > 0) {
    						$("#attFile").val(res.fileName[0]);						
    					}
    					
    					// 글 수정
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
    				},
    				error : function(req) {
    					console.log(req.responseText);
    					
    				}
    			});
    			updateForm.submit(); // ajaxForm 실행	
    		}
    });
    ```