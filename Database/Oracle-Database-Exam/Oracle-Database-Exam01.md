### SCOTT 1. 사원번호가 짝수인 사원들 중 급여등급이 2등급인 사람의 월급을 10% 인상하시오.

```sql
SELECT E.EMPNO, E.SAL * 1.1 AS RSAL, SG.GRADE
FROM EMP E INNER JOIN SALGRADE SG
                   ON E.SAL BETWEEN SG.LOSAL AND SG.HISAL
                  AND SG.GRADE = 2
WHERE MOD(E.EMPNO, 2) = 0
;
```

### SCOTT 2. 상반기에 입사하고 급여등급이 4등급인 사원들 중 급여가 가장 높은 사원의 정보를 출력하시오.

```sql
SELECT E.ENAME, E.JOB, E.SAL, E.DT, E.RNK
FROM (SELECT E.ENAME, E.JOB, E.SAL, TO_CHAR(E.HIREDATE, 'YYYY-MM') AS DT,
             RANK() OVER(ORDER BY SAL DESC) AS RNK
      FROM EMP E INNER JOIN SALGRADE SG
                         ON E.SAL BETWEEN SG.LOSAL AND SG.HISAL
                        AND SG.GRADE = 4
      WHERE TO_CHAR(HIREDATE, 'Q') < 3) E
WHERE E.RNK = 1
;
```
### SCOTT 3. 급여가 2000 이상인 사원들을 포함하고 있는 부서들의 부서 정보와 사원 수에 따른 순위를 출력하시오.

```sql
SELECT D.DEPTNO, D.DNAME, D.LOC, E.CNT, E.RNK, E.TC_RNK
FROM (SELECT DEPTNO, COUNT(*) AS CNT,
             RANK() OVER(ORDER BY COUNT(*) DESC) AS RNK,
             SUM(CASE WHEN SAL >= 2000
                      THEN 1
                      ELSE 0
                 END) AS TC,
             RANK() OVER(ORDER BY SUM(CASE WHEN SAL >= 2000
                                           THEN 1
                                           ELSE 0
                                      END) DESC) AS TC_RNK
      FROM EMP
      GROUP BY DEPTNO) E INNER JOIN DEPT D
                                 ON E.DEPTNO = D.DEPTNO
WHERE E.TC > 0
ORDER BY E.TC_RNK ASC
;
```
### SCOTT 4. 급여등급이 3등급이상이면 '진급' 3등급 미만이면 '누락'으로 출력하고, 사원들의 급여 별 순위와 사원정보를 함께 출력하시오.

```sql
SELECT E.EMPNO, E.ENAME, E.JOB, E.SAL, SG.GRADE,
       CASE WHEN SG.GRADE >= 3
            THEN '진급'
            ELSE '누락'
       END AS UP_DOWN, RANK() OVER(ORDER BY SAL DESC) AS RNK
FROM EMP E INNER JOIN SALGRADE SG
                   ON E.SAL BETWEEN SG.LOSAL AND SG.HISAL
;
```
### SCOTT 5. 시카고에서 근무하는 연도별 입사자 중 급여 2순위를 구하고 시카고 급여 2위인 사원이 1985년도 전에 입사했다면 급여를 20% 인상시키시오.

```sql
SELECT E.ENAME, E.YY, E.RNK, E.LOC, E.RSAL
FROM (SELECT TO_CHAR(E.HIREDATE, 'YYYY') AS YY, ENAME, D.LOC,
             CASE WHEN TO_CHAR(E.HIREDATE, 'YYYY') < 1985
                  THEN E.SAL * 1.2
                  ELSE E.SAL
             END AS RSAl,
             RANK() OVER(PARTITION BY TO_CHAR(HIREDATE, 'YYYY') ORDER BY SAL DESC) AS RNK
      FROM EMP E INNER JOIN DEPT D
                         ON E.DEPTNO = D.DEPTNO
                        AND D.LOC = 'CHICAGO') E
WHERE E.RNK = 2
;
```

### SCOTT 6. 근무지별로 근무하는 사원의 수를 구하고 가장 많이 근무하고 있는 근무지에 있는 사원들의 이름을 출력하시오.

```sql
SELECT E.ENAME
FROM EMP E INNER JOIN DEPT D
                   ON E.DEPTNO = D.DEPTNO
           INNER JOIN (SELECT D.LOC, RANK() OVER(ORDER BY COUNT(*) DESC) AS RNK
                       FROM EMP E INNER JOIN DEPT D
                                          ON E.DEPTNO = D.DEPTNO
                       GROUP BY D.LOC) L
                   ON D.LOC = L.LOC
                  AND L.RNK = 1
;
```
### SCOTT 7.  커미션을 가장 많이 받은 사원보다 급여등급이 높은 사원은 급여를 50% 삭감하고, 삭감 후의 전체 사원 급여등급과 급여순위를 구하시오.

```sql
SELECT E.ENAME, SG.GRADE,
       CASE WHEN SG.GRADE > S.GRADE
            THEN E.SAL * 0.5
            ELSE E.SAL
       END AS RSAL,
       RANK() OVER(ORDER BY CASE WHEN SG.GRADE > S.GRADE
                                 THEN E.SAL * 0.5
                                 ELSE E.SAL
                            END DESC) AS RNK
FROM EMP E INNER JOIN SALGRADE SG
                   ON E.SAL BETWEEN SG.LOSAL AND SG.HISAL
           INNER JOIN (SELECT SG.GRADE, 
															RANK() OVER(ORDER BY NVL(E.COMM, -1) DESC) AS RNK
                       FROM EMP E INNER JOIN SALGRADE SG
                                          ON E.SAL BETWEEN SG.LOSAL AND SG.HISAL) S
                   ON S.RNK = 1
;
```

### SCOTT 8. 매니저별 부하 직원들의 급여 평균을 구하고, 평균이 가장 높은 매니저와 가장 낮은 매니저의 이름과 부하직원 수, 부하직원의 평균 급여를 출력하시오.

```sql
SELECT E.ENAME, M.CNT, M.AVG_SAL
FROM EMP E INNER JOIN (SELECT MGR, AVG(SAL) AS AVG_SAL, COUNT(*) AS CNT,
                              RANK() OVER(ORDER BY AVG(SAL) DESC) AS UP_RNK,
                              RANK() OVER(ORDER BY AVG(SAL) ASC) AS DOWN_RNK
                       FROM EMP
                       WHERE MGR IS NOT NULL
                       GROUP BY MGR) M
                   ON E.EMPNO = M.MGR
                  AND (M.UP_RNK = 1 OR M.DOWN_RNK = 1)
;
```
### SCOTT 9. 급여가 전체 평균급여보다 작고 이름에 A가 들어가는 사원과 동일한 부서에서 근무하는 모든 사원의 사원번호, 사원명, 실수령액을 출력하시오.

```sql
SELECT E.EMPNO, E.ENAME, E.SAL + NVL(E.COMM, 0) AS RSAL
FROM EMP E INNER JOIN (SELECT DISTINCT E.DEPTNO
                       FROM EMP E INNER JOIN(SELECT AVG(SAL) AS AVG_SAL
                                             FROM EMP) A
                                          ON E.SAL < A.AVG_SAL
                       WHERE E.ENAME LIKE '%A%') D
                   ON E.DEPTNO = D.DEPTNO
;
```

### SCOTT 10. 업무가 CLERK인 직원이 2명 이상인 부서의 이름과 평균 급여를 출력하시오.

```sql
SELECT D.DNAME, A.AVG_SAL
FROM DEPT D INNER JOIN (SELECT DEPTNO
                        FROM EMP
                        WHERE JOB = 'CLERK'
                        GROUP BY DEPTNO
                        HAVING COUNT(*) >= 2) J
                    ON D.DEPTNO = J.DEPTNO
            INNER JOIN (SELECT DEPTNO, AVG(SAL) AS AVG_SAL
                        FROM EMP
                        GROUP BY DEPTNO) A
                    ON D.DEPTNO = A.DEPTNO
;
```


### HR 1. 부서번호가 50번인 사원들의 평균 급여를 구하고, 그보다 높은 급여를 받는 부서번호가 30번인 사원의 first_name과 급여를 출력하시오.

```sql
SELECT E.FIRST_NAME, E.SALARY
FROM EMPLOYEES E INNER JOIN (SELECT AVG(SALARY) AS AVG_SAL
                             FROM EMPLOYEES
                             WHERE DEPARTMENT_ID = 50) A
                         ON E.SALARY > A.AVG_SAL
WHERE E.DEPARTMENT_ID = 30
;
```

### HR 2. 부서별 최고급여를 받는 직원의 정보를 출력하시오.

```sql
SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.LAST_NAME, 
		   E.SALARY, D.DEPARTMENT_ID, D.DEPARTMENT_NAME
FROM (SELECT EMPLOYEE_ID, FIRST_NAME, LAST_NAME, SALARY, DEPARTMENT_ID,
             RANK() OVER(PARTITION BY DEPARTMENT_ID ORDER BY SALARY DESC) AS RNK
      FROM EMPLOYEES) E INNER JOIN DEPARTMENTS D
                                ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
WHERE E.RNK = 1
;
```

### HR 3. 부서별 급여평균 중 가장 낮은 부서의 급여평균과 입사연도별 급여평균이 가장 적은 해의 급여평균을 비교하여 부서의 급여평균이 높으면 해당 부서의 사원의 급여를 10% 인상하고, 낮으면 해당 부서의 사원의 급여를 10% 인하하여 해당 사원의 이름과 변경된 급여값을 출력하시오.

```sql
SELECT E.FIRST_NAME, E.DEPARTMENT_ID, E.SALARY,
       CASE WHEN D.AVG_SAL > Y.AVG_SAL
            THEN E.SALARY * 1.1
            ELSE E.SALARY * 0.9
       END AS RSAL
FROM EMPLOYEES E INNER JOIN (SELECT DEPARTMENT_ID, AVG(SALARY) AS AVG_SAL,
                                    RANK() OVER(ORDER BY AVG(SALARY) ASC) AS RNK
                             FROM EMPLOYEES
                             GROUP BY DEPARTMENT_ID) D
                         ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
                        AND D.RNK = 1
                 INNER JOIN (SELECT TO_CHAR(HIRE_DATE, 'YYYY'), 
																		AVG(SALARY) AS AVG_SAL,
                                    RANK() OVER(ORDER BY AVG(SALARY) ASC) AS RNK
                             FROM EMPLOYEES
                             GROUP BY TO_CHAR(HIRE_DATE, 'YYYY')) Y
                         ON Y.RNK = 1
;
```

### HR 4. 핸드폰번호 뒷자리가 34로 끝나는 직원들이 가장많은 업무의 급여평균을 구한 후 전체 직원의 급여를 비교하여 직원의 급여가 많을 경우 10% 삭감하여 전체 직원의 월급순위 1위부터 10위까지의 이름과 급여를 구하시오.

```sql
SELECT E.FIRST_NAME, E.RSAL, E.RNK
FROM (SELECT E.FIRST_NAME,
             CASE WHEN E.SALARY > A.AVG_SAL
                  THEN E.SALARY * 0.9
                  ELSE E.SALARY
             END AS RSAL,
             RANK() OVER(ORDER BY CASE WHEN E.SALARY > A.AVG_SAL
                                       THEN E.SALARY * 0.9
                                       ELSE E.SALARY
                                  END DESC) AS RNK
      FROM EMPLOYEES E INNER JOIN (SELECT JOB_ID, AVG(SALARY) AS AVG_SAL,
                                          RANK() OVER(ORDER BY COUNT(*) DESC) AS RNK
                                   FROM EMPLOYEES
                                   WHERE PHONE_NUMBER LIKE '%34'
                                   GROUP BY JOB_ID) A
                               ON A.RNK = 1) E
WHERE E.RNK BETWEEN 1 AND 10
;
```



### HR 5. 도시별로 급여 1위를 구하고 도시별 1위 중 04년도 입사자의 이름, 입사일, 급여, 근무중인 도시를 구하시오.

```sql
SELECT E.FIRST_NAME, E.HIRE_DATE, E.SALARY, E.CITY
FROM (SELECT E.FIRST_NAME, E.HIRE_DATE, E.SALARY, L.CITY,
             RANK() OVER(PARTITION BY L.CITY ORDER BY E.SALARY DESC) AS RNK
      FROM EMPLOYEES E INNER JOIN DEPARTMENTS D
                               ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
                       INNER JOIN LOCATIONS L
                               ON D.LOCATION_ID = L.LOCATION_ID) E
WHERE E.RNK = 1
AND TO_CHAR(E.HIRE_DATE, 'YY') = '04'
;
```

### HR 6. 발령을 받았던 사람들 중 발령을 가장 많이 받은 사람들이 근무하는 지역을 구하고 그 지역이 속해있는 대륙을 출력하시오.

```sql
SELECT DISTINCT L.CITY, R.REGION_NAME
FROM (SELECT EMPLOYEE_ID, RANK() OVER(ORDER BY COUNT(*) DESC) AS RNK
      FROM JOB_HISTORY
      GROUP BY EMPLOYEE_ID) JH INNER JOIN EMPLOYEES E
                                       ON JH.EMPLOYEE_ID = E.EMPLOYEE_ID
                               INNER JOIN DEPARTMENTS D
                                       ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
                               INNER JOIN LOCATIONS L
                                       ON D.LOCATION_ID = L.LOCATION_ID
                               INNER JOIN COUNTRIES C
                                       ON L.COUNTRY_ID = C.COUNTRY_ID
                               INNER JOIN REGIONS R
                                       ON C.REGION_ID = R.REGION_ID
WHERE JH.RNK = 1
;
```

### HR 7. 2006년 2분기 마지막 날 기준 재직자들의 부서별 인원수와 부서 이름, 부서별 인원수 랭킹을 출력하시오.

```sql
SELECT D.DEPARTMENT_NAME, E.CNT, E.RNK
FROM (SELECT DECODE(JH.DEPARTMENT_ID, NULL, 
						 E.DEPARTMENT_ID, JH.DEPARTMENT_ID) AS DEPARTMENT_ID,
             COUNT(*) AS CNT, RANK() OVER(ORDER BY COUNT(*) DESC) AS RNK
      FROM EMPLOYEES E LEFT OUTER JOIN JOB_HISTORY JH
                                    ON E.EMPLOYEE_ID = JH.EMPLOYEE_ID
                                   AND TO_DATE('2006-07-01') - 1 BETWEEN 
																			 JH.START_DATE AND JH.END_DATE
      WHERE HIRE_DATE < '2006-07-01'
      GROUP BY DECODE(JH.DEPARTMENT_ID, NULL, E.DEPARTMENT_ID, JH.DEPARTMENT_ID)) E 
INNER JOIN DEPARTMENTS D
        ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
ORDER BY E.RNK ASC
;
```

### HR 8. ‘Seattle’ (CITY)에서 근무하는 사원과 해당사원의 매니저, 부서를 구하시오. (이름은 LAST_NAME 으로 구한다.)

```sql
SELECT E.LAST_NAME AS E_NAME, E2.LAST_NAME AS M_NAME, D.DEPARTMENT_NAME
FROM EMPLOYEES E INNER JOIN DEPARTMENTS D
                         ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
                 INNER JOIN LOCATIONS L
                         ON D.LOCATION_ID = L.LOCATION_ID
                        AND L.CITY = 'Seattle'
                 INNER JOIN EMPLOYEES E2
                         ON E.MANAGER_ID = E2.EMPLOYEE_ID
ORDER BY E.LAST_NAME ASC
;
```

### HR 9. 부서별로 가장 적은 급여를 받고 있는 직원의 급여만 10% 인상하고 전체 급여 순위를 뽑으시오.

```sql
SELECT E.EMPLOYEE_ID, E.SALARY, DECODE(E.RNK, 1, E.SALARY * 1.1, E.SALARY) AS RSAL,
       RANK() OVER(ORDER BY DECODE(E.RNK, 1, E.SALARY * 1.1, E.SALARY) DESC) AS RNK
FROM (SELECT EMPLOYEE_ID, SALARY, 
	    RANK() OVER(PARTITION BY DEPARTMENT_ID ORDER BY SALARY ASC) AS RNK
      FROM EMPLOYEES) E
;
```

### HR 10. 부서별 직원들의 최대급여, 최소급여, 평균급여를 조회하되, 평균급여가 ‘IT’ 부서의 평균급여보다 많고, ‘Sales’ 부서의 평균보다 적은 부서 정보만 출력하시오.

```sql
SELECT D.DEPARTMENT_ID, D.DEPARTMENT_NAME, MAX(E.SALARY) AS MAX, 
			 MIN(E.SALARY) AS MIN, ROUND(AVG(E.SALARY), 2) AS AVG_SAL
FROM DEPARTMENTS D INNER JOIN EMPLOYEES E
                           ON D.DEPARTMENT_ID = E.DEPARTMENT_ID
                   INNER JOIN (SELECT MAX(AVG(E.SALARY)) AS MAX_AVG, 
																			MIN(AVG(E.SALARY)) AS MIN_AVG
                               FROM EMPLOYEES E INNER JOIN DEPARTMENTS D
                                              ON E.DEPARTMENT_ID = D.DEPARTMENT_ID
                                           AND D.DEPARTMENT_NAME IN ('IT', 'Sales')
                               GROUP BY E.DEPARTMENT_ID, D.DEPARTMENT_NAME) A
                           ON 1 = 1
GROUP BY D.DEPARTMENT_ID, D.DEPARTMENT_NAME, A.MIN_AVG, A.MAX_AVG
HAVING AVG(E.SALARY) > A.MIN_AVG
AND AVG(E.SALARY) < A.MAX_AVG
;
```