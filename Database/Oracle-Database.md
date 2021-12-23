### Oracle Database 실습
쿼리를 작성할 때 중요한 것은
1. 무엇을 구하는가? - 어떤 데이터를 구하려고 하는 것인가?
2. 조건 찾기
3. 필요한 것은?
4. 서브쿼리 안쪽부터 쿼리 작성
5. 검증

- 문제 1번 - HR데이터에서 각 상급자별 부하 직원이 가장 많은 업무를 구하시오.

-> '상급자별' '부하직원 업무별' 인원수 중 상급자별 부하 직원 수 1위인 업무를 구하기.

1. 첫 번째 단계 - EMPLOYEES 테이블의 MANAGER_ID와, JOB_ID을 이용해서 바로 구할 수 있다.

```sql
SELECT MANAGER_ID, JOB_ID, COUNT(*) AS CNT
			 RANK() OVER(PARTITION BY MANAGER_ID ORDER BY COUNT(*) DESC) AS RNK
FROM EMPLOYEES
GROUP BY MANAGER_ID, JOB_ID
```

2. 두 번째 단계 - EMPLOYEES 테이블과 JOBS 테이블을 조인해서(조건은 '서브 쿼리의 MANAGER_ID'와 'EMPLOYEE 테이블의 EMPLOYEE_ID'가 같다는 것과 '서브 쿼리의 JOB_ID'와 'JOBS 테이블의 JOB_ID'가 같다는 것을 활용) 상급자의 FIRST_NAME과 부하 직원의 JOB_TITLE을 구한다.

```sql
SELECT E.FIRST_NAME, J.JOB_TITLE, R.CNT, R.RNK
FROM (SELECT MANAGER_ID, JOB_ID, COUNT(*) AS CNT,
             RANK() OVER(PARTITION BY MANAGER_ID ORDER BY COUNT(*) DESC) AS RNK
      FROM EMPLOYEES
      GROUP BY MANAGER_ID, JOB_ID) R INNER JOIN EMPLOYEES E
                                             ON R.MANAGER_ID = E.EMPLOYEE_ID
                                     INNER JOIN JOBS J
                                             ON R.JOB_ID = J.JOB_ID
WHERE R.RNK = 1
ORDER BY E.FIRST_NAME ASC, J.JOB_TITLE ASC
;
```

- 문제 2번 - HR 데이터에서 각 부서의 급여 1등 사원이 가장 많이 거주하는 대륙은?
```sql
SELECT RB.REGION_NAME, REGION_ID, RB.CNT
FROM (SELECT RA. REGION_ID, RA.REGION_NAME, COUNT(*) AS CNT, RANK() OVER(ORDER BY COUNT(*) DESC) AS RNK
      FROM (SELECT C.COUNTRY_ID, CA.REGION_ID
            FROM (SELECT L.COUNTRY_ID
                  FROM (SELECT EMPLOYEE_ID, DEPARTMENT_ID, LOCATION_ID, RNK -- 1순위 골라내기
                        FROM (SELECT E.EMPLOYEE_ID, D.DEPARTMENT_ID, LOCATION_ID, RANK() OVER(PARTITION BY D.DEPARTMENT_ID ORDER BY SALARY DESC) AS RNK -- 부서별 1등 구하기
                              FROM EMPLOYEES E INNER JOIN DEPARTMENTS D
                                                       ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
                              GROUP BY E.EMPLOYEE_ID, D.DEPARTMENT_ID, SALARY, D.LOCATION_ID)
                        WHERE RNK = 1) EE INNER JOIN LOCATIONS L
                                                  ON EE.LOCATION_ID = L.LOCATION_ID) C 
                                          INNER JOIN COUNTRIES CA
                                                  ON C.COUNTRY_ID = CA.COUNTRY_ID) R 
                                          INNER JOIN REGIONS RA
                                                  ON R.REGION_ID = RA.REGION_ID
      GROUP BY RA. REGION_ID, RA.REGION_NAME) RB
WHERE RNK = 1
;
```