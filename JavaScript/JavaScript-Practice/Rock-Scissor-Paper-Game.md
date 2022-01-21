```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>가위 바위 보 게임</title>
<style type="text/css">


img {
	width: 320px;
	margin: 5px;
}

input {
	width: 100px;
	height: 100px;
	font-size: 20px;
	background-color: yellow;
	border-color: lime;
	margin: 5px;
}

#start {
	background-color: orange;
	display: inline-block;
	vertical-align: top;
}

#txt {
	resize: none;
	width: 320px;
	height: 50px;
	font-size: 40px;
	margin-left: 5px;
}

</style>
<script type="text/javascript">
var interval = null;
var imgs = ["scissor.png", "rock.png", "paper.png"];
var pos = 0;
var o = 1.0;
var win = 0;
var lose = 0;
var draw = 0;

// 시작 버튼을 클릭하면 실행
function play() {
	pos++;
	if(pos == imgs.length) {
		pos = 0;
	}
	
	document.getElementById("imgBox").src = imgs[pos];
}

// 가위 바위 보 중 하나를 클릭하면 실행
function stop(obj) {
	clearInterval(interval);
	interval = null;
	var random = Math.floor(Math.random() * 3);
	document.getElementById("imgBox").src = imgs[random];
	
	switch(obj) {
	case "0" : if(random == 0) {
			       draw++;
			   } else if(random == 1) {
				   lose++;
			   } else {
				   win++;
				}
		break;
	case "1" : if(random == 0) {
		           win++;
			   } else if(random == 1) {
				   draw++;
		   	   } else {
				   lose++;
			   }
		break;
	case "2" : if(random == 0) {
				  lose++;
			   } else if(random == 1) {
				  win++;
		   	   } else {
				  draw++;
			   }
	}
	
	
	var txt = document.getElementById("txt");
	txt.innerHTML = win + "승" + draw + "무" + lose + "패";
	
	var scissor = document.getElementById("scissor");
	scissor.disabled = true;
	var rock = document.getElementById("rock");
	rock.disabled = true;
	var paper = document.getElementById("paper");
	paper.disabled = true;
	
}

// 게임 시작
function start() {
	var scissor = document.getElementById("scissor");
	scissor.disabled = false;
	var rock = document.getElementById("rock");
	rock.disabled = false;
	var paper = document.getElementById("paper");
	paper.disabled = false;
	
	interval = setInterval(play, 70);
}



</script>


</head>
<body>

<textarea rows="5" cols="20" id=txt readonly="readonly" placeholder="시작 버튼을 누르세요."></textarea>


<div>
<img alt="가위바위보" src="rock.png" id="imgBox">
<input type="button" value="시작" id="start" onclick="start()">
</div>

<input type="button" value="가위" id="scissor" onclick="stop('0')" disabled="disabled">
<input type="button" value="바위" id="rock" onclick="stop('1')" disabled="disabled">
<input type="button" value="보" id="paper" onclick="stop('2')" disabled="disabled">


</body>
</html>
```