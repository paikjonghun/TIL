# Oracle Database에 대하여

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