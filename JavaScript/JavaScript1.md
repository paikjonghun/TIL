## 목차

- [목차](#목차)
- [JavaScript](#javascript)
  - [JavaScript 기능](#javascript-기능)
  - [사용 방법](#사용-방법)
  - [Console에 Hello World! 찍기](#console에-hello-world-찍기)
  - [경고창에서 Hello World! 찍기](#경고창에서-hello-world-찍기)
- [Sever & Client](#sever--client)
- [변수](#변수)
- [사칙연산 & 증감연산](#사칙연산--증감연산)
- [문자열 결합 연산](#문자열-결합-연산)
- [문자열 형변환](#문자열-형변환)
- [조건문](#조건문)
- [반복문](#반복문)
- [배열](#배열)
  - [배열 실습 - 오름차순 구현](#배열-실습---오름차순-구현)
- [함수](#함수)
- [Events](#events)

## JavaScript

자바스크립트는 모든 웹 개발자가 배워야 하는 3가지 언어 중 하나이다. 

1. 웹 페이지의 내용을 정의하는 HTML 
2. 웹 페이지 레이아웃을 지정하는 CSS 
3. 웹 페이지의 동작을 프로그래밍하는 JavaScript

> 자바스크립트를 설치하거나 다운받을 필요가 없다. 브라우저의 컴퓨터, 태블릿 및 스마트폰에서 JavaScript가 이미 실행 중이다.
> 

### JavaScript 기능



1. JavaScript는 HTML 콘텐츠를 변경할 수 있다. 
2. JavaScript는 HTML 특성 값을 변경할 수 있다.
3. JavaScript는 CSS(HTML 스타일)를 변경할 수 있다.

### 사용 방법

- HTML에서 <head> 부분, <script> 태그와 </script> 태그 사이에 자바스크립트 코드가 삽입된다.
- 주석은 자바와 동일 - `//` , `/* */`

### Console에 Hello World! 찍기

```jsx
<script type="text/javascript">
console.log("Hello World!");
</script>
```

- 브라우저의 개발자 도구, Console 창에서 확인할 수 있다. (F12)

### 경고창에서 Hello World! 찍기

```jsx
<script type="text/javascript">
alert("Hello World!");
</script>
```

- 브라우저의 경고창을 통해서 확인할 수 있다.

## Sever & Client

- Server에서 동작하는 프로그램 → Server side → java, jsp
- 브라우저(또는 사용자 PC에서 동작)에서 동작 → Client side → javascript, Active-X(사용자가 원하지 않아도 사용자 컴퓨터에서 정보를 긁어갈 수 있음. .exe 도 마찬가지로 안전하지 않다.)
- 보안 차원에서는 Server side가 안전하다.
- 하지만 서버에서 결과물을 가져와도 자바스크립트가 없으면 동작하지 않음.
- JavaScript의 버전은 ES6까지 올라왔다. 자바스크립트를 위해서 나왔다기보다는 확장기능을 위해서 나왔다. (제이쿼리 등을 위해서.)

## 변수

```jsx
var a = 1;
var b = 2.4;
var c = 'c';
var d = "Hi!";
var e = true;
console.log(a);
console.log(b);
console.log(c);
console.log(d);
console.log(e);
```

- 자바스크립트는 `var 하나로 모든 변수 선언 가능`. 자바랑 똑같은데 타입만 var로 바뀐 것.
- 자바스크립트가 자바보다 쉬운 이유
    - 자바에서는 기본 타입말고 객체들도 변수로 만들 수 있었고. 일반 자료형도 들어갔음.
    - 자바스크립트는 객체 변수도 var다.
- 주의할 점
    - 쓰는 건 되게 쉽지만, 그래서 짜다보면 뭐가 들어있는지 잘 모름.
    - 기억 잘하고, 주석을 잘 달자.

## 사칙연산 & 증감연산

```jsx
var a1 = 4;
var a2 = 2;
console.log(a1 + a2);
console.log(a1 - a2);
console.log(a1 * a2);
console.log(a1 / a2);
console.log(a1 % a2);

a1++;
a1 += 1;
console.log(a1);
```

## 문자열 결합 연산

```jsx
var a1 = 6;
var a2 = 2;
var d1 = "3";
console.log(a1 + a2 + d1); // 83
console.log(d1 + a1 + a2); // 자바랑 연산 순서는 같다. 362
```

- 자바에서는 큰따옴표랑 작은따옴표 차이가 있었다. 큰따옴표가 문자열, 작은따옴표는 문자 → 자바 스크립트는 홑따옴표나 쌍타옴표 **둘 다 문자열**로 쓰인다
- 자바랑 연산 순서 같다. 괄호로 우선연산 하는 것도 자바랑 똑같다.
- 변수 명 쓸 때 첫 문자로 숫자를 사용할 수 없다.

## 문자열 형변환

```jsx
var a1 = 6;
var a2 = 2;
var d1 = "3";
// 숫자 문자열을 숫자로 바꿀 때 * 1을 한다.
console.log((d1 * 1) + a1 + a2); // 콘솔에 11 출력
// 자바스크립트는 형변환이 원활하다.
var d2 = "abc";
console.log(d2 * 1); // 콘솔에 NaN 출력
// 문자열에 연산을 하면 NaN이 나옴. NaN은 에러는 아님. 
// NaN이라는 값이 존재함. 숫자가 아니다(Not a Number)라고 찍어주는 것임.
```

- 숫자가 아닌 문자열을 변환하면 NaN이 출력됨.

## 조건문

- `if` ,`if-else`, `if-else if-else` : 자바랑 동일하다.
- `break`, `default` : 자바와 동일

## 반복문

- `for`문도 자바와 동일. 타입만 var로 지정
- `while`도 자바와 동일
- 향상된 for문, 그리고 For in과 For of
    
    - Java에서는
        
        ```java
        for(변수타입 변수명 : 목록데이터) {
        }
        ```
        
    - JavaScript는 이것을 분리해놨다.
        
        ```jsx
        for(var key in 목록데이터) {
        	내용
        }
        for(var data of 목록데이터) {
        	내용
        }
        ```
        
    - `in` - 목록 데이터에서 순차적으로 key를 취득한다. 배열의 경우 index.
    - `of` - Java 향상된 for문과 동일.

- 터레이블

- 맵

## 배열

- 자바에서의 배열은?
    - 크기가 정해져 있고, 순차적으로 내용을 담는다.
    - 자바에서는 중괄호를 썼다.
- 자바스크립트에서의 배열은?
    - 크기를 유동적 조절 가능하다. 순차적으로 내용을 담는다.
    - 리스트랑 똑같다고 생각하면 됨. 리스트도 중간에 끼워넣기 가능한 것처럼.

```jsx
var arr = [1, 2, 3];

console.log(arr);

arr[0] = 7;

for(var ad of arr) {
	console.log(ad);
}
```

### 배열 실습 - 오름차순 구현

```jsx
var arr = [5, 7, 3];
var temp = 0;

for(var i = 0; i < arr.length - 1; i++) {
	for(var j = i + 1; j < arr.length; j++) {
		if(arr[i] > arr[j]) {
			temp = arr[i];
			arr[i] = arr[j];
			arr[j] = temp;
		}
	}
}

console.log(arr); // [3, 5, 7]
```

## 함수

- Function
    - 자바에서의 메소드랑 똑같은 역할인데, 메소드는 접근권한부터 시작함.
- function은 반환타입이 없고, 타입이 없다. - 타입이 var밖에 없기 때문에.
- 메소드를 축소해놓은 상태가 함수다.

```jsx
function test() {
	console.log("test!");
}

test(); // 함수 사용. 콘솔창에 test! 출력됨
```

## Events

- 버튼 만들기 - 클릭하면 경고창 출력.
    
    ```jsx
    <head>
    	<script>
    		function test() {
    		alert("test!")
    		}
    	</script>
    </head>
    <body>
    <input type="button" value="버튼" onclick="test();">
    ```
    
    - onclick - 사용자가 HTML 요소를 클릭하면.

- 버튼 만들기2 - 클릭하면 텍스트 박스에 자동 입력
    
    ```jsx
    <script type="text/javascript">
    function test() {
    	var txt = document.getElementById("txt"); // id="txt"인 요소의 값을 txt에 저장
    	txt.value += 1; // txt 요소의 value에 1을 누적
    }
    </script>
    </head>
    <body>
    <input type="text" id="txt">
    <input type="button" value="버튼" onclick="test();">
    </body>
    ```
    
    - HTML은 문서라서 `document.`
    - `document.getElementById()` : id값을 활용해서 <body>에 있는 요소(element)를 가져올 수 있다.

- `this` : 이 객체. 화면에 붙어있는 것은 객체다.
    
    ```jsx
    <script type="text/javascript">
    function test(obj) {
    	console.log(obj); 
    	var txt = document.getElementById("txt");
    	console.log(txt);
    	txt.value += 1;
      console.log(txt.value);
    	console.log(txt.id);
    }
    
    //test();
    
    </script>
    </head>
    <body>
    <input type="text" id="txt">
    <input type="button" value="버튼" onclick="test(this);">
    </body>
    ```
    
    - 버튼을 클릭하면
        
        ![스크린샷.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/98757dbe-8d99-48d3-ba20-abaab3f82aad/스크린샷_2022-01-16_오후_11.10.13.png)