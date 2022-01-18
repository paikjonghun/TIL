
## 목차
- [목차](#목차)
- [JavaScript Event](#javascript-event)
- [문자열 처리](#문자열-처리)
- [문자열 찾기](#문자열-찾기)
- [숫자 처리](#숫자-처리)

## JavaScript Event

> HTML 이벤트는 HTML 요소에서 발생하는 것. JavaScript가 HTML 페이지에서 사용될 때, JavaScript는 이런 이벤트에 반응할 수 있다. JavaScript 코드를 활용해서 요소의 속성값을 다룰 수 있다.
> 

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
/* :focus - 해당 요소가 포커스 되었을 때 */
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
		// 해당 요소의 너비와 높이를 50px 증가.
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
	// onkeypress
	switch(event.key) {
	// 눌려진 키가 +일 경우 요소의 높이와 너비를 2px 증가
	case "+" :
		box.style.width = box.clientWidth + 2 + "px";
		box.style.height = box.clientHeight + 2 + "px";
		break;
	// 눌려진 키가 -일 경우 요소의 높이와 너비를 2px 감소
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
<!-- event는 어떤 이벤트가 발생했는지를 인자값으로 넣어줌 -->
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

## 문자열 처리

- `length` : 길이를 찾는 속성
    - 자바에서는 `length()` 메소드였지만 자바스크립트에서는 `.length`를 활용하면 됨
- 이스케이프 문자, 역슬래시( \ )
    - 역슬래시 다음 글자를 String으로 인식한다. 큰따옴표, 작은따옴표, 역슬래시를 문자열로 쓰고 싶으면, 앞에 역슬래시를 붙여준다.
    
    ```jsx
    let text = "We are the so-called \"Vikings\" from the north.";
    ```
    
    - \n : 개행 (New Line)

- `substring(start, end)` : 숫자 1개일 경우 숫자 이상, 숫자 2개일 경우 숫자1 이상 숫자2 미만의 문자열을 자른다.
    
    ```jsx
    console.log(s.substring(3));
    console.log(s.substring(3, 4));
    ```
    

- `replace()` : 첫 번째 문자를 두 번째 문자로 바꾼다. 기본적으로 하나만 바꾸기 때문에 해당하는 전체 문자를 바꾸려면 정규식 사용해야함.
    
    ```jsx
    console.log(s.replace("l", "k"));
    ```
    
    - replace 정규식 : `/단어/옵션`
        - 옵션 g : 모든 것
        - 옵션 i : 대소문자 구분 없음
        
        ```jsx
        console.log(s.replace(/l/g, "k"));
        ```
        

- `trim()` : 문자열의 앞, 뒤 공백을 제거한다.
    
    ```jsx
    var s2 = "          Hi      !             ";
    console.log("[" + s2.trim() + "]");
    ```
    

- `padStart`, `padEnd` : 자리수를 정하고 무엇으로 채울지
    
    ```jsx
    var s3 = "14";
    console.log(s3.padStart(5, "0"));
    console.log(s3.padEnd(5, "0"));
    ```
    
- `charAt()` : 해당하는 인덱스의 글자 가져오기
    
    ```jsx
    console.log(s.charAt(3));
    ```
    

- `charCodeAt()` : 해당 인덱스 글자의 코드 가져오기
    
    ```jsx
    console.log(s.charCodeAt(0));
    ```
    
    - 해당 인덱스 글자를 배열로 가져올 수는 있음. 하지만 실제로 배열은 아니기 때문에 문자를 바꾸는 것은 코드 문제는 없지만 작동 안됨.
        
        ```jsx
        console.log(s[3]); 
        
        s[3] = "a"; // 작동 안됨
        ```
        

- `split()` : 해당 값을 기준으로 자르고 배열로 만들기
    
    ```jsx
    var s4 = "사과,배,감";
    var temp = s4.split(",");
    console.log(temp);
    ```
    

## 문자열 찾기

- `indexOf()` :
    
    ```jsx
    // 값의 인덱스 위치를 찾겠다.
    console.log(s.indexOf("l"));
    
    // 인덱스 중 숫자 이상에서 값의 인덱스 위치를 찾겠다.
    console.log(s.indexOf("l", 5));
    
    // 해당 값이 없으면 -1 출력
    console.log(s.indexOf("x"));
    
    // 값의 인덱스 위치를 뒤에서부터 찾겠다.
    console.log(s.lastIndexOf("l"));
    ```
    

- `startsWith()` : 해당 값으로 문자열 시작을 하면 true, 아니면 false
    
    ```jsx
    console.log(s.startsWith("Hi"));
    ```
    

- `endsWith()` : 해당 값으로 문자열이 끝나면 true, 아니면 false
    
    ```jsx
    console.log(s.endsWith("Hi"));
    ```
    

## 숫자 처리

- `isNaN` : 해당 값이 숫자가 아니면 true, 맞으면 false.
    
    ```jsx
    isNaN(10 * "abc") == true
    ```
    

- `infinity` : 숫자를 0으로 나누면 나타남

- `toString(숫자)` : 해당 진수로 변환
    
    ```jsx
    var b = 12;
    console.log(b.toString(16));
    console.log(b.toString(8));
    console.log(b.toString(2));
    ```
    

- `Math.round` : 반올림
    
    ```jsx
    console.log(Math.round(3.1415));
    ```
    

- `Math.ceil` : 올림
    
    ```jsx
    console.log(Math.floor(3.1415));
    ```
    

- `Math.floor` : 내림
    
    ```jsx
    console.log(Math.ceil(3.1415));
    ```
    

- `Math.abs` : 절대값
    
    ```jsx
    console.log(Math.abs(-7));
    ```
    

- `Math.random()` - 0 이상 1 미만의 랜덤값 호출
    
    ```jsx
    console.log(Math.floor(Math.random() * 10) + 1);
    ```