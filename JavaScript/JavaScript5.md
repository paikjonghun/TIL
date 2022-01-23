## 목차
- [목차](#목차)
- [JS Objects](#js-objects)
- [JS Location](#js-location)
- [JS onload Event](#js-onload-event)
- [JS Function Call](#js-function-call)
- [HTML DOM](#html-dom)


## JS Objects

- 생성과 수정
    - 자바에서 맵을 쓸 때 키 밸류의 형태를 취했던 것과 비슷하다. bean 하고도 비슷하다.

```jsx
**var** a = {
// 데이터를 중괄호로 묶고, key : value로 구성. 구분은 컴마로 구분
no : 1,
name : "홍길동"
}

console.log(a.no + "\t" + a.name);

a.name = "김철수"; // 홍길동을 김철수로 바꾸기

console.log(a.no + "\t" + a.name);
// undefined가 나옴. == undefined 하면 undefined인지 확인할 수 있음. 
// 근데 typeof 써야함. typeof 자료형 체크
console.log(typeof a.age == "undefined");
console.log(typeof a.age);
```



- 추가
    - 자바에서 bean은 한번 선언하면 고정이었는데, 자바의 맵같은 경우는 넣은 만큼 계속 들어간다. 그래서 맵과 비슷하다. 필요할 때 넣으면 됨. 빈보다는 맵에 가깝다.

```jsx
a.age = 30;
console.log(a.age);
console.log(a); // (a) 쓰면 다 나옴.
console.log(a["name"]); // 김철수
```


- 삭제

```jsx
delete a.age; // 해당 키밸류 삭제
console.log(a.age);
```


- 배열 활용

```jsx
var list = new Array();

for(var i = 10; i > 0; i--) {
	var d = {
			no : i,
			title : "Test" + i
	}
	list.push(d);
}

for(var d of list) {
	console.log(d.no + "\t" + d.title);
}
```


## JS Location

- `location.href` : 지정된 경로로 이동

```jsx
function go() {
	location.href = "https://www.google.com";
// 앵커같이 이동시켜주는 역할을 하고 있음. 앵커는 이동을 하나로 고정시키는데. 
// 자바스크립트를 이용해서 로케이션 이동할 때는 갖고 있는 상태값에 따라
// 다르게 이동시킬 수 있음. 로케이션은 단순 이동이기 때문에 권장하지는 않음. 
}
```

- `location.reload()` : 페이지 새로고침

```jsx
function r() {
	location.reload();
}
```

## JS onload Event

- `window.onload` : 브라우저에 해당 내용(HTML)을 다 읽었을 때

```jsx
// 프로그램은 순차적으로 실행되기 때문에, 아직 txt를 못찾음.
// 주의사항 : onload가 여러개 존재할 경우 마지막 것만 적용
window.onload = function() {
	document.getElementById("txt").value += "초기값 셋팅";
}
window.onload = function() {
	document.getElementById("txt").value += "초기값 셋팅2";
}
```


## JS Function Call

```jsx
var test = function(msg) {
	alert("Hi!" + this.name + msg);
	// call 사용 시 this는 call에서 불러온 객체를 의미.
}

// call(객체, 값들...) : 활용 객체를 지정하고 값들을 인자에 순차적으로 넣는다.
test.call(a, "~~~"); // .call() : 변수에 담겨있는 함수를 실행
```


## HTML DOM

- DOM : Document Object Model
- HTML DOM을 사용하여 자바스크립트는 HTML 문서의 모든 요소에 접근하고 변경할 수 있다.

- `innerHTML` : 문서의 내용을 추가하고 싶으면, 문서를 작성해야함.

```jsx
var num = 0;

function add() {
	num++;
	
	var html = "<div id=\"" + num + "\">" + num + "번</div>";
	
	// innerHTML : 해당 객체의 엔티티를 가져오거나 넣어줄 때 사용
	// += : 기존 엔티티를 가져와서 추가를 하겠다.
	document.getElementById("a").innerHTML += html;
}
```

- 기존 엔티티의 앞에서부터 추가

```jsx
function add2() {
	num++;
	
	var html = "<div id=\"" + num + "\">" + num + "번</div>";
	// 기존 엔티티의 앞에서부터 추가하겠다.
	document.getElementById("a").innerHTML = 
			html + document.getElementById("a").innerHTML; 
}
```

- 내용 지우기

```jsx
function clr() {
	num = 0;
	document.getElementById("a").innerHTML = "";
}
```

- 팝업창 만들기 - 새로운 화면 요소를 기존 요소에 추가하기

```jsx
function pop() {
	var html = "";
	
	html += "<div id=\"pop\" onclick=\"c();\"></div>";
	html += "<div id=\"bg\" onclick=\"c();\"></div>";
	
	document.body.innerHTML += html;
}
```

- 팝업창 닫기 - `remove()` : 화면 요소를 제거

```jsx
function c() {
	// remove() : 화면요소를 제거
	document.getElementById("pop").remove();
	document.getElementById("bg").remove();
}
```

- `createHTML()` 도 있는데, 그냥 innerHTML이 낫다. create는 코드를 더 써줘야함.