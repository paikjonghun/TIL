

## JS - 숫자 야구 게임 만들기

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>숫자 야구</title>
<style type="text/css">

#input {
	resize: none;
	width: 190px;
	height: 190px;
	font-size: 15px;
}

input {
	display: inline-block;
	width: 60px; 
	height: 60px;
	vertical-align: top;
	margin-right: 3px;
	margin-bottom: 5px;
}

</style>

<script type="text/javascript">
var ran_arr = [];
var countNumb = "";
var cycle = 1;

// 랜덤 숫자 세 개 뽑기
for(var i = 0; i < 3; i++) {
	var ran_num = Math.floor(Math.random() * 9) + 1;
	ran_arr[i] = ran_num;
	for(var j = 0; j < i; j++) {
		if(ran_arr[i] == ran_arr[j]) {
			i--;
			break;
		}
	}
}

console.log(ran_arr);

// 숫자 세 개 입력 받기
function clickNumb(numb) {
	
	var btnDis = document.getElementById("btn" + numb);
	var inputNum = document.getElementById("input");
	btnDis.disabled = true;
	
	inputNum.value += numb;
	countNumb += numb;
	console.log(countNumb);
	
	if(countNumb.length == 3) {
		var s = 0;
		var b = 0;

		for(var i = 0; i < 3; i++) {
			for(var j = 0; j < 3; j++) {
				if(countNumb[i] == ran_arr[j] && i == j) {
					s++;
				} else if (countNumb[i] == ran_arr[j]) {
					b++;
				}
			}
		}
		var o = 3 - (s + b);
		console.log(s, b, o);
		inputNum.value += "\n" + cycle + "회 결과 :" + s + "S " + b + "B " + o + "O" + "\n" + "\n";
		
		for(var i = 0; i < 3; i++) {
			var btnDis = document.getElementById("btn" + countNumb[i]);
			btnDis.disabled = false;
		}
		
		cycle++;
		countNumb = "";
		
		if(s == 3) {
			inputNum.value += "승리!";
			for(var i = 1; i < 10; i++) {
				var btnDis = document.getElementById("btn" + i);
				btnDis.disabled = true;
			}
		}
	}
	
	if(cycle == 10 && s != 3) {
		inputNum.value += "패배!";
		for(var i = 1; i < 10; i++) {
			var btnDis = document.getElementById("btn" + i);
			btnDis.disabled = true;
		}
	}
}

// 리셋
function reset() {
	var inputNum = document.getElementById("input");
	inputNum.value = "";
	cycle = 1;
	conutNumb = "";
	for(var i = 1; i < 10; i++) {
		var btnDis = document.getElementById("btn" + i);
		btnDis.disabled = false;
	}
	
	
	// 랜덤숫자 다시 뽑기
	for(var i = 0; i < 3; i++) {
		var ran_num = Math.floor(Math.random() * 9) + 1;
		ran_arr[i] = ran_num;
		for(var j = 0; j < i; j++) {
			if(ran_arr[i] == ran_arr[j]) {
				i--;
				break;
			}
		}
	}
	
	
}

</script>
</head>
<body>

<div>
	<textarea id="input" rows="1" cols="10" readonly="readonly"></textarea>
	<input id="btn" type="button" value="reset" onclick="reset();">
</div>
<div>
	<input id="btn1" type="button" value="1" onclick="clickNumb('1');">
	<input id="btn2" type="button" value="2" onclick="clickNumb('2');">
	<input id="btn3" type="button" value="3" onclick="clickNumb('3');">
</div>
<div>
	<input id="btn4" type="button" value="4" onclick="clickNumb('4');">
	<input id="btn5" type="button" value="5" onclick="clickNumb('5');">
	<input id="btn6" type="button" value="6" onclick="clickNumb('6');">
</div>
<div>
	<input id="btn7" type="button" value="7" onclick="clickNumb('7');">
	<input id="btn8" type="button" value="8" onclick="clickNumb('8');">
	<input id="btn9" type="button" value="9" onclick="clickNumb('9');">
</div>

</body>
</html>
```