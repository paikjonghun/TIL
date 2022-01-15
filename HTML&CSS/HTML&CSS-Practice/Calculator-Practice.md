## JS 활용해 계산기 만들기

<img src=./src/calculator.png width="70%">

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
body {
	font-family: Apple SD 산돌고딕 Neo
}
#now {
	width: 135px;
	height: 15px;
	font-size: 13px;
	text-align: right;
	color: gray;
}
#input {
	width: 135px;
	height: 30px;
	font-size: 15px;
	text-align: right;
	font-weight: 300;
}

input {
	width: 32px;
	height: 32px;
	margin-top: 4px;
	margin-left: 1px;
}
</style>

<script type="text/javascript">
var i;
var s;
var sign_var = "";
var res_sign_var = "";
var rsave;
var rsave2;

function input(obj) {
	i = document.getElementById("input");
	if(i.value == 0) {
		i.value = obj.value;
	} else {
		i.value += obj.value;	
	}
	console.log(i.value);
}


function sign(obj) {
	s = document.getElementById("now")
	if(sign_var == "") {
		s.value = i.value;
		console.log(i.value);
	} else {
		switch(sign_var) {
		case "+" : s.value = (s.value * 1) + (i.value * 1)
		break;
		case "-" : s.value = (s.value * 1) - (i.value * 1)
		break;
		case "*" : s.value = (s.value * 1) * (i.value * 1)
		break;
		case "/" : s.value = (s.value * 1) / (i.value * 1)
		}
	}
	sign_var = obj.value;
	i.value = 0;
}

function result(obj) {
	
	if(s.value != 0) {
		rsave = i.value;
		rsave2 = s.value;
		s.value = 0;
		res_sign_var = sign_var;
		sign_var = "";
	}

	
	switch(res_sign_var) {
	case "+" : i.value = (rsave2 * 1) + (rsave * 1)
	break;
	case "-" : i.value = (rsave2 * 1) - (rsave * 1)
	break;
	case "*" : i.value = (rsave2 * 1) * (rsave * 1)
	break;
	case "/" : i.value = (rsave2 * 1) / (rsave * 1)
	}
	rsave2 = i.value
}

function init(obj) {
	i.value = 0;
	s.value = 0;
	sign_var = "";
	res_sign_var = 0;
	rsave = 0;
	rsave2 = 0;
	
}



</script>
</head>
<body>

<div>
	<input type="text" id="now" value="0">
</div>
<div>
	<input type="text" id="input" value="0">
</div>
<div>
	<input type="button" id="1" value="1" onclick="input(this);">
	<input type="button" id="2" value="2" onclick="input(this);">
	<input type="button" id="3" value="3" onclick="input(this);">
	<input type="button" id="+" value="+" onclick="sign(this);">
</div>
<div>
	<input type="button" id="4" value="4" onclick="input(this);">
	<input type="button" id="5" value="5" onclick="input(this);">
	<input type="button" id="6" value="6" onclick="input(this);">
	<input type="button" id="-" value="-" onclick="sign(this);">
</div>
<div>
	<input type="button" id="7" value="7" onclick="input(this);">
	<input type="button" id="8" value="8" onclick="input(this);">
	<input type="button" id="9" value="9" onclick="input(this);">
	<input type="button" id="*" value="*" onclick="sign(this);">
</div>
<div>
	<input type="button" id="init" value="C" onclick="init(this)">
	<input type="button" id="0" value="0" onclick="input(this);">
	<input type="button" id="=" value="=" onclick="result(this);">
	<input type="button" id="/" value="/" onclick="sign(this);">
</div>




</body>
</html>
```