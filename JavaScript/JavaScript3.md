## 목차
- [목차](#목차)
- [배열](#배열)
- [날짜](#날짜)
- [Timing](#timing)
- [3초마다 이미지 바꾸기](#3초마다-이미지-바꾸기)

## 배열

- `sort()` : 배열을 정렬한다.
    - 기본적으로 정렬 함수는 값을 문자열로 정렬. 숫자를 문자열로 정렬하면 "25"가 "100"보다 뒤에 있는데, 이는 "2"가 "1"보다 크기 때문.
    
    ```jsx
    tmp.sort();
    ```
    

- `join()` : 배열을 문자열로 바꾸되 구분자를 지정할 수 있다. 기본 구분값은 컴마(,)
    
    ```jsx
    console.log(tmp);
    console.log(tmp.join());
    console.log(tmp.join("^_^"));
    ```
    

- `push()` : 끝에 새 요소를 추가. list().add 랑 같은 기능.
    
    ```jsx
    tmp.push("Hi");
    ```
    

- `pop()` : 배열에서 마지막 요소를 제거.
    
    ```jsx
    tmp.pop();
    ```
    

- `shift()` : 배열 첫번째 요소를 꺼내오면서 제거. 수학에 shift 연산에서 착안해서 만듬.
    
    ```jsx
    var a = tmp.shift();
    console.log(a); // 배열의 첫번째 요소를 콘솔에 출력
    console.log(tmp); // 배열의 첫번째 요소 제거한 것을 콘솔에 출력
    ```
    

- `unshift()` : 배열 앞에 추가.
    
    ```jsx
    tmp.unshift("사과");
    console.log(tmp);
    ```
    

- `splice()` : 배열에 새로운 요소를 추가.
    - `splice(위치, 개수, 값들...)` : 해당 위치에 개수만큼 삭제 후 값들을 순차적으로 추가한다. 추가, 수정, 제거, 끼워넣기 가능.
    - **array에서 가장 중요한 메소드**
    
    ```jsx
    tmp.splice(1, 0, "귤"); // 1번 인덱스에 귤 추가.
    console.log(tmp);
    
    tmp.splice(1, 2); // 1번 인덱스부터 2개 삭제.
    console.log(tmp);
    
    tmp.splice(0, 1, "멜론"); // 값 변경도 가능 - 0번 인덱스부터 1개 삭제 후 멜론 추가
    console.log(tmp);
    ```
    

## 날짜

- Date Object. 브라우저 상에서 시간을 가져옴. 사용자 컴퓨터 시간을 가져올 수 밖에 없음.
- timestamp : 1970년 1월 1일 00:00:00 (UTC)부터 시작해서 특정 시점까지 얼마나 많은 초 단위의 시간이 지났는지를 통해 시점을 표기하는 방식. POSIX 시간이나 Epoch 시간이라고 부르기도 한다.
    
    ```jsx
    var d = new Date();
    console.log(d); // 한국 표준시
    console.log(d.toUTCString()); // 그리니치 천문대 기준 시간
    ```
    

- Get Methods - 연, 월, 일, 시, 분, 초, 밀리초, 타임스탬프, 요일 등을 가져온다.
    
    ```jsx
    console.log(d.getFullYear()); // 연도
    console.log(d.getMonth() + 1); // 월을 인덱스로 출력해주기 때문에 +1 1월(0), 2월(1), ...
    console.log(d.getDate()); // 날짜
    console.log(d.getDay()); // 요일을 인덱스로 출력해준다. 일(0), 월(1), 화(2), ...
    console.log(d.getHours()); // 시간
    console.log(d.getMinutes()); // 분
    console.log(d.getSeconds()); // 초
    console.log(d.getMilliseconds()); // 밀리초
    console.log(d.getTime()); // 타임스탬프
    ```
    

## Timing

- `setTimeout()` : 지정된 시간동안 함수 실행
    - `setTimeout(함수, 시간, 값들...)`
    - 시간 뒤에 함수 실행. 시간단위는 밀리초. 1s = 1000ms.
    - 단발성 이벤트를 만들 때 쓰는 것이 타임아웃.
    - 값들을 함수에 전달. 값을 입력하는 것은 선택 사항.
    - 타임아웃은 이 함수를 이 시간 뒤에 실행하겠다. 라는 작업을 만드는 것임.
    
    ```jsx
    setTimeout(function(msg) {
    	alert(msg + "3초지났다.");	
    }, 3000, "Hi!");
    
    function test() {
    	alert()
    }
    ```
    

- `setInterval()` : 계속해서 함수의 실행을 반복
    - `setInterval(함수, 시간, 값들...)`
    - 시간마다 함수 실행. 시간 단위는 밀리초. 1s = 1000ms
    - 값들을 함수에 전달한다. 값을 입력하는 것은 선택 사항.
    
    ```jsx
    setInterval(function() {
    	d = new Date;
    	var time = d.getHours().toString().padStart(2, "0")
    			   + ":" + d.getMinutes().toString().padStart(2, "0")
    			   + ":" + d.getSeconds().toString().padStart(2, "0");
    	document.getElementById("txt").value = time;
    }, 1000);
    ```
    

- `clearTimeout(타임아웃객체)` : 해당 타임아웃 객체를 취소 및 제거한다.
    
    ```jsx
    var timeout = setTimeout(function(msg) {
    			  	alert(msg + "3초지났다.");	
    			  }, 3000, "Hi!");
    
    function helpMe() {
    	clearTimeout(timeout);
    }
    ```
    

- `clearInterval(타임아웃객체)` : setInterval() 메서드에 지정된 함수의 실행을 중지한다.
    
    ```jsx
    function pauseImg() {
    	if(interval == null) {
    		interval = setInterval(play, 3000);
    	} else {
    		clearInterval(interval);
    		interval = null;
    	}
    }
    ```
    

## 3초마다 이미지 바꾸기

```jsx
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
#imgBox {
	width: 250px;
}
</style>

<script type="text/javascript">
var interval = null;
var imgs = ["plant.jpg", "sun.png", "tree.png"];
var pos = 0;
var o = 1.0;

interval = setInterval(play, 3000); // 3초마다 play 함수 실행

function play() {
	pos++;
	if(pos == imgs.length) {
		pos = 0;
	}
	document.getElementById("imgBox").src = imgs[pos];
	fade(); // fade 함수 호출
}

function fade() { // fade 효과를 넣는 함수
	// Fade in
	for(var i = 1; i <= 10; i++) {
		setTimeout(function(val) {
			document.getElementById("imgBox").style.opacity = 0.0 + 0.1 * val;
		}, 100 * i, i); // 0.1초부터 1초까지, 투명도는 0.1부터 1까지 진행됨
	}

	// Fade out
	for(var i = 1; i <= 10; i++) {
		setTimeout(function(val) {
			document.getElementById("imgBox").style.opacity = 1.0 - 0.1 * val;
		}, 2000 + 100 * i, i); // 2.1초부터 3초까지, 투명도는 0.9부터 0까지 진행됨
	}
}

function pauseImg() { // pause 버튼 클릭하면 호출
	if(interval == null) {
		interval = setInterval(play, 3000); // 3초마다 play함수 호출
	} else {
		clearInterval(interval); // interval 멈춤
		interval = null;
	}
}
</script>
</head>

<body>
<input type="button" value="pause" onclick="pauseImg();">

<br/>

<img alt="이미지들" src="plant.jpg" id="imgBox" />
<script type="text/javascript">
fade(); // 시작하면 fade 함수 1회 호출
</script>
</body>
</html>
```