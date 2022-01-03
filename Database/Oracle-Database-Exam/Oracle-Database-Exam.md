### Oracle Database 실습
쿼리를 작성할 때 중요한 것은
1. 무엇을 구하는가? - 어떤 데이터를 구하려고 하는 것인가?
2. 조건 찾기
3. 필요한 것은?
4. 서브쿼리 안쪽부터 쿼리 작성
5. 검증

---

- 문제  - HR 데이터에서 각 상급자별 부하 직원이 가장 많은 업무를 구하시오. -> '상급자별' '부하직원 업무별' 인원수 중 상급자별 부하 직원 수 1위인 업무를 구하기.

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

- 문제 - HR 데이터에서 각 부서의 급여 1등 사원이 가장 많이 거주하는 대륙은?
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


1. SCOTT 데이터 - 부서별 평균 급여 순위 1등을 구하시오.
    ```sql
    -- 내 풀이
    SELECT DNAME, AVG_SAL, RNK 
    FROM (SELECT D.DEPTNO, D.DNAME, ROUND(AVG(SAL), 2) AS AVG_SAL, 
          RANK() OVER(ORDER BY AVG(SAL) DESC) AS RNK
          FROM EMP E INNER JOIN DEPT D
                             ON E.DEPTNO = D.DEPTNO
          GROUP BY D.DEPTNO, D.DNAME)
    WHERE RNK = 1
    ;
    -- 강사님 풀이
    SELECT D.DNAME, E.AVG_SAL, E.RNK
    FROM DEPT D INNER JOIN (SELECT DEPTNO, ROUND(AVG(SAL), 2) AS AVG_SAL,
                                   RANK() OVER(ORDER BY AVG(SAL) DESC) AS RNK
                            FROM EMP
                            GROUP BY DEPTNO) E
                        ON D.DEPTNO = E.DEPTNO
                       AND E.RNK = 1
    ;
    ```
    
1. HR 데이터 - 각 부서별 급여 1등 사원이 가장 많이 거주하는 대륙을 구하시오.
    
    ```sql
    -- 부서별 급여 1등 사원의 수 대륙별로 찾기.
    SELECT R.REGION_ID, R.REGION_NAME, COUNT(*) AS CNT
    FROM REGIONS R INNER JOIN COUNTRIES C
                           ON R.REGION_ID = C.REGION_ID
                   INNER JOIN LOCATIONS L
                           ON C.COUNTRY_ID = L.COUNTRY_ID
                   INNER JOIN DEPARTMENTS D
                           ON L.LOCATION_ID = D.LOCATION_ID
                   INNER JOIN (SELECT DEPARTMENT_ID,
                                      RANK() OVER(PARTITION BY DEPARTMENT_ID
                                                  ORDER BY SALARY DESC) AS RNK
                               FROM EMPLOYEES) E
                           ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
                          AND E.RNK = 1
    GROUP BY R.REGION_ID, R.REGION_NAME
    ;
    
    -- 부서별 급여 1등 사원의 수 대륙별 1등 찾기.
    SELECT R.REGION_NAME AS RESION, R.CNT, R.RNK
    FROM (SELECT R.REGION_ID, R.REGION_NAME, COUNT(*) AS CNT,
                 RANK() OVER(ORDER BY COUNT(*) DESC) AS RNK
          FROM REGIONS R INNER JOIN COUNTRIES C
                                 ON R.REGION_ID = C.REGION_ID
                         INNER JOIN LOCATIONS L
                                 ON C.COUNTRY_ID = L.COUNTRY_ID
                         INNER JOIN DEPARTMENTS D
                                 ON L.LOCATION_ID = D.LOCATION_ID
                         INNER JOIN (SELECT DEPARTMENT_ID,
                                            RANK() OVER(PARTITION BY DEPARTMENT_ID
                                                        ORDER BY SALARY DESC) AS RNK
                                     FROM EMPLOYEES) E
                                 ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
                                AND E.RNK = 1
          GROUP BY R.REGION_ID, R.REGION_NAME) R
    WHERE R.RNK = 1
    ;
    ```

1. SCOTT 데이터 - 10번 부서의 사람들 중에서 20번 부서의 사원과 같은 업무를 하는 사원의 사원번호, 이름, 부서명, 입사일, 지역을 출력하고, 전체 사원 중 급여 순위를 구하시오.
    
    ```sql
    SELECT E.EMPNO, E.ENAME, D.DNAME, E.HIREDATE, D.LOC, E.RNK
    FROM (SELECT EMPNO, ENAME, DEPTNO,
                 TO_CHAR(HIREDATE, 'YYYY/MM/DD') AS HIREDATE, JOB,
                 RANK() OVER(ORDER BY SAL DESC) AS RNK
          FROM EMP) E INNER JOIN DEPT D
                              ON E.DEPTNO = D.DEPTNO
    WHERE E.DEPTNO = 10
    AND E.JOB IN (SELECT JOB
                  FROM EMP
                  WHERE DEPTNO = 20)
    ;
    ```
    
1. HR 데이터 - LAST_NAME이 Bloom인 사람보다 급여가 많은 사원 중 가장 많이 버는 사람을 제외한, 평균 급여를 출력하시오.
    
    ```sql
    -- LAST_NAME이 Bloom 인 사람의 급여 찾기.
    SELECT SALARY
    FROM EMPLOYEES
    WHERE LAST_NAME = 'Bloom'
    ;
    
    -- LAST_NAME이 Bloom인 사람의 급여 찾고, 그 급여보다 큰 급여들을 뽑고 순위 매기고, 
    -- 1등 제외하고 평균 급여 구하기.
    SELECT ROUND(AVG(E.SALARY), 2) AS AVG_SAL
    FROM (SELECT E.SALARY, RANK() OVER(ORDER BY E.SALARY DESC) AS RNK
          FROM EMPLOYEES E INNER JOIN (SELECT SALARY
                                       FROM EMPLOYEES
                                       WHERE LAST_NAME = 'Bloom') S
                                   ON E.SALARY > S.SALARY) E
    WHERE E.RNK != 1
    ;
    ```
    


1. SCOTT 데이터 - 댈러스에 근무하고 급여등급이 4등급인 사원들의 평균 급여를 구하시오.
    
    ```sql
    SELECT ROUND(AVG(SAL), 1) AS AVG_SAL
    FROM EMP E INNER JOIN DEPT D
                     ON E.DEPTNO = D.DEPTNO
                    AND D.LOC = 'DALLAS'
             INNER JOIN SALGRADE SG
                     ON E.SAL BETWEEN SG.LOSAL AND SG.HISAL
                    AND SG.GRADE = 4
    ;
    ```
    
1. HR 데이터 - 업무가 STOCK MANAGER인 사원의 급여순위를 구하시오.
    
    ```sql
    SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.SALARY,
           RANK() OVER(ORDER BY E.SALARY DESC) AS RNK
    FROM EMPLOYEES E INNER JOIN JOBS J
                             ON E.JOB_ID = J.JOB_ID
                            AND UPPER(J.JOB_TITLE) = 'STOCK MANAGER'
    ;
    ```
    
1. SCOTT 데이터 - 이름이 ‘A’로 시작하는 사원 중 급여가 가장 높은 사람의 부서명과 급여를 구하시오.
    
    ```sql
    SELECT E.ENAME, D.DNAME, E.SAL
    FROM (SELECT ENAME, DEPTNO, SAL,
                 RANK() OVER(ORDER BY SAL DESC) AS RNk
          FROM EMP
          WHERE ENAME LIKE 'A%') E INNER JOIN DEPT D
                                           ON E.DEPTNO = D.DEPTNO
    WHERE E.RNK = 1
    ;
    ```
    
1. HR 데이터 - 각 부서 내에서 개인의 임금이 업무별 평균임금 미만인 직원의 임금을 40%, 이상인 직원의 임금을 20% 인상하시오. (단, 인상된 임금은 업무별 임금 최대치를 넘길 수 없다.)
    
    ```sql
    -- 업무별 평균임금 구하기
    SELECT JOB_ID, AVG(SALARY) AS AVG_SAL
    FROM EMPLOYEES
    GROUP BY JOB_ID
    ;
    
    -- 업무별 평균임금 미만인 직원의 임금을 40%, 이상인 직원의 임금을 20% 인상.
    SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.JOB_ID, E.DEPARTMENT_ID, E.SALARY,
           CASE WHEN E.SALARY < EA.AVG_SAL
                THEN E.SALARY * 1.4
                ELSE E.SALARY * 1.2
           END AS RSAL, J.MAX_SALARY
    FROM EMPLOYEES E INNER JOIN (SELECT JOB_ID, AVG(SALARY) AS AVG_SAL
                                 FROM EMPLOYEES
                                 GROUP BY JOB_ID) EA
                             ON E.JOB_ID = EA.JOB_ID
                     INNER JOIN JOBS J
                             ON E.JOB_ID = J.JOB_ID
    ;
    
    -- '업무별 평균임금 미만인 직원의 임금을 40%, 이상인 직원의 임금을 20% 인상'한 것에서 임금이
    -- 임금 최대치를 넘으면 MAX_SALARY로 돌려준다.
    SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.JOB_ID, E.DEPARTMENT_ID, E.SALARY,
           CASE WHEN E.RSAL > E.MAX_SALARY
                THEN E.MAX_SALARY
                ELSE E.RSAL
           END AS RSAL, E.MAX_SALARY
    FROM (SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.JOB_ID, E.DEPARTMENT_ID, E.SALARY,
                 CASE WHEN E.SALARY < EA.AVG_SAL
                      THEN E.SALARY * 1.4
                      ELSE E.SALARY * 1.2
                 END AS RSAL, J.MAX_SALARY
           FROM EMPLOYEES E INNER JOIN (SELECT JOB_ID, AVG(SALARY) AS AVG_SAL
                                        FROM EMPLOYEES
                                        GROUP BY JOB_ID) EA
                                    ON E.JOB_ID = EA.JOB_ID
                            INNER JOIN JOBS J
                                    ON E.JOB_ID = J.JOB_ID) E
    ORDER BY E.EMPLOYEE_ID
    ;
    ```
    
1. SCOTT 데이터 - 댈러스에 사는 연도별 입사자 급여 1순위를 구하시오.
    
    ```sql
    SELECT E.YY, E.SAL, E.RNK, E.LOC
    FROM (SELECT TO_CHAR(E.HIREDATE, 'YYYY') AS YY, E.SAL, D.LOC,
                 RANK() OVER(PARTITION BY TO_CHAR(E.HIREDATE, 'YYYY') 
    										ORDER BY E.SAL DESC) AS RNK
          FROM EMP E INNER JOIN DEPT D
                             ON E.DEPTNO = D.DEPTNO
                            AND D.LOC = 'DALLAS') E
    WHERE E.RNK = 1
    ;
    ```
    
1. HR 데이터 - 발령을 받았던 사람들 중 가장 많이 받은 사람의 현재 이름, 직업, 급여, 근무도시를 출력하시오.
    
    ```sql
    -- 사원별 발령 순위 뽑기
    SELECT EMPLOYEE_ID, COUNT(*) AS CNT,
           RANK() OVER(ORDER BY COUNT(*) DESC) AS RNK
    FROM JOB_HISTORY
    GROUP BY EMPLOYEE_ID
    ;
    
    -- 사원별 발령 1순위의 이름. 직업, 급여, 근무도시 뽑기
    SELECT E.FIRST_NAME, J.JOB_TITLE, E.SALARY, L.CITY
    FROM EMPLOYEES E INNER JOIN (SELECT EMPLOYEE_ID, COUNT(*) AS CNT,
                                        RANK() OVER(ORDER BY COUNT(*) DESC) AS RNK
                                 FROM JOB_HISTORY
                                 GROUP BY EMPLOYEE_ID) JH
                             ON E.EMPLOYEE_ID = JH.EMPLOYEE_ID
                            AND JH.RNK = 1
                     INNER JOIN JOBS J
                             ON E.JOB_ID = J.JOB_ID
                     INNER JOIN DEPARTMENTS D
                             ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
                     INNER JOIN LOCATIONS L
                             ON D.LOCATION_ID = L.LOCATION_ID
    ;
    ```