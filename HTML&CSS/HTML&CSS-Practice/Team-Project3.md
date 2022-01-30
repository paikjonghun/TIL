```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>카카오뱅크 ERP - 내부비용관리 - 월별 목록</title>
<link rel="icon" href="images/favicon/favi.gif" />
<link rel="stylesheet" type="text/css" href="css/cmn.css" />
<style type="text/css">
/* 가로 사이즈 조정용 */
.cont_wrap {
	width: 900px;
}
/* 개인 작업 영역 */
.mnthly_slct_wrap {
	width: calc(100% - 400px);
	display: inline-block;
	vertical-align: top;
	text-align: right;
}

.mnthly_slct {
	display: inline-block;
	vertical-align: top;
}

.intrnl_cost_admnstrtn_list {
	border-collapse: collapse;
	font-size: 13px;
	margin-bottom: 20px;
}

.intrnl_cost_admnstrtn_list thead th {
	width: 225px;
	height: 30px;
	border: 1px solid #DDDDDD;
	background-color: #F2F2F2;
}

.intrnl_cost_admnstrtn_list tbody td {
	height: 40px;
	border: 1px solid #dddddd;
	padding-left: 10px;
}

.intrnl_cost_admnstrtn_list tbody td:nth-child(4) {
	font-weight: bold;
}

.intrnl_cost_admnstrtn_list tbody td:nth-child(1):hover {
	cursor: pointer;
	text-decoration: underline;
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
					<div class="menu_depth3">지출결의서</div>
					<div class="menu_depth3">지출결의서관리</div>
					<div class="menu_depth3_on">내부비용관리</div>
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
			내부비용관리
		</div>
		<!-- 내용영역 -->
		<div class="cont_wrap">
			<div class="page_title_bar">
				<div class="page_title_text">내부비용관리 월별 목록</div>
				<div class="mnthly_slct_wrap">
					<input type="month" class="mnthly_slct" />
				</div>
			</div>
			<!-- 해당 내용에 작업을 진행하시오. -->
			<div class="cont_area">
				<!-- 여기부터 쓰면 됨 -->
				<table class="intrnl_cost_admnstrtn_list">
					<thead>
						<tr>
							<th>전표귀속연월</th>
							<th>현금 비용 합계</th>
							<th>카드 비용 합계</th>
							<th>총 합계</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<td>2022. 01</td>
							<td>200,000원</td>
							<td>300,000원</td>
							<td>500,000원</td>
						</tr>
						<tr>
							<td>2022. 02</td>
							<td>100,000원</td>
							<td>100,000원</td>
							<td>200,000원</td>
						</tr>
						<tr>
							<td></td>
							<td></td>
							<td></td>
							<td></td>
						</tr>
						<tr>
							<td></td>
							<td></td>
							<td></td>
							<td></td>
						</tr>
						<tr>
							<td></td>
							<td></td>
							<td></td>
							<td></td>
						</tr>
						<tr>
							<td></td>
							<td></td>
							<td></td>
							<td></td>
						</tr>
						<tr>
							<td></td>
							<td></td>
							<td></td>
							<td></td>
						</tr>
						<tr>
							<td></td>
							<td></td>
							<td></td>
							<td></td>
						</tr>
					</tbody>
				</table>
				<div class="board_bottom">
					<div class="pgn_area">
						<div class="page_btn page_first">first</div>
						<div class="page_btn page_prev">prev</div>
						<div class="page_btn_on">1</div>
						<div class="page_btn">2</div>
						<div class="page_btn">3</div>
						<div class="page_btn">4</div>
						<div class="page_btn">5</div>
						<div class="page_btn page_next">next</div>
						<div class="page_btn page_last">last</div>
					</div>
					<div class="cmn_btn">신규</div>
				</div>
			</div>
		</div>
	</div>
</body>
</html>
```