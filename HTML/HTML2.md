### `<a>` 태그
- `a(anchor)` : 이동용 태그. Entity에 있는 것을 클릭시 목적지로 이동.
- `href` : 목적지
- `target` : 어디에 띄울지 나타내는 것. _self가 기본값. _blank는 새탭에서 띄운다.

```html
<a href="https://www.google.com" target="_blank">구글</a>
```

### 경로
- 절대 경로 : 절대적 위치
    - ex) `C:₩ ~ ₩ ~ ₩a.png`
    - ex) `http://~/a.png`
- 상대 경로 : 현재 위치를 기준으로 경로 제공
    - `./` 또는 아무것도 없을때 : 현재 위치
    - `../` : 상위폴더
- 예시
    
    ```html
    - a.html - 여기서 a.png를 찾아갈 때 : ./img/a.png
    - img
        - a.png
    - test
        - b.html - 여기서 a.png를 찾아갈 때 : ../img/a.png
    ```
    
- 웹서비스를 만들 때에는 상대 경로를 더 많이 씀

### `<img>` 태그
- `img` : 이미지 출력
- `alt` : 설명 - 정확한 용도는 TTS 출력용.
- `src` : 위치 정보
- 이미지에 a태그를 달 수도 있음

```html
<a href="https://apple.com">
<img alt="감사합니다" src="thanks.jpeg">
</a>
```

### `<map>` 태그
- `map` : 클릭 가능한 영역을 만듬
- `usemap` : 해시 태그로 시작. 이미지와 맵의 관계 생성
- `area` : 영역 지정
- `shape` : 형태
- `coords` : 좌표

```html
<img alt="감사합니다" src="thanks.jpeg" usemap="#thanks">
<map name = "thanks">
	<area alt="사각" shape="rect" coords="390, 62, 489, 306" href="http://www.naver.com" />
	<area alt="원" shape="circle" coords="194, 218, 22" href="http://www.daum.net" />
	<area alt="다각형" shape="poly" coords="200, 59, 243, 41, 242, 80" href="http://www.amazon.com" />
</map>
```

### `<ul>` 태그, `<ol>` 태그, `<li>` 태그

- `ul` : 순서 없는 목록을 지정하겠다.
- `ol` : 순서가 있는 목록을 지정하겠다.
- `li` : 목록 요소

```html
<!-- 
	ul : 순서 없는 목록을 지정하겠다.
	li : 목록 요소
-->
<ul>
	<li>가나다</li>
	<li>라마바</li>
</ul>
<!-- ol : 순서가 있는 목록을 지정하겠다. -->
<ol>
	<li>가나다</li>
	<li>라마바</li>
</ol>
```

### `<dl>` 태그, `<dt>` 태그, `<dd>` 태그

- `dl` : 목록 지정
- `dt` : 제목
- `dd` : 부제 또는 설명

```html
<dl>
	<dt>ABC1234567890</dt>
	<dd>def1234567890</dd>
</dl>
```

### `<table>` 태그 : 표

- `<thead></thead>` : 헤더
- `<tbody></tbody>` : 내용. 만약 thead, tfoot이 없는 경우 생략 가능
- `<tfoot></tfoot>` : 끝 영역
- `<tr></tr>` : row 영역(행) - 여러 row중 td가 가장 많은 개수에 맞추어 크기 지정
- `<td></td>` : column 영역
- `<th></th>` : column 헤더 영역. td와 동일하나 굵기와 가운데 정렬 차이.
- `border` : 테두리 두께
- `cellspacing` : 셀 간격
- `<colgroup></colgroup>` : td나 th의 간격을 통째로 지정. width만 가능. tr은 불가능.
- `rowspan` : 해당 내용이 지정된 개수만큼 세로로 영역을 확보
- `colspan` : 해당 내용이 지정된 개수만큼 가로로 영역을 확보

```html
<table border="1" cellspacing="0">
	<colgroup><!-- column group - td나 th의 간격을 통째로 지정. width만 가능. tr은 불가능.-->
		<col width="100" />
		<col width="200" />
		<col width="50" />
	</colgroup>
	<thead>
		<tr>
			<th>1번 칸</th>
			<th>2번 칸</th>
			<th>3번 칸</th>
		</tr>
	</thead>
	<tbody>
	<!-- rowspan : 해당 내용이 지정된 개수만큼 세로로 영역을 확보 -->
	<!-- colspan : 해당 내용이 지정된 개수만큼 가로로 영역을 확보 -->
		<tr height="50"> <!-- row를 나타내기에 높이 지정 -->
			<td rowspan="2">1-1</td> <!-- column을 나타내기에 너비 지정 width를 활용해 가능. -->
			<td colspan="2">1-2</td>
		</tr>
		<tr>
			<td>2-2</td><!-- 같은 위치의 td들 중 width가 가장 큰 것의 크기에 맞춘다. -->
			<td>2-3</td>
		</tr>
	</tbody>
	<tfoot>
		<tr>
			<td>안녕하세요.</td>
		</tr>
	</tfoot>
</table>
```