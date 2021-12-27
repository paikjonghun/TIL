# Oracle Database에 대하여
<br />
<br />
<br />
자바할 때는 이클립스를 썼듯이, 데이터베이스를 위한 오라클 개발 도구를 설치할 것임. 

오라클 개발 도구 : Oracle SQL Developer / Mysql 등

### 윈도우에서 SQL Developer + Oracle DB 설치

[oracle.com](http://oracle.com) 으로 이동 → Oracle 회원가입 → Oracle SQL Developer 설치 →  Oracle Database Express Edition11g 설치([https://oracle.com/database/technologies/xe-prior-release-downloads.html](https://oracle.com/database/technologies/xe-prior-release-downloads.html))

### 맥에서 SQL Developer + Oracle Cloud DB 활용

맥에서의 SQL Developer 설치(설치 후에 jdk 경로 지정, bash 권한 부여) + Oracle Cloud Free Tier 연결

### **Local_SYS 생성**

SQL Developer 실행 → 앞으로 시작화면 보이지 않게 체크 → 보고서 닫기 → 녹색 십자 → 새로 만들기/데이터베이스 접속 선택

위치 + 프로젝트 + 계정명 - Local_SYS

### **'다른 사용자'에 SCOTT 만들고 인코딩 변경하고 scott.sql 다운로드 후 열기**

초록색 +버튼 눌러서 새로 만들기

Name : Local_SCOTT

사용자 이름 : SCOTT

비밀번호 저장 : tiger

→ 테스트하고 상태 : 성공 뜨면 접속

→ Local_SYS 접속 해제

→ 환경설정에서 인코딩을 UTF-8로 변경

→ scott.sql 열기

### Data

![스크린샷 2021-12-13 오전 10.06.32.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b97ba40e-9f31-44a9-9abb-f970b9bbf2b8/스크린샷_2021-12-13_오전_10.06.32.png)

**Data : 수치화 가능한 기록된 내용**

Data를 용도에 맞게 가공한 것이 정보. 날숨의 이산화탄소 수치, 물 사용량, 교통카드 기록. 일상생활에서 모든 것들이 데이터화 되어있고, 개인 신상도 데이터화 되어있다. 

데이터가 가공되지 않았을 때에는 가치가 적다. 데이터 가공을 어떻게 할 것인지가 중요하다.

데이터에 대한 기준을 어떻게 잡고 분류할 것인지 정해야 한다.

Data를 기준에 따라 용도별로 보관하고, 가공하면 정보가 되는 것.

가장 효율적으로 데이터를 보관하는 방법 → 표에 정리하기!

### Table

![스크린샷 2021-12-13 오전 10.11.45.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e8a3a8e2-ae23-4997-8470-27360d11fecf/스크린샷_2021-12-13_오전_10.11.45.png)

데이터베이스에서는 표를 **테이블(table)**이라고 말함.

표에서 가로 한 줄을 **row**라고 말함.

표에서 세로 한 줄을 **column**이라고 말함.

**row와 column으로 이뤄진 데이터는 결국 테이블!**

**테이블**은 데이터의 집합. 

### **Schema**

**스키마**는 테이블들이 모여있음.(그 외의 것들도 포함되어 있음.)

스키마는 **개념**이라는 뜻을 가지고 있음.

### Database

스키마가 모여서 → **데이터베이스**를 이루게 됨

데이터베이스를 관리하기 위해서 DBMS(Data Base Management System - 데이터베이스 관리 시스템)를 사용.

### **DBMS의 종류**

![스크린샷 2021-12-13 오전 10.38.37.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9a4ae6c-c748-4e07-ba2d-fa6e1d9f3b85/스크린샷_2021-12-13_오전_10.38.37.png)

**Ansi-DB** - 국제표준협회에 등록되어있는 데이터베이스. 가장 근간이 되는 데이터베이스 형태. 조상격. 웬만한 데이터 기법들이 여기서 나옴.

→ **Oracle** / **MS-SQL** / **MySQL**(무료였는데, 오라클이 MySQL을 샀음. 유료화 정책 시작) / **SQLite3**(경량화 기반의 데이터 베이스) / **DB2**(이제는 거의 없어짐)

- **Oracle**

오라클은 타겟이 크고 많은 데이터베이스를 관리하는 것

구글 페북 MySQL썼는데, 오라클이 MySQL(썬 마이크로시스템즈) 인수

- **MariaDB**

MySQL 5.5를 기반으로 한 MariaDB가 나오게 됨. 서비스명은 못바꿔서 설치해도 MySQL이라고 떠있음.

구글이 MariaDB로 이관하려 했고, 버튼 하나로 이관 성공. 구글이 MariaDB지원 시작.

MariaDB는 다양한 시도를 했음. 호환성 높이기 위함. 오라클에서 쓰는 모든 문법을 제공하겠다. 오라클 모드.

오라클을 배우더라도 MariaDB를 사용하는데 크게 상관은 없다.

- **SQLite3**

경량화 기반의 데이터 베이스. 데이터베이스 자체가 23MB. 최소 24MB만 있으면 돌아감. 스마트폰에 들어가는 데이터베이스가 대부분 이것. 

- **MS-SQL**

범용성이 떨어짐. 자체가 비싸고, 윈도우에서 밖에 작동 안함. 자주 쓸 일은 없다.

MS-SQL의 낮은 버전 라이센스를 한국 모 회사에서 샀다. 

Sybase라는 데이터베이스가 있다. 서비스 기술 국산화 사업을 진행하면서 만듦.

버전마다 기능이 다르다.

Sybase 라이센스를 해외에 팔았음. 따라서 국내 Sybase 지원이 없다. 버전업 안되고, 지원보상도 없음. 요즘 대부분 걷어내긴 했는데 가끔 가다 볼 수 있다

- **티베로**

티맥스회사에서 나온 것이고, 한국산 데이터베이스 관리 시스템.

- **No-SQL**
    - MongoDB
    - Oracle noSQL

NoSQL기반 기술들도 활발하게 개발되고 있는데, 원활하게 모든 데이터를 관리할 수 있으면 좋겠는데, 통계 같은 부분에서는 Ansi-DB기반 기술들에서만 해결 가능.

- **FireBase**

선을 걸치고 있음.

- **PL/SQL** - SQL이 선행되어야 함.

### **Oracle DataBase Full Version과 Express Edition의 차이**

익스프레스 에디션은 교육용, 경량화, 설치가 간편함. 

전문업체가면 풀버전을 설치해야할 수도 있음.

데이터베이스는 데이터를 보관할 공간이 필요함. 공간 확보를 하고, 보관될 수 있는 구조를 만들어야 함.

익스프레스 에디션은 보통 기본 데이터 크기가 1GB, 풀버전은 4GB를 잡음. 

숫자1개 = 1Byte

1KB = 1024Byte

1MB = 1024KB

1GB = 1024MB

1TB = 1024GB

자바할 때는 자바 자체만 생각했는데, 데이터베이스라는 것은 요청을 해서 소통을 해야함.

### **호스트 이름 - 도메인 or ip**

![스크린샷 2021-12-13 오전 11.19.28.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ea7d335e-684f-4fe8-b07f-c2947ea284d6/스크린샷_2021-12-13_오전_11.19.28.png)

컴퓨터에서 네이버를 여는 것. 컴퓨터가 [www.naver.com](http://www.naver.com)를 찾아가는 것.

컴퓨터는 공유기에 연결이 되어있다. 허브, 허브 스위치 등. 공유기랑 소통하는 것은 라우터. 

라우터는 여러개의 망을 연결. 다수의 공유기들과 라우터가 연결되어있음. 

그렇게 연결된 공유기들에는 또 다를 컴퓨터가 연결되어 있음. 라우터 하나를 통해서 망으로 연결되어 있고, 라우터들끼리도 망을 연결하고 있음. 라우터는 서버와 연결이 되어있음.

인터넷망이 거미줄처럼 연결되어있음. 전세계적으로. 

도메인은 DNS라는 것을 거치게 됨.(Domain Name Server) 그러면 ip정보가 돌아옴. ip는 숫자로 이뤄져있음. ip라는 것은 ipv4가 있고, ipv6가 있음. 

- ipv4
    - 0~255.0~255.0~255.0~255 → A,B,C,D 클래스로 이뤄져있는 ip체계
- ipv6

ip는 각 컴퓨터도 가지고 있고, 각 서버도 가지고 있다.

브로드 캐스팅을 뿌리고 리턴값이 돌아오면 가는 경로를 기억해서 그 주소로 감.

- **Local Host**

Local Host 는 내 컴퓨터. 내 컴퓨터는 [localhost](http://localhost) 또는 127.0.0.1 이라고 표현함

사설 인터넷 망 : 192.168.x.x → 나는 사설망에 있구나. 공적인 망이 아니구나. 사설망은 사설망끼리만 소통 가능. 외부에서 내 것을 찾아올 수 없음. 

- **포트(port)**

우리는 컴퓨터를 사용할 때 동시에 프로그램을 여러개 켜기도 한다. 하나의 프로그램만 정확하게 찾아 가고 싶어서 포트라는 것을 쓰게됨.

지정된 위치(port-항구)에 가서 배를 주차하게끔 되어있음.

네가 서비스를 받고 싶으면 해당하는 포트에 들어와야지만 이용할 수 있어 → 서비스 접근 허용 경로

방화벽이 열려있어야 함.

포트는 65000번 아래로만 설정가능. 

프로그램마다 고유의 포트번호가 있음. 운영할 때는 포트 번호를 바꾸기는 함. 

- **SID(Service ID)**

구동중인 서비스에 대해서 한번 더 체크하는 것. 데이터베이스에서 SID를 중요하다고 얘기하는 이유는 포트를 쓰는데 이 포트를 쓰는 것이 오라클이 아닐 수도 있음. 확인하는 용도로 사용하는 것이 SID. 포트와 SID가 일치해야 접속 가능. 경로가 허용되어있어야 하고, 이름이 같아야 함.

SQL에서 명령은 구문이라고 말함.

### **데이터베이스에도 언어가 있다.**

![스크린샷 2021-12-13 오후 1.39.50.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/603db8c0-bffc-47db-a074-9521c4f5614b/스크린샷_2021-12-13_오후_1.39.50.png)

- **SQL - 질의문** → 데이터 베이스의 데이터들을 관리하기 위한!

→ DML(Data Manipulation Language) : 조작어

→ DDL(Data Definition Language) : 정의어

→ DCL(Data Control Language) : 제어어

DML에서 조회를 담당하고 있는 것은 Select문

- **Select문 → 데이터 조회, 결과는 view**

![스크린샷 2021-12-13 오후 1.52.28.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f28055a7-598e-4059-bbe4-9a6a7a66984b/스크린샷_2021-12-13_오후_1.52.28.png)

1. **Select : 컬럼or값, ... → 보여줄 내용을 지정.**
2. **from : 데이터(테이블 or view) → 데이터를 가져온다.**
3. **where : 조건 (선택사항) → 데이터를 걸러낸다.**
4. **order by : 정렬조건 (선택사항) → 데이터를 정렬한다.**

일반적 : 2 - 3 - 1 - 4

순차함수 : 2 - 3 - 4 - 1

무조건 데이터를 가져오고 거른다. 그래서 2, 3은 고정. 보여줄 걸 먼저 정한 뒤에 순차적으로 정렬함.

데이터는 테이블이나 뷰를 말함. 데이터를 뷰라고 함.

데이터 베이스는 데이터를 다루는 것. 

데이터의 원본이 바뀌면 안되기 때문에, 일반적으로 복사해서 쓰게 됨.

테이블도 뷰가 됨. 원본이 바뀌는 순간 무결성이 무너진다. 데이터는 완벽함을 유지해야하기 때문에 값이 바뀌거나 순서가 바뀌면 안됨. 원천 데이터가 바뀌는 것에 민감하게 받아들여야 함. 

- **SELECT문이 가장 중요하다.**

웹사이트를 사용하는 가장 근본적인 이유는 **효과적인 정보전달**.

정보전달한 것을 봐야함. **무엇을 볼 것인지가 중요.**

정확하게 내가 원하는 데이터를 원하는 형태로 보기 위해서 셀렉트문을 써야함.

```sql
SELECT EMPNO, ENAME, SAL
FROM EMP
;
```

**where절** → 조건을 처리함. 결과는 boolean형태가 되어야 함.

![스크린샷 2021-12-13 오후 2.33.01.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ce7ea671-bfc9-40e2-81f0-74061190c6d3/스크린샷_2021-12-13_오후_2.33.01.png)

```sql
A > B
A < B
A >= B
A <= B
A = B -- 같다
A != B -- 다르다
```

데이터베이스에서는 **문자열 쓸 때 홑따옴표 씀. 문자열과 문자를 구분하진 않음.**

데이터베이스는 문법이라 말 하듯이 쓰면 그대로 나옴. 복잡하게 생각하는 경우가 많다.

- **값 AND 값 - 자바에서의 &&와 같다. 이상, 이하 표현 가능.**
    
    ```sql
    SAL >= 800 AND SAL <= 1300
    ```
    

- **값 OR 값 - 자바에서의 ||와 같다. '또는' 표현.**
    
    ```sql
    SAL = 800 OR SAL = 1100 OR SAL = 1300
    ```
    

- **값 BETWEEN A AND B : 값에서 A 이상 B 이하인 것**
    
    ```sql
    SAL BETWEEN 800 AND 1300
    ```
    

OR랑 AND랑 섞어 쓰면 문제 발생하기 쉬움.

- **값 IN (값1, 값2, ...) : 값이 값1 이거나 값2 이거나...**
    
    ```sql
    SAL IN (800, 1100, 1300)
    ```
    

- **문자열 비교**
    
    ```sql
    A = '~' -- A안에 ~와 똑같은 값이 있는지 비교를 한다. 
    
    A LIKE '형태' -- LIKE는 닮았다는 뜻. 형태를 지칭할 때 % => 값이 있을 수도 있고 없을 수도 있다.
    
    'A%' -- A로 시작하고 이후에 값이 있을 수도 있고, 없을 수도 있다.
    
    '%A' -- A로 끝나고 앞에 값이 있을 수도 있고, 없을 수도 있다.
    
    '%A%' -- A가 존재하고 앞뒤에 값이 있을 수도 있고, 없을 수도 있다.
    
    ```
    

- **Order by : 정렬**

![스크린샷 2021-12-13 오후 3.36.28.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a10ff4a3-a2bd-4d2e-9a2b-bbb18b081f46/스크린샷_2021-12-13_오후_3.36.28.png)

→ **ASC** : 오름차순

→ **DESC** : 내림차순

Order by 값 or 컬럼 ASC, ..., ...

Order by 값 or 컬럼 DESC, ..., ...

1번 기준 정렬. 만약 동일한 값이 있으면 2번 기준 추가정렬.

```sql
SELECT ENAME, JOB, SAL 
FROM EMP
ORDER BY JOB ASC, SAL DESC, ENAME ASC -- JOB 기준으로 오름차순 정렬, 
;																			-- 동일한 값 있으면 SAL 기준으로 내림차순 정렬,
																			-- 동일한 값 있으면 ENAME 기준으로 오름차순 정렬
```

- **AS(ALIAS) : 별칭**

```sql
-- AS(ALIAS) : 별칭
SELECT ENAME AS 이름, JOB AS 업무, SAL AS 급여 -- SELECT가 먼저 실행이 된다. 
-- 그래서 별칭을 ORDER에서 가져다가 쓸 수 있다.
FROM EMP
ORDER BY 업무 ASC, SAL DESC, ENAME ASC
;
```

- **DUAL : 아무것도 들어있지 않은 VIEW**

```sql
SELECT 2 + 2 * 2
FROM DUAL
;
```

- **해당 컬럼의 모든 로우에 곱하기(*) 가능, 동시에 ALIAS를 활용해 별칭 붙이기 가능.**

```sql
SELECT ENAME, SAL * 1.2 AS RSAL
FROM EMP
;
```

---
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />

- **ROUND / FLOOR / TRUNC / CEIL / MOD / ABS**

```sql
-- ROUND(값, 자리수) : 값을 소수점 해당 자리수까지 반올림
-- FLOOR(값) : 값의 소수점을 버림처리
-- TRUNC(값, 자리수) : 값을 소수점 해당 자리수까지 버림(올림은 없음)
-- CEIL(값) : 값의 소수점을 올림처리
-- MOD(값1, 값2) : 값1을 값2로 나눈 나머지
-- ABS(값) : 절대값 처리
SELECT ROUND(3.1415, 1),
       FLOOR(3.1415), TRUNC(3.1415, 2),
       CEIL(3.1415),
       MOD(5, 2),
       ABS(-7)
FROM DUAL
;
```

- **DBMS_RANDOM.RANDOM / DBMS_RANDOM.VALUE / DBMS_RANDOM.STRING**

```sql
-- DBMS_RANDOM.RANDOM : 2의 -31승 이상 2의 31승 미만의 정수 생성
-- DBMS_RANDOM.VALUE(값1, 값2) : 값1 이상, 값2 미만의 범위에서 난수 생성
-- DBMS_RANDOM.STRING(옵션, 길이) : 옵션에 따라 임의 문자열을 길이만큼 생성
-- 옵션 : U = 대문자만, L = 소문자만, A = 대소문자, X = 대문자 + 숫자, P = 출력가능 모든 
SELECT DBMS_RANDOM.RANDOM,
       FLOOR(DBMS_RANDOM.VALUE(1, 46)),
       DBMS_RANDOM.STRING('X', 12),
       DBMS_RANDOM.STRING('P', 12)
FROM DUAL
;
```

- **날짜와 시간 관련**

```sql
-- SYSDATE : 현재시간(ORACLE 설치된 시스템 기준)
-- TO_CHAR(날짜, 형식) : 날짜를 형식에 맞추어 제공
-- Y : 연도
-- M : 월
-- DD : 일
-- D, DAY, DY : 요일
-- AM, FM : 오전, 오후
-- HH : 시(12시간 기준)
-- HH24: 시(24시간 기준)
-- MI : 분
-- SS : 초
-- W : 달의 몇번째 주
-- IW : 연 몇번째 주
-- Q : 분기
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD DAY AM HH:MI:SS'),
       TO_CHAR(SYSDATE, 'W IW Q')
FROM DUAL 
;
```

- **통화기호**

```sql
-- 지역이 미국버전이면 달러가 나오고, 한국이면 원이 나오고
-- 출력 결과가 ####인 경우 : 형식 변환이 정상적으로 처리가 되지 않을 때
-- 9 : 숫자 자릿수
-- L : 시스템 지역 통화 기호
SELECT TO_CHAR(123456789, '999,999,999,999')
FROM DUAL
;
```

- **날짜 - 숫자**

```sql
-- 날짜 - 숫자 : 해당날짜로부터 숫자만큼 이전일
SELECT SYSDATE - 7
FROM DUAL
;
```

- **실습**

```sql
-- 1, 3 분기에 입사한 사원들을 구하시오

SELECT ENAME, TO_CHAR(HIREDATE)
FROM EMP
WHERE TO_CHAR(HIREDATE, 'Q') = '3' OR TO_CHAR(HIREDATE, 'Q') = '1'
-- WHERE MOD(TO_CHAR(HIREDATE, 'Q'), 2) = 1
```

- **CONCAT**

```sql
-- CONCAT(값1, 값2) : 문자열 값1과 값2를 이어준다. 문자열 합치기. 하지만 이것만 사용하지는 않는다.
SELECT CONCAT (CONCAT ('HELLO', ' WROLD'), '!!')
FROM DUAL
;
```

- **||**

```sql
-- || : 문자열을 이어준다. 일반적으로 더 쉽기 때문에 CONCAT보다는 이것을 쓴다.
SELECT 'HELLO' || ' WORLD' || '!!'
FROM DUAL
;
```

- **LENGTH / SUBSTR / INSTR / REPLACE / LOWER / UPPER**

```sql
-- LENGTH(값) : 값의 길이를 구할 때
-- 오라클은 인덱스 기반이 아니다. 개수 기반임. 
-- SUBSTR(값, 숫자1, 숫자2) : 값에서 숫자 1번째 글자부터 숫자 2개만큼 자른다.
-- INSTR(값1, 값2) : 값1에서 값2의 위치를 앞에서부터 찾는다.
-- INSTR(값1, 값2, 숫자) : 값1에서 숫자번째 글자부터 값2의 위치를 앞에서부터 찾는다.
-- INSTR(값1, 값2, -1) : 값1에서 값2의 위치를 뒤에서부터 찾는다.
-- INSTR(값1, 값2, -1, 숫자) : 값1에서 숫자번째 값2의 위치를 뒤에서부터 찾는다.
-- REPLACE(값1, 값2, 값3) : 값1에서 값2를 찾아 값3으로 변경
-- LOWER(값) : 값을 소문자화
-- UPPER(값) : 값을 대문자화
SELECT LENGTH('HELLO WORLD!!'), SUBSTR('HELLO WORLD!!', 8, 3),
       INSTR('HELLO WORLD!!', 'L', 5),
       INSTR('HELLO WORLD!!', 'L', -1, 1),
       REPLACE('HELLO WORLD!!', 'L', 'K'),
       LOWER('HELLO WORLD!!'), UPPER('hello world!!')
FROM DUAL
;
```

![스크린샷 2021-12-14 오후 3.07.28.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b46899b1-4747-42ca-a071-b0197a737c0e/스크린샷_2021-12-14_오후_3.07.28.png)

- **CASE - 자바의 if문과 비슷**

```sql
CASE WHEN -- 조건 / 조건이 true이면 값을 돌려준다.
		 THEN -- 값
		 ...
		 ELSE -- 값n(선택) / 모두 false이면 값n을 돌려준다.
END

-- 예시
SELECT CASE WHEN MOD(6, 2) = 1
            THEN '홀수입니다.'
            ELSE '짝수입니다.'
       END 
FROM DUAL
;
```

- **DECODE(값, 값1, 값1a, 값2, 값2a, ..., 값n(선택))**

```sql
-- DECODE : 자바에서 switch와 비슷하다.
SELECT DECODE(MOD(3, 2), 1, '홀수입니다.', '짝수입니다.')
FROM DUAL
;
-- 값이 값1과 같으면 값1a를 돌려준다.
-- 값이 값2와 같으면 값2a를 돌려준다.
-- 일치하는 것이 없으면 값n을 돌려준다.
```

- **NVL**

```sql
-- NVL(값1, 값2) : 값1이 NULL이면 값2로 대체. NULL VALUE LESS
SELECT ENAME, SAL, COMM, SAL + NVL(COMM, 0)
FROM EMP
;
```

- **실습2**

```sql
-- 성과급이 없는 사원들을 구하시오.
-- 조건 중 IS NULL : NULL인 경우 TRUE
-- 조건 중 IS NOT NULL : NULL이 아닌 경우 TRUE
SELECT *
FROM EMP
WHERE COMM IS NULL
;
```

- **NOT IN**

```sql
-- NOT IN : IN의 반대
SELECT *
FROM EMP
WHERE SAL NOT IN (800, 1100, 1300)
;
```

- **커미션을 받지 않는 사원의 급여를 10% 인상하여 전체 사원의 실 급여를 구하시오.(실급여 = 급여 + 커미션)**
- 1번 풀이

```sql
-- 커미션을 받지 않는 사원의 급여를 10% 인상하여
-- 전체 사원의 실 급여를 구하시오.(실급여 = 급여 + 커미션)
SELECT ENAME, SAL, COMM, SAL + NVL(COMM, SAL * 0.1) AS RSAL
FROM EMP
;
```

- 2번 풀이

```sql
SELECT ENAME, SAL, COMM,
       CASE WHEN COMM IS NULL
            THEN SAL * 1.1
            ELSE SAL + COMM
       END AS RSAL
FROM EMP
;
```

- 3번 풀이

```sql
SELECT ENAME, SAL, COMM, SAL + NVL(COMM, SAL * 0.1) AS RSAL
FROM EMP
;
```

---

<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />









### UNION

- UNION - 중복된 ROW는 제거하고 합침
- UNION ALL - 중복 상관 없이 합침

![Alt text](/Users/paikjonghun/GitHub/TIL/Database/Oracle-Database/UNION.png)

JOIN은 옆으로 붙이는데, UNION은 세로로 붙일 수 있다.

- 사용법

```sql
SELECT문

UNION

SELECT문
;
```

다만, 조건이 있다. 위와 아래의 **실행 결과 컬럼의 개수와 타입이 동일**해야 함.

명칭은 **위의 것 기준**으로 동작.

유니온은 통계를 할 때 가끔 쓸 때가 있다.

```sql
SELECT ENAME, SAL
FROM EMP
UNION
SELECT ENAME, SAL
FROM EMP
;

SELECT ENAME, SAL
FROM EMP
UNION ALL
SELECT ENAME, SAL
FROM EMP
;

-- 입사 연도별 급여 평균, 입사 연도별 월별 급여 평균을 구하시오.
SELECT TO_CHAR(HIREDATE, 'YYYY') AS DT, AVG(SAL) AS AVG_SAL, 'YEAR' AS GBN
FROM EMP
GROUP BY TO_CHAR(HIREDATE, 'YYYY')
UNION
SELECT TO_CHAR(HIREDATE, 'YYYY/MM') AS DT, AVG(SAL) AS AVG_SAL, 'MON' AS GBN
FROM EMP 
GROUP BY TO_CHAR(HIREDATE, 'YYYY/MM')
ORDER BY GBN DESC, DT ASC
;
```

### 테이블 생성

- 테이블 만들기
- 새 테이블 누르기
- PK 클릭해서 PK 설정
- 데이터 타입 오라클은 세 가지만 기억하면 됨
    - NUMBER
    - VARCHAR2
    - DATE

![스크린샷 2021-12-24 오전 11.20.29.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d06d2954-2d52-417d-b6d3-3b405f89edb0/스크린샷_2021-12-24_오전_11.20.29.png)

오라클은 VARCHAR2 버전을 쓰는데, 그 외에는 varchar를 씀

VARCHAR2의 사이즈 제한이 4000byte가 MAX임.

BLOB은 4GB 까지 가능

CLOB은 32GB 까지 가능

### 알아야하는 DDL은

- CREAT - 생성
- ALTER - 변경
- DROP - 제거

### DCL은 권한

- GRANT - 부여하다
- REVOKE - 권한 회수

![스크린샷 2021-12-24 오후 12.11.25.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a004a73e-9335-4068-8767-0490281e1f88/스크린샷_2021-12-24_오후_12.11.25.png)

### INSERT (DML) - 데이터 추가

```sql
INSERT  INTO 테이블명(컬럼명, ...) --(컬럼명)에 기본값이 지정된 것이나 NULL을 허용한 것은 생략가능
VALUES(값,...) -- 테이블명 뒤에 선언된 컬럼들의 개수와 타입이 동일해야 함. 단, 어느정도
							 -- 자동형변환 제공
;

INSERT INTO TEST(T_NO, T_CON)
VALUES(1, ‘TEST1’)
;
```

T_NO는 고유번호. 제약조건.

데이터에 대해서 제약조건을 걸기 때문에, 같은 값은 무조건 또 못들어감. 유니크라는 제약 조건임.

프라이머리 키는 중복해서 들어갈 수 없다.

### 트랜잭션

TRANSACTION - 데이터 안정성을 위하여 데이터 수정 행위들을 작업으로 관리

- ROLLBACK - 마지막 COMMIT 시점까지 진행중인 작업을 취소한다.
- COMMIT - 작업들을 적용한다. 저장한다.

![스크린샷 2021-12-24 오후 1.41.37.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f47533f-8377-45b4-b50a-4bc653c53f30/스크린샷_2021-12-24_오후_1.41.37.png)

### **UPDATE (DML) - 데이터 수정**

```sql
UPDATE 테이블명 SET 컬럼 = 값, ... -- 값 직접입력 or 컬럼명(컬럼명이 나올 경우, 
																-- 해당 ROW에 컬럼의 값을 넣어준다.
WHERE 조건; -- 조건이 없는 경우 모든 row를 업데이트 한다.*** 신입 개발자가 많이하는 실수
```

따라서, 툴에 따라 auto commit 설정이 있는 경우 해당 기능을 비활성화 해야한다. 예민하게 반응해야 한다.

검색해보고 설정 끄고 쿼리 작업해야하고, 쿼리 작성할 때 where 조건 넣었는지 확인해야 한다.

조건이 없을 때, 내가 이걸 정말 다 바꾸려고 하는 게 맞는지 한번 더 확인해야 함.

![스크린샷 2021-12-24 오후 1.48.54.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bcdaea5b-be03-4534-acb3-8c9899eb1eb9/스크린샷_2021-12-24_오후_1.48.54.png)

### DELETE (DML) - ROW를 제거

SELECT와 닮음 - 하지만 DELETE 뒤에 아무것도 안 들어가는 이유 : row를 삭제하는 것이므로 컬럼 정보 필요 X)

```sql
DELETE FROM 테이블명
WHERE 조건; -- 조건이 없는 경우 모든 row를 제거***
```

***** 수정, 삭제는 WHERE를 빼놓고 얘기할 수 없다!**

![스크린샷 2021-12-24 오후 2.07.37.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4973250a-3950-4db1-bbec-1ad069490648/스크린샷_2021-12-24_오후_2.07.37.png)

### SEQUENCE - 자동 증가 객체. oracle에만 존재.

![스크린샷 2021-12-26 오후 7.26.22.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a20986aa-b071-4d89-a240-aec62c9ade3e/스크린샷_2021-12-26_오후_7.26.22.png)

일반적으로 시퀀스를 만들 때 테이블명_SEQ.

9를 28개까지 최대값으로 둘 수 있음.(9999999999999999999999999999)

**캐쉬**는 일반적으로 없음으로 둠. 캐쉬는 휘발성 메모리. 

**주기**는 끝까지 값이 갔을 때 다시 처음부터 갈거냐고 말하는 것. 보통 프라이머리 키에서 시퀀스를 가져다 쓰게 되는데, 저 숫자를 벗어나게 되면 반복되는 숫자가 나오기 때문에 주기는 보통 없음.

**정렬**은 캐쉬가 있으면 쓰면 되는데, 캐쉬 없으니까 안 써도 됨.

시퀀스명.NEXTVAL : 해당 구문 실행시 **1회간** 증분수치를 적용하여 조회

시퀀스명.CURRVAL : 현재 시퀀스 값 조회

```sql
SELECT TEST_SEQ.NEXTVAL, TEST_SEQ.CURRVAl
FROM DUAL
;
```

***시퀀스는 트랜잭션의 영향을 받지 않는다.

***시퀀스는 현재값 수정이 불가능