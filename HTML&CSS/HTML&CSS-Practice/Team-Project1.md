```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>카카오뱅크 ERP - Sample</title>
<link rel="icon" href="images/favicon/favi.gif" />
<link rel="stylesheet" type="text/css" href="css/cmn.css" />
<style type="text/css">
/* 가로 사이즈 조정용 */
.cont_wrap {
	width: 900px;
}

/* 개인 작업 영역 */
.expns_rsltn_dtl_view {
	border-collapse: collapse;
	font-size: 13px;
	margin-bottom: 20px;
}

.expns_rsltn_dtl_view tbody td {
	border: 1px solid #DDDDDD;
	height: 40px;
}

.expns_rsltn_dtl_view td:nth-child(1) {
	text-align: center;
	font-weight: bold;
	background-color: #F2F2F2;
	font-weight: bold;
}

.expns_rsltn_dtl_view td:nth-child(2) {
	padding-left: 20px;
	display: table-cell;
	vertical-align: middle;
}

.btn_wrap {
	display: inline-block;
	vertical-align: top;
	text-align: right;
	width: 850px;
}

.atchd_file {
	display: inline-block;
	vertical-align: middle;
	width: 30px;
	height: 40px;
	background-image: url('./images/cmn/dwnld_icon.png');
	background-position: center;
	background-repeat: no-repeat;
	background-size: 20px;
	cursor: pointer;
	position: relative;
}

.file_name {
	display: inline-block;
	vertical-align: middle;
}

.file_name:hover {
	text-decoration: underline;
	cursor: pointer;
	font-weight: bold;
}

</style>
</head>
<body>
	<div class="top_area">
		<div class="logo">
			<div class="logo_erp_text">ERP</div>
		</div>
		<div class="menu_depth1_wrap">
			<!-- _on 달린 것이 활성화 상태 -->
			<div class="top_menu">
				<div class="top_menu_text">인사</div>
				<div class="top_menu_bar"></div>
			</div>
			<div class="top_menu">
				<div class="top_menu_text">그룹웨어</div>
				<div class="top_menu_bar"></div>
			</div>
			<div class="top_menu_on">
				<div class="top_menu_text">경영관리</div>
				<div class="top_menu_bar"></div>
			</div>
			<div class="top_menu">
				<div class="top_menu_text">영업관리</div>
				<div class="top_menu_bar"></div>
			</div>
			<div class="top_menu">
				<div class="top_menu_text">고객지원</div>
				<div class="top_menu_bar"></div>
			</div>
			<div class="top_menu">
				<div class="top_menu_text">경영정보</div>
				<div class="top_menu_bar"></div>
			</div>
		</div>
	</div>
	<div class="left_area">
		<div class="emp_info_area">
			<div class="emp_info">
				<div class="emp_image_area">
					<div class="emp_image_wrap">
						<img alt="사원이미지" src="images/cmn/emp_image.png" />
					</div>
				</div>
				<div class="emp_info_wrap">
					<div class="emp_name_txt">
						백종훈
						<div class="emp_opt_btn"></div>
					</div>
					<div class="emp_dept_txt">경영관리팀 대리</div>
				</div>
			</div>
			<div class="emp_menu_area">
				<div class="emp_menu">메신저</div>
				<div class="emp_menu">내정보</div>
				<div class="emp_menu_last">로그아웃</div>
			</div>
		</div>
		<div class="left_menu_dpeth1_txt">경영관리</div>
		<div class="left_menu_wrap">
			<!-- 2depth 메뉴 -->
			<!-- 현재 선택중인 메뉴는 on -->
			<div class="menu_depth2_on">
				<div class="menu_depth2_area">
					<div class="menu_depth2_text">회계관리</div>
					<div class="left_menu_bar"></div>
				</div>
				<!-- 2depth 메뉴 -->
				<!-- 현재 선택중인 메뉴는 on -->
				<div class="menu_depth3_wrap">
					<div class="menu_depth3">기초등록</div>
					<div class="menu_depth3_on">지출결의서</div>
					<div class="menu_depth3">지출결의서관리</div>
					<div class="menu_depth3">내부비용관리</div>
					<div class="menu_depth3">전표관리</div>
					<div class="menu_depth3">급여관리</div>
					<div class="menu_depth3">카드관리</div>
				</div>
			</div>
			<div class="menu_depth2">
				<div class="menu_depth2_area">
					<div class="menu_depth2_text">프로젝트 투입관리</div>
					<div class="left_menu_bar"></div>
				</div>
				<div class="menu_depth3_wrap">
					<!-- 현재 선택중인 메뉴는 on -->
					<div class="menu_depth3_on">프로젝트 관리</div>
					<div class="menu_depth3">가용 인력 현황</div>
				</div>
			</div>
			<div class="menu_depth2">
				<div class="menu_depth2_area">
					<div class="menu_depth2_text">시설물 관리</div>
					<div class="left_menu_bar"></div>
				</div>
				<div class="menu_depth3_wrap">
					<div class="menu_depth3">시설물 사용 신청</div>
					<div class="menu_depth3">시설물 목록</div>
					<div class="menu_depth3">사용신청 승인관리</div>
				</div>
			</div>
			<div class="menu_depth2">
				<div class="menu_depth2_area">
					<div class="menu_depth2_text">자산 관리</div>
					<div class="left_menu_bar"></div>
				</div>
				<div class="menu_depth3_wrap">
					<div class="menu_depth3">자산목록</div>
					<div class="menu_depth3">소모성비품 입출기록</div>
				</div>
			</div>
		</div>
	</div>
	<div class="right_area">
		<div class="navi_bar">
			Home
			<div class="navi_next"></div>
			경영관리
			<div class="navi_next"></div>
			회계관리
			<div class="navi_next"></div>
			지출결의서
		</div>
		<!-- 내용영역 -->
		<div class="cont_wrap">
			<div class="page_title_bar">
				<div class="page_title_text">지출결의서 일자별 상세보기</div>
				<!-- 검색영역 선택적 사항 -->
			</div>
			<!-- 해당 내용에 작업을 진행하시오. -->
			<div class="cont_area">
				<!-- 여기부터 쓰면 됨 -->
				<table class="expns_rsltn_dtl_view">
					<colgroup>
						<col width="150" />
						<col width="700" />
					</colgroup>
					<tbody>
						<tr>
							<td>작성일자</td>
							<td>2022년 1월 27일 목요일</td>
						</tr>
						<tr>
							<td>사원코드</td>
							<td>234</td>
						</tr>
						<tr>
							<td>사원명</td>
							<td>홍길동</td>
						</tr>
						<tr>
							<td>계정코드</td>
							<td>55</td>
						</tr>
						<tr>
							<td>계정명</td>
							<td>출장비</td>
						</tr>
						<tr>
							<td>지출처</td>
							<td>코레일</td>
						</tr>
						<tr>
							<td>지출금액</td>
							<td>100,000원</td>
						</tr>
						<tr>
							<td>지출일시</td>
							<td>2022년 1월 27일 목요일</td>
						</tr>
						<tr>
							<td>지출유형</td>
							<td>뭘까요?</td>
						</tr>
						<tr>
							<td>적요</td>
							<td></td>
						</tr>
						<tr>
							<td>첨부파일</td>
							<td>
								<div class="atchd_file"></div>
								<div class="file_name">영수증.jpg</div>
							</td>
						</tr>
					</tbody>
				</table>

				<div class="btn_wrap">
					<div class="cmn_btn">취소</div>
					<div class="cmn_btn">수정</div>
					<div class="cmn_btn">삭제</div>
				</div>
			</div>
		</div>
	</div>
</body>
</html>
```