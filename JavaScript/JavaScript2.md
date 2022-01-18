## JS Event

이벤트는 HTML 요소에 발생하는 것. JavaScript가 HTML 페이지에서 사용될 때, JavaScript는 이벤트에 반응할 수 있다.

JavaScript 코드를 활용해서 HTML 요소의 다양한 속성값을 다룰 수 있다.


- `onclick` : 요소를 클릭했을 때
- `onmouseover` : 마우스가 요소 위에 올라갔을 때
- `onmouseout` : 마우스가 요소 위에서 나갔을 때
- `ondblclick` : 요소에 더블클릭이 발생했을 때
- `onfocus` : 요소에 포커스가 잡힐 때
- `onchange` : focus가 벗어나면서 가지고 있던 value가 변경되었을 경우
- `onkeypress` : 키를 누른 상태, 누르고 있는 동안 실행이 됨. (the event occurs when the user presses a key)
- `onkeydown` : 키를 누른 상태, 누르고 있는 동안 실행이 됨. (the event occurs when the user is pressing a key)
- `onkeyup` : 키를 떼었을 때 - 1회성 (the event occurs when the user releases a key)

```jsx
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
#box {
	display: inline-block;
	width: 150px;
	height: 150px;
	background-color: orange;
	vertical-align: top;
}

#a {
	color: red;
	font-weight: bold;
	display: none;
}

#txt:focus {
	outline: 3px solid rgba(40, 40, 255, 0.4);
}

</style>
<script type="text/javascript">
var colors = ["orange", "silver", "blue"];
var pos = 0;
function test(stat) {
	console.log(stat); // 함수 호출되면서 어떤 이벤트가 발생했는지 콘솔에서 확인할 수 있다.
	var box = document.getElementById("box");
	switch(stat) {

	// onclick 이벤트가 발생하면 요소의 배경색 바꾸기
	case "click" :
		pos++;
		if(pos == colors.length) {
			pos = 0;
		}
		box.style.backgroundColor = colors[pos];
		break;

	// onmouseover 이벤트가 발생하면 요소의 outline 값 주기
	case "mouseover" :
		box.style.outline = "3px solid rgba(255, 70, 70, 0.4)";
		break;

	// onmouseout 이벤트가 발생하면 요소의 outline 값 빼기
	case "mouseout" :
		box.style.outline = "none"; // 속성을 아예 지우고 싶으면 스타일 자체를 ""로 하거나 
																// none 같은 직접 설정으로 속성을 남길 수 있다.
		break;

	// ondblclick 이벤트가 발생하면 요소의 width값, height값 변경하기
	case "dblclick" :
		// clientWidth, clientHeight : 해당 요소의 너비와 높이를 가져온다.
		console.log(box.clientWidth);
		console.log(box.clientHeight);
		box.style.width = box.clientWidth + 50 + "px";
		box.style.height = box.clientHeight + 50 + "px";
		break;
	}
}

function testFocus() { 
	// onfocus 이벤트가 발생하면 요소의 value값을 제거한다.
	var txt = document.getElementById("txt");
	txt.value = "";
}

function testChange() {
	// onchange 이벤트가 발생하면 콘솔창에 change 출력
	console.log("change");
	var txt = document.getElementById("txt");
	var a = document.getElementById("a");
	// id="txt"인 요소의 value의 길이가 4글자 미만이면 보이게 한다. 아니면 안 보이게 한다.
	if(txt.value.length < 4) {
		a.style.display = "block"; 
	} else {
		a.style.display = "";
	}
}

function testKey(event) {
	// console.log(event)는 어떤 이벤트가 발생했는지 콘솔창에 출력해줌.
	console.log(event);
	var box = document.getElementById("box");
	switch(event.key) {
	case "+" :
		box.style.width = box.clientWidth + 2 + "px";
		box.style.height = box.clientHeight + 2 + "px";
		break;
	case "-" :
		if(box.clientWidth - 2 >= 0) {
			box.style.width = box.clientWidth - 2 + "px";
			box.style.height = box.clientHeight - 2 + "px";	
		}
		break;	
	}
}

</script>
</head>
<body onkeypress="testKey(event);">
<div id="box" 
	 onclick="test('click');" 
	 onmouseover="test('mouseover');" 
	 onmouseout="test('mouseout');"
	 ondblclick="test('dblclick');"></div>

<br/>

<input type="text" id="txt" placeholder="아무거나 입력 최소 4자 이상" size="30"
	   onfocus="testFocus();"
	   onchange="testChange();">
<div id="a">최소 4자 이상이 되어야 함!</div>

</body>
</html>
```