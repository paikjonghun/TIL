1. HTML `<select>` tag
2. HTML `<textarea>` tag
3. HTML Block & Inline
4. CSS
   1. Selector
   2. HTML 공통 속성
   3. 색상
   4. border

---

### 1. HTML `<select>` tag

- `select` : 단일 선택용. 셀렉트 박스라고 함.
- 잘 쓸일은 없긴 한데, `<option group>` 이라는 게 있음. 단일 선택 항목들에 카테고리를 붙일 수 있음.

```html
<select>
	<optgroup label="채소">
		<option>고구마</option>
		<option>감자</option>
	</optgroup>
	<optgroup label="과일">
		<option>사과</option>
		<option>오렌지</option>
	</optgroup>
</select>
```

<br/>

### 2. HTML `<textarea>` tag

- `textarea` : 여러 줄 텍스트 입력. 값을 할당할 때 엔티티에 추가한다.
- `rows` : 몇 줄까지 한번에 보일지.
- `cols` : 대문자 기준 몇 글자까지 한 줄에 보일지.
- textarea 고정은 css에서만 가능.

```html
<textarea rows="5" cols="20">가나다
라마바
사아자</textarea>
```

<br/>

### 3. HTML Block & Inline

> 모든 HTML 요소에는 요소의 유형에 따라 기본 표시 값이 있습니다. 블록과 인라인의 두 가지 표시 값이 있습니다.
> 
- `div` : 영역 지정 태그. display 속성이 block.(block - 가로 100%를 영역으로 지정)
- `span` : 영역 지정 태그. display 속성이 inline.(inline - 내용만큼만 영역으로 지정)

```html
<div class="a">1</div>
<div id="b1">2
	<div class="b1_1">2-1
		<div>2-1-1</div>
	</div>
	<div class="b1_2">2-2
		<div>2-2-1</div>
	</div>
</div>
<div name="c">3</div>
<span class="a">1</span>
<span id="b2">2</span>
<span name="c">3</span>
```

<br/>

### 4. CSS
> CSS는 웹 페이지의 스타일을 지정하는 데 사용하는 언어입니다. CSS 규칙은 선택기와 선언 블록으로 구성됩니다.
- HTML → 보여준다.
- CSS → 꾸며준다.
- Javascript → 움직인다.

<br/>

#### 4. 1. Selector

- Selector - css의 절반 이상.
    - 내가 원하는 요소를 찾기 위해서 사용하는 것 - 스타일을 지정할 HTML 요소를 가리킨다.
        - Selector 종류가 많다.

<br/>

- 셀렉터(기본)
    - `태그명` - 해당 태그들에 적용
        
        ```html
        div {
        	color: #FFFFFF; /* color : 글자 색상을 나타냄 */
        }
        ```
        
    - `.클래스명` - 속성 중 class가 작성된 클래스명과 동일한 경우 적용
        
        ```html
        .a {
        	background-color: red;
        }
        ```
        
    - `#아이디명` - 속성 중 id가 작성된 아이디명과 동일한 경우 적용
        
        ```html
        #b1, #b2 {
        	background-color: green;
        }
        ```
        
    - `[속성 = 값]` - 해당 속성이 해당 값인 경우 적용
        
        ```html
        [name="c"] {
        	background-color: blue;
        }
        ```
        
    - `*` - 모든 것에 적용

<br/>

#### 4. 2. HTML 공통 속성
- class - 중복 허용. 복수지정 가능. 디자인용으로 주로 사용
- id - 고유사용권장. 개발용도로 주로 사용
- name - 중복 가능. 값 전송(다른 페이지) 용도. 개발용도로 주로 사용

디자인을 분리한다는 것이 이런 것. 일괄 적용, 수정이 가능하다.

<br/>

#### 4. 3. 색상

- 색상 - 3원색(red, green, blue)
    - red - 0~255
    - grenn - 0~255
    - blue - 0~255
    
    > 밝기 0 ~ 255 : 어둡다 → 밝다
    > 
- 10진수를 16진수(HEX)로 변환
    - 0~255 / 0~255 / 0~255 → 00~FF / 00~FF / 00~FF

CSS에서 색상 지정

1. 지정 명칭 - red, green, blue
2. 16진수 - #FFFFFF
3. RGB - RGB(255, 255, 255)

<br/>

#### 4. 4. border

기본적으로 보더 옵션은

border-width: *1px*;

border-style: *solid*; /* solid: 실선 */

border-color: *#000*;