## 목차
- [목차](#목차)
- [jQuery Animate](#jquery-animate)
- [DOM insertion](#dom-insertion)

## jQuery Animate

- .지정애니메이션(속도, 함수)
    - 속도만큼 실행. 종료 시 함수 실행(선택)

- `.animate({옵션}, 속도, 함수)`
    - 속도만큼 실행. 종료 시 함수 실행(선택)
    - 옵션은 현재 상태에서 되고자 하는 형태를 지정
    - 옵션은 정해진 값 이외에도 현재값을 기준으로 누적형태를 취할 수 있다.
    
    ```jsx
    $("#bigBtn").on("click", function() {
    		
    	$("#box").animate({
    		// 누적하며 적용할 수도 있고, 고정값으로 적용할 수도 있음.
    		width: "+=50px",
    		height: "+=50px",
    			
    	}, 3000);
    });
    ```
    

- `.stop()`
    - 현재 진행중인 애니메이션 1건만 멈출 수 있음.
    
    ```jsx
    $("#stopBtn").on("click", function() {
    		$("#box").stop();
    });
    ```
    

- `.delay(시간)` : 앞의 내용 실행 완료 후 다음 내용은 지정 시간 이후에 실행
- `.slideDown()` : 위에서 내려오면서 객체 등장

```jsx
$("#dBtn").on("click", function() {
	// 객체가 fadeout 되었다가 2초 뒤에 위에서 내려오면서 등장하고 1초 뒤에 width 300px, 
	// height 100px이 된다.
	$("#box").fadeOut().delay(2000).slideDown().delay(1000).animate({
		width: "300px",
		height: "100px"
	});
});
```

## DOM insertion

- `.append(값)` : 엔티티에 값을 뒤에 누적

```jsx
$("#addBtn1").on("click", function() {
	cnt++;
		
	var html = "";
	html += "<tr>        ";
	html += "<td>" + cnt + "</td>  ";
	html += "<td>test" + cnt + "</td>";
	html += "</tr>       ";
		 
	$("tbody").append(html);		
});
```

- `.prepend(값)` : 엔티티에 값을 앞에 누적. innerHTML 사용할 때보다 훨씬 간편함

```jsx
$("#addBtn2").on("click", function() {
	cnt++;
		
	var html = "";
	html += "<tr>        ";
	html += "<td>" + cnt + "</td>  ";
	html += "<td>test" + cnt + "</td>";
	html += "</tr>       ";
	
	$("tbody").prepend(html);
		
		
});
```

- `.html()`, `.html(값)` : 엔티티를 가져오거나 엔티티에 값을 넣는다.

```jsx
$("#changeBtn").on("click", function() {
	cnt++;
		
	var html = "";
	html += "<tr>        ";
	html += "<td>" + cnt + "</td>  ";
	html += "<td>test" + cnt + "</td>";
	html += "</tr>       ";
		
	console.log($("tbody").html());
	$("tbody").html(html);
		
		
});
```

- `.empty()` : 엔티티를 지운다.

```jsx
$("#resetBtn").on("click", function() {
	cnt = 0;
	// 내용을 비우는 첫번째 방법 
	// $("tbody").html("");
		
	// 두번째 방법. empty() : 엔티티를 지운다.
	$("tbody").empty();
		
});
```

- `.remove()` : 선택한 객체 자체를 삭제

```jsx
$("tbody").on("click", "tr", function() {
		// remove는 내가 사라진다. empty는 내 내용이 사라진다.
		
		if(confirm("삭제하시겠습니까?")) {
			$(this).remove();
		}
	});
```

- 팝업창 만들기

```jsx
$("#popupBtn").on("click", function() {
		var html = "";
		
		html += "<div id=\"pop\"></div>";
		html += "<div id=\"bg\"></div>";
		
		$("body").append(html);
		
		$("#pop, #bg").hide();
		$("#pop, #bg").fadeIn();
		
	});
```

- 팝업창 닫기

```jsx
$("body").on("click", "#pop, #bg", function() {
		$("#pop, #bg").fadeOut(function() {
			$("#pop, #bg").remove();
		});
	});
```
