# DB쿼리 공부

1. 마리아DB사용
    - 준비작업
        - 접속 명령어(윈도우 기준)

            ```sql
            C:\Program Files\MariaDB 10.3\bin\mysql.exe" -uroot -p
            ```

        ```sql
        -- DataBase 생성
        CREATE DATABASE SCOTT; 

        -- USER생성 
        create user strong1133 @localhost identified by '1234';

        -- STRONG1133 권한 부여
        GRANT CREATE, ALTER, SELECT, INSERT, UPDATE, DELETE ON SCOTT.* TO 'strong1133'@'localhost';

        -- STRONG1133 권한 조회
        SHOW GRANTS FOR 'strong1133'@'localhost';

        -- 권한 적용
        FLUSH PRIVILEGES;

        -- STRONG1133 접속
        "C:\Program Files\MariaDB 10.3\bin\mysql.exe" -u strong1133 -p

        -- 'SCOTT' DATABASE로 변경
        USE SCOTT;
        ```

2. 오라클 사용
    - 준비작업

        ```sql
        -- 루트 권한 접속
        SQLPULS
        SYSTEM
        PWD

        -- 오라클 내부의 예제DB 잠금 해제
        @C:\oraclexe\app\oracle\product\11.2.0\server\rdbms\admin\scott.sql

        -- 예제DB 잠금해제 확인 및 SCOTT 비밀번호 변경
        show user
        alter user scott identified by tiger;

        -- 루트 로그아웃 후 SCOTT으로 재접속
        EXIT
        SQLPLUS SCOTT/TIGER
        ```

## 연습용 scott/tiger db생성

---

- Table 생성

    ```sql
    CREATE TABLE EMP(
       EMPNO      INTEGER NOT NULL,
       ENAME       VARCHAR(10),
       JOB        VARCHAR(9),
       MGR        INTEGER,
       HIREDATE   VARCHAR(12),
       SAL        DECIMAL(7, 2),
       COMM       DECIMAL(7, 2),
       DEPTNO     INTEGER
    );
     
    CREATE TABLE DEPT(
       DEPTNO     INTEGER NOT NULL,
       DNAME      VARCHAR(14),
       LOCATION   VARCHAR(13)
    );

    CREATE TABLE SALGRADE(
       GRADE      INTEGER NOT NULL,
       LOSAL      INTEGER NOT NULL,
       HISAL      INTEGER NOT NULL
    );

    CREATE TABLE BONUS (
       ENAME      VARCHAR(10) NOT NULL,
       JOB        VARCHAR(9) NOT NULL,
       SAL        DECIMAL(7, 2),
       COMM       DECIMAL(7, 2)
    );
    ```

- PK 생성

    ```sql
    ALTER TABLE EMP 
       ADD CONSTRAINT EMP_PK
       PRIMARY KEY (EMPNO);

    ALTER TABLE DEPT
       ADD CONSTRAINT DEPT_PK
       PRIMARY KEY (DEPTNO);

    ALTER TABLE SALGRADE
       ADD CONSTRAINT SALGRADE_PK
       PRIMARY KEY (GRADE);

    ALTER TABLE BONUS
       ADD CONSTRAINT BONUS_PK
       PRIMARY KEY (ENAME, JOB);

    ALTER TABLE EMP
       ADD CONSTRAINT DEPT
       FOREIGN KEY (DEPTNO)
       REFERENCES DEPT (DEPTNO);

    ALTER TABLE EMP
       ADD CONSTRAINT MGR
       FOREIGN KEY (MGR)
       REFERENCES EMP (EMPNO);
    ```

- Data 추가

    ```sql
    INSERT INTO DEPT VALUES
      (10, 'ACCOUNTING', 'NEW YORK'),
      (20, 'RESEARCH',   'DALLAS'),
      (30, 'SALES',      'CHICAGO'),
      (40, 'OPERATIONS', 'BOSTON');
     
    INSERT INTO EMP VALUES
      (7839, 'KING',   'PRESIDENT', NULL, '1981-11-17', 5000, NULL, 10),
      (7566, 'JONES',  'MANAGER',   7839, '1981-04-02', 2975, NULL, 20),
      (7788, 'SCOTT',  'ANALYST',   7566, '1982-12-09', 3000, NULL, 20),
      (7876, 'ADAMS',  'CLERK',     7788, '1983-01-12', 1100, NULL, 20),
      (7902, 'FORD',   'ANALYST',   7566, '1981-12-03', 3000, NULL, 20),
      (7369, 'SMITH',  'CLERK',     7902, '1980-12-17',  800, NULL, 20),
      (7698, 'BLAKE',  'MANAGER',   7839, '1981-05-01', 2850, NULL, 30),
      (7499, 'ALLEN',  'SALESMAN',  7698, '1981-02-20', 1600,  300, 30),
      (7521, 'WARD',   'SALESMAN',  7698, '1981-02-22', 1250,  500, 30),
      (7654, 'MARTIN', 'SALESMAN',  7698, '1981-09-28', 1250, 1400, 30),
      (7844, 'TURNER', 'SALESMAN',  7698, '1981-09-08', 1500,    0, 30),
      (7900, 'JAMES',  'CLERK',     7698, '1981-12-03',  950, NULL, 30),
      (7782, 'CLARK',  'MANAGER',   7839, '1981-06-09', 2450, NULL, 10),
      (7934, 'MILLER', 'CLERK',     7782, '1982-01-23', 1300, NULL, 10);
     
    INSERT INTO SALGRADE VALUES
      (1,  700, 1200),
      (2, 1201, 1400),
      (3, 1401, 2000),
      (4, 2001, 3000),
      (5, 3001, 9999);
      
    COMMIT;
    ```

1. 테이블 조작 예시

    ```sql
    -- 테이블 목록 조회
    SHOW TABLES;

    -- EMP테이블에서 직업이 MANAGER 인 정보 조회
    SELECT * FROM EMP WHERE JOB ='MANAGER';
    ```

    - 과제 01일차
        - 1번
            - 사원 테이블에서 사원번호가 7369, 7698 번인 사원번호와 이름을
            출력하세요.
                - IMG

                    <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled.png?raw=true" />

                - 쿼리

                    ```sql
                    SELECT EMPNO, ENAME FROM EMP
                    WHERE EMPNO = 7369 OR EMPNO = 7698;
                    ```

                    Comment
                    EMP로 부터 EMPNO와 ENAME을 불러올 것인데
                    단, EMPNO가 7369 또는 7698 인 것들만 불러올 것이다.
                    WHERE EMPNO IN(7369,7698); -> 이렇게도 가능하다.

        - 2번
            - 사원 테이블에서 사원번호가 7369, 7698 번인 아닌 사원번호와 이름을 출력하세요.
                - IMG

                    <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%201.png?raw=true" />

                - 쿼리

                    ```sql
                    -- NOT 해결법
                    SELECT EMPNO, ENAME FROM EMP
                    WHERE NOT (EMPNO=7369 OR EMPNO=7698);

                    -- NOT IN
                    SELECT EMPNO, ENAME
                    FROM EMP 
                    WHERE EMPNO NOT IN (7369, 7698);
                    ```

                    Comment
                    EMP로 부터 EMPNO와 ENAME을 불러올 것인데
                    단, EMPNO가 7369 또는 7698 가 아닌 것들만 불러올 것이다.

        - 3번
            - 사원 테이블에서 급여(SAL)가 3000에서 5000사이인 사원 정보를 다 출력하세요.
                - IMG

                    <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%202.png?raw=true" />

                - 쿼리

                    ```sql
                    -- BETWEEN
                    SELECT * FROM EMP
                    WHERE SAL BETWEEN 3000 AND 5000;

                    -- >= AND <=
                    SELECT * FROM EMP
                    WHERE SAL >= 3000 AND SAL <= 5000;
                    ```

                    Comment
                    '*' 는 모든 정보를 포함하겠다는 의미
                    EMP로 부터 모든 정보를 가져올 것인데  3000~5000 사이만 가져온다.
                    ≥ AND ≤ 를 써줘도 되고 BETWEEN A AND B 를 써줘도 된다.

        - 4번
            - 사원 테이블에서 고용일자(HIREDATE)가 1981년 12월 1일 이후 고용된 사원 정보를 다 출력하세요.
                - IMG

                    <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%203.png?raw=true" />

                - 쿼리

                    ```sql
                    SELECT *
                    FROM EMP e
                    WHERE TO_NUMBER(TO_CHAR(e.HIREDATE, 'YYYYMMDD')) >= 19811201;
                    ```

                    Comment
                    EMP로 부터 모든 정보를 불러 온다.
                    단 HIREDATE가 1911 12 01 의 이후 날짜만
                    여기서 1911 12 01 이 YYYYMMDD형태이기 때문에 HIREDATE역시 해당 형태로 맞춰줘야 비교가 가능해진다. 이를 위해  TO_NUMBER(TO_CHAR(e.HIREDATE, 'YYYYMMDD')) 로 형태를 맞춰준다.

        - 5번
            - 사원 테이블에서 직업(JOB)이 SALESMAN 중에서 사원번호의 최대값을 출력하세요.
                - IMG

                    <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%204.png?raw=true" />

                - 쿼리

                    ```sql
                    SELECT MAX(EMPNO) AS EMPNO
                    FROM EMP
                    WHERE JOB = 'SALESMAN';
                    ```

                    Comment
                    EMP로 부터 최댓값을 가져올것이다.
                    해서 SELECT문에 MAX(EMPNO)를 주는 것 
                    AS를 통해 EMPNO로 명명해준다. 

    - 과제 02일차
        - 1번
            - 사원 테이블에서 각 사원에 부서명을 아래 예제처럼 출력하세요.
            (사원, 부서 테이블 조인 시 부서가 없는 사원은 출력 안함)
            ex) 정렬은 부서명(DNAME), 사원명(ENAME) 오름차순
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%205.png?raw=true" />

            - 쿼리

                ```sql
                -- DEPTNO 로 조인 하여 조회
                SELECT D.DNAME, E.EMPNO, E.ENAME
                FROM EMP E, DEPT D
                WHERE E.DEPTNO = D.DEPTNO AND D.DEPTNO IS NOT NULL
                ORDER BY D.DNAME ASC, E.ENAME ASC;
                ```

                Comment
                사원 정보는 EMP에 부서 정보는 DEPT 테이블에 있다.
                두 개의 테이블을 다루기 위해 FK로 조인을 해야 하고 이 경우 FK는 DEPTNO
                두개의 테이블로 부터 값이 오기때문에 D, E 와 같이 출처를 명시해 주는 것이 좋다. WHERE E.DEPTNO = D.DEPTNO AND D.DEPTNO IS NOT NULL 를 통해 각 FK로 연결된 값들을 조인 시켜 불러 올 수 있고 매칭이 안되는 값들은 버릴 수 있게 된다

        - 2번 (LEFT JOIN 복습 필요!)
            - 사원 테이블에서 각 사원에 부서명을 아래 예제처럼 출력하세요.
            (사원, 부서 테이블 조인 시 부서가 없는 사원도 출력)
            ex) 정렬은 부서명(DNAME), 사원명(ENAME) 오름차순
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%206.png?raw=true" />

            - 쿼리

                ```sql
                -- LEFT OUTER JOIN 
                SELECT D.DNAME, E.EMPNO, E.ENAME
                FROM EMP E  LEFT JOIN  DEPT D ON E.DEPTNO = D.DEPTNO
                ORDER BY D.DNAME ASC, E.ENAME ASC;

                SELECT D.DNAME, E.EMPNO, E.ENAME
                FROM  DEPT D  LEFT JOIN  EMP E  ON E.DEPTNO = D.DEPTNO
                ORDER BY D.DNAME ASC, E.ENAME ASC;

                -- 두개의 차이점은 
                EMP E  LEFT JOIN  DEPT D -- EMP를 기준으로 즉 DEPT에 없어도 EMP에 있음 조회가 되고
                DEPT D  LEFT JOIN  EMP E -- DEPT를 기준으로 즉 EMP 에 없어도 DEPT에 있음 조회

                ```

                Comment
                1번 문제와 같이 JOIN을 통해 해결 해야 하는데
                EMP에는 있지만 DEPT에 없어도 불러오긴 해야 함으로 LEFT JOIN을 시켜줘야 한다.

                두개의 차이점은 
                EMP E  LEFT JOIN  DEPT D -- EMP를 기준으로 즉 DEPT에 없어도 EMP에 있음 조회가 되고
                DEPT D  LEFT JOIN  EMP E -- DEPT를 기준으로 즉 EMP 에 없어도 DEPT에 있음 조회된다.

        - 3번
            - 부서 위치가 'DALLAS', 'CHICAGO' 곳에 근무하는 사원 정보 아래
            예제처럼 출력하세요.
            ex) 정렬은 부서위치(LOC) 내림차순, 사원명(ENAME) 오름차순
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%207.png?raw=true" />

            - 쿼리

                ```sql
                SELECT D.LOCATION, E.EMPNO, E.ENAME
                FROM EMP E, DEPT D
                WHERE E.DEPTNO = D.DEPTNO AND (D.LOCATION='DALLAS' OR D.LOCATION= 'CHICAGO')
                ORDER BY D.LOCATION DESC, E.ENAME ASC;
                ```

                Comment
                마찬가지로 조인을 해야하는데 부서 위치가 'DALLAS', 'CHICAGO' 곳에 근무하는 직원만 불러와야 한다. 해서 FK가 일치하면서 그리고 근무 위치가 일치하는 정보를 가져오면 된다.

        - 4번(GROUP BY 복습 필요!)
            - 부서 별 최고 급여(SAL) 금액을 아래 예제처럼 출력하세요?
            ex) 부서 없는 사원은 제외
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%208.png?raw=true" />
            - 쿼리

                ```sql
                SELECT E.DEPTNO, MAX(SAL) AS SAL
                FROM EMP E
                WHERE E.DEPTNO IS NOT NULL
                GROUP BY E.DEPTNO
                ORDER BY E.DEPTNO ASC;

                -- DEPTNO를 중복없이 DEPTNO마다 최고 급여를 합산하기 위하여
                -- GROUP BY로 DEPTNO를 묶어 준다.
                ```

                Comment
                DEPTNO가 있지만 DEPTNO와 SAL모두 EMP 안에 있기 때문에 조인은 필요 없다
                또한 DEPTNO를 중복없이 DEPTNO마다 최고 급여를 합산하기 위하여
                GROUP BY로 DEPTNO를 묶어 준다.

        - 5번 (서브쿼리 복습!)
            - 부서별 최고 급여(SAL) 금액을 받는 사원 정보를 아래 예제처럼 출력하세요?
             ex) 부서 없는 사원은 제외
            - IMG

               <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%209.png?raw=true" />

            - 쿼리

                ```sql
                SELECT E.DEPTNO, E.SAL, E.EMPNO, E.ENAME, E.JOB
                FROM EMP E
                WHERE (E.SAL, E.DEPTNO) IN (
                	SELECT MAX(EMP.SAL) , EMP.DEPTNO
                	FROM EMP
                	WHERE DEPTNO IS NOT NULL
                	GROUP BY EMP.DEPTNO
                )
                ORDER BY E.DEPTNO ASC;
                ```

                Comment
                최종 적으로 가져와야 하는 컬럼들이 MAX(SAL) 이외에도 많기 때문에 GROUP에 어려움이 있다. 해서 서브 쿼리를 통해 DEPTNO로 그룹화된 MAX(SAL) 을 따로 구해주고 이것을 상위쿼리에서 조건문으로 연결하여 포함된 것들 이라 해서 ( ) IN ( ) 의 형태로 가져와준다.

    - 과제 03일차
        - 1번
            - 사원 테이블에서 각 사원에 급여(SAL) 등급을 아래 예제처럼 출력하세요?
            (급여순위점수(SALGRADE) 테이블 조인)
            ex) 정렬은 등급(GRADE) 오름차순
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2010.png?raw=true" />

            - 쿼리

                ```sql
                SELECT EMPNO, ENAME, SAL, GRADE
                FROM EMP E, SALGRADE S
                WHERE E.SAL <= S.HISAL AND E.SAL >= S.LOSAL
                ORDER BY GRADE ASC;
                ```

                Comment
                SALGRADE안에는 LOSAL ~ HISAL 에 따른 GRADE 정보만 있기 때문에
                비교연산을 통해 구해주면 끝.

        - 2번
            - 사원 테이블에서 평균 급여(SAL) 보다 높은 사원 정보를 아래 예제처럼 출력하세요?
            ex) 정렬은 급여(SAL) 내림차순
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2011.png?raw=true" />

            - 쿼리

                ```sql
                SELECT EMPNO, ENAME, JOB, SAL
                FROM EMP E
                WHERE E.SAL > (
                	SELECT AVG(E.SAL) 
                	FROM EMP E
                )
                ORDER BY SAL DESC;
                ```

                Comment
                그룹화 문제 최종적으로는 다른 컬럼들도 조회 해야 하기 때문에 조건문안에 서브쿼리로 그룹화를 해주면된다.

        - 3번
            - 사원 테이블에서 부서 별 평균 급여(SAL) 보다 높은 사원 정보를 아래 예제처럼 출력하세요?
            ex) 정렬은 급여(SAL) 내림차순
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2012.png?raw=true" />

            - 쿼리

                ```sql
                SELECT DNAME, EMPNO, ENAME, JOB, SAL
                FROM EMP E, DEPT D, (
                	SELECT DEPTNO, AVG(EMP.SAL) AS AVGSAL
                	FROM EMP 
                	GROUP BY EMP.DEPTNO
                	) AVGTAB
                WHERE E.DEPTNO = D.DEPTNO AND AVGTAB.DEPTNO = E.DEPTNO
                AND E.SAL > AVGTAB.AVGSAL
                ORDER BY SAL DESC;
                ```

                Comment
                그룹화 + 서브쿼리 문제 
                각 DEPTNO 에 해당하는 SAL의 AVG값을 그룹화 하여 하나의 테이블 처럼 활용해야 함으로 FROM 안에 서브 쿼리를 넣어 AVGTAB으로 명명해 활용
                WHERE절에는 FK가 DEPT, EMP, AVGTAB 에서 다 일치 해야 하고 SAL이 AVGTAB에 있는 AVGSAL 보다 커야 한다. 

        - 4번
            - 사원 테이블에서 각 사원에 급여(SAL) 순위 점수 별로 인원수를 아래 예제처럼 출력하세요?
            (급여순위점수(SALGRADE) 테이블 조인)
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2013.png?raw=true" />

            - 쿼리

                ```sql
                SELECT TEMP.GRADE, S.LOSAL, S.HISAL, TEMP.CNT
                FROM (SELECT GRADE, COUNT(*) AS CNT
                FROM SALGRADE S, EMP E
                WHERE E.SAL <= S.HISAL AND E.SAL >= S.LOSAL
                GROUP BY GRADE) TEMP, SALGRADE S
                WHERE S.GRADE = TEMP.GRADE;
                ```

                Comment
                서브쿼리를 먼저 구해준다. E.SAL에 해당하는 S.GRADE를 COUNT(*) 로 카운팅 해줘서 반환
                서브쿼리로 구해진 결과를 TEMP라 명명하고 SALGRADE 와 TEMP는 GRADE라는 FK를 갖게 되됨으로 WHERE절에서 조인시켜준다.

        - 5번
            - 부서명이 'RESEARCH' 이거나 부서위치가 'NEW YORK' 사원 정보를 아래 예제처럼 출력하세요?
            ex) 정렬은 부서명(DNAME) 오름차순
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2014.png?raw=true" />4%80%E1%85%A9%E1%86%BC%E1%84%87%E1%85%AE%200e43c18b7f0043c28093e72eb689d18b/Untitled%2014.png)

            - 쿼리

                ```sql
                SELECT D.DNAME, D.LOCATION, E.EMPNO, E.ENAME
                FROM EMP E, DEPT D
                WHERE (D.DNAME='RESEARCH' OR D.LOCATION ='NEW YORK')
                	AND E.DEPTNO = D.DEPTNO
                ORDER BY D.DNAME, E.EMPNO;
                ```

                Comment
                DEPTNO가 있지만 DEPTNO와 SAL모두 EMP 안에 있기 때문에 조인은 필요 없다
                또한 DEPTNO를 중복없이 DEPTNO마다 최고 급여를 합산하기 위하여
                GROUP BY로 DEPTNO를 묶어 준다.

    - 과제 04일차
        - 1번
            - 사원 테이블에서 각 사원에 급여(SAL)가 높은 순서대로 상위 5명을 아래 예제처럼 출력하세요
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2015.png?raw=true" />

            - 쿼리

                ```sql
                SELECT ER.* 
                FROM (
                	SELECT E.*, ROW_NUMBER() OVER(ORDER BY E.SAL DESC) AS RN
                	FROM EMP E
                )ER
                WHERE ER.RN <= 5;
                ```

                Comment
                상위 5명을 찾아야 하는데 그러기 위해서는 ROW_NUMBER를 구하는 서브쿼리가 필요하다
                서브쿼리로  SAL를 DESC로 정렬한 ROW_NUMBER를 구한 뒤 다시 상위 쿼리에서 
                WHERE 절로ROW_NUMBER가 5보다 작은것들만 찾아주면 된다.

        - 2번
            - 사원 테이블에서 각 사원에 급여(SAL)가 높은 순서대로 순위를 부여 했을 때 6등~10등인 사람을 순위대로 아래 예제처럼 출력하세요
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2016.png?raw=true" />

            - 쿼리

                ```sql
                SELECT ER.* 
                FROM (
                	SELECT E.*, ROW_NUMBER() OVER(ORDER BY E.SAL DESC) AS RN
                	FROM EMP E
                )ER
                WHERE ER.RN >= 6 AND ER.RN <=10;
                ```

                Comment
                1번과 마찬가지로 ROW_NUMBER를 찾아 주는 하위 쿼리를 이용하여 
                 6 ≤ ROW_NUMBER ≤ 10 인 컬럼을 찾아 주면 된다.

        - 3번
            - 아래 SQL 실행 했을 경우 0건 출력 되는 이유를 설명하세요?
            ex) SELECT ROWNUM, A.*
                  FROM   EMP A
                  WHERE  ROWNUM > 5
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%207.png?raw=true" />

            - 쿼리

                ```sql
                SELECT D.LOCATION, E.EMPNO, E.ENAME
                FROM EMP E, DEPT D
                WHERE E.DEPTNO = D.DEPTNO AND (D.LOCATION='DALLAS' OR D.LOCATION= 'CHICAGO')
                ORDER BY D.LOCATION DESC, E.ENAME ASC;
                ```

                Comment
                ROW_NUMBER는 직접 비교 연산을 수행 할 수 없다.
                해서 서브쿼리와 AS를 통해 컬럼 값을 활용해야 비교연산이 가능해진다.

        - 4번(CASE에 대한 다양한 예제 찾아볼 것)
            - SALGRADE 테이블 데이터 세로 정보를 가로로 아래 예제처럼 출력하세요.
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2017.png?raw=true" />

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2018.png?raw=true" />

            - 쿼리

                ```sql
                SELECT
                	MAX (CASE S.GRADE WHEN 1 THEN S.LOSAL||'~'||S.HISAL END) GRADE_1,
                	MAX (CASE S.GRADE WHEN 2 THEN S.LOSAL||'~'||S.HISAL END) GRADE_2,
                	MAX (CASE S.GRADE WHEN 3 THEN S.LOSAL||'~'||S.HISAL END) GRADE_3,
                	MAX (CASE S.GRADE WHEN 4 THEN S.LOSAL||'~'||S.HISAL END) GRADE_4,
                	MAX (CASE S.GRADE WHEN 5 THEN S.LOSAL||'~'||S.HISAL END) GRADE_5
                	FROM SALGRADE S;
                ```

                ```sql
                SELECT S1.GRADE_1, S2.GRADE_2, S3.GRADE_3, S4.GRADE_4, S5.GRADE_5
                FROM (
                	SELECT 
                	CASE S.GRADE WHEN 1 THEN S.LOSAL||'~'||S.HISAL END
                	GRADE_1
                	FROM SALGRADE S
                ) S1,
                 (
                	SELECT 
                	CASE S.GRADE WHEN 2 THEN S.LOSAL||'~'||S.HISAL END
                	GRADE_2
                	FROM SALGRADE S
                ) S2,
                 (
                	SELECT 
                	CASE S.GRADE WHEN 3 THEN S.LOSAL||'~'||S.HISAL END
                	GRADE_3
                	FROM SALGRADE S
                ) S3,
                 (
                	SELECT 
                	CASE S.GRADE WHEN 4 THEN S.LOSAL||'~'||S.HISAL END
                	GRADE_4
                	FROM SALGRADE S
                ) S4,
                 (
                	SELECT 
                	CASE S.GRADE WHEN 5 THEN S.LOSAL||'~'||S.HISAL END
                	GRADE_5
                	FROM SALGRADE S
                ) S5
                WHERE S1.GRADE_1 IS NOT NULL AND
                 S2.GRADE_2 IS NOT NULL AND
                 S3.GRADE_3 IS NOT NULL AND
                 S4.GRADE_4 IS NOT NULL AND
                 S5.GRADE_5 IS NOT NULL;
                ```

                Comment
                CASE를 이용해 크로스 조인을 시킨 후 NULL값을 제외시켜주면 된다.
                집계함수를 쓰면 NULL값을 수동으로 제외 시켜줄 필요 없다.

                MAX (CASE S.GRADE WHEN 1 THEN S.LOSAL||'~'||S.HISAL END) GRADE_1,

                S.GRADE가 1이면 S.LOSAL ~ S.HISAL 형태로 보여 주고 이름을 GRADE_1로 하겠다는 의미

        - 5번(DECODE 알아 볼 것!)
            - 부서별 최고 급여(SAL) 금액을 받는 사원 정보를 아래 예제처럼 출력하세요?
             ex) 부서 없는 사원은 제외
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2019.png?raw=true" />

            - 쿼리

                ```sql
                SELECT SG.GRADE, DECODE(SG.GRADE, 1, s.GRADE_1, 2, s.GRADE_2, 3, s.GRADE_3, 4, s.GRADE_4, 5, s.GRADE_5) AS LOSAL_HISAL 
                FROM   (
                SELECT 
                MAX(CASE S.GRADE WHEN 1 THEN S.LOSAL||'~'||S.HISAL END) GRADE_1,
                MAX(CASE S.GRADE WHEN 2 THEN S.LOSAL||'~'||S.HISAL END) GRADE_2,
                MAX(CASE S.GRADE WHEN 3 THEN S.LOSAL||'~'||S.HISAL END) GRADE_3,
                MAX(CASE S.GRADE WHEN 4 THEN S.LOSAL||'~'||S.HISAL END) GRADE_4,
                MAX(CASE S.GRADE WHEN 5 THEN S.LOSAL||'~'||S.HISAL END) GRADE_5
                FROM SALGRADE S
                ) s, SALGRADE SG;
                ```

                Comment
                CASE를 이용해 크로스 조인한 테이블들을 DEOCODE를 통해 세로로 바꿔준다. 

    - 과제 05일차
        - 1번
            - 사원 테이블에서 EMPNO, MGR, SAL 세 개의 컬럼을 단순 숫자 의미로 가정 할 경우 세 개의 값 중 최대값(MAX_VALUE), 최소값(MIN_VALUE) 을 아래 예제처럼 출력하세요.
            ex) EMPNO, MGR, SAL 컬럼 중 널 값이 존재할 경우 0으로 치환 정렬은 최대값 내림차순
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2020.png?raw=true" />

            - 쿼리

                ```sql
                SELECT UE.EMPNO, UE.MGR, UE.SAL, UE.MAX_VALUE, UE.MIN_VALUE
                FROM(
                	SELECT E.EMPNO, E.MGR, E.SAL, E.EMPNO MAX_VALUE, NVL(E.SAL, 0) MIN_VALUE
                	FROM EMP E
                	WHERE E.EMPNO > E.MGR AND E.MGR > NVL(E.SAL, 0)
                	UNION
                	SELECT E.EMPNO, E.MGR, E.SAL, E.EMPNO MAX_VALUE, NVL(E.MGR,0) MIN_VALUE
                	FROM EMP E
                	WHERE E.EMPNO >E.SAL AND NVL(E.MGR, 0) <E.SAL
                	UNION
                	SELECT E.EMPNO, E.MGR, E.SAL, E.MGR MAX_VALUE, NVL(E.EMPNO, 0) MIN_VALUE
                	FROM EMP E
                	WHERE E.MGR > E.SAL AND NVL(E.EMPNO, 0 )< E.SAL
                	UNION
                	SELECT E.EMPNO, E.MGR, E.SAL, E.MGR MAX_VALUE, NVL(E.SAL, 0) MIN_VALUE
                	FROM EMP E
                	WHERE E.MGR > E.EMPNO AND NVL(E.SAL, 0) < E.EMPNO
                	UNION
                	SELECT E.EMPNO, E.MGR, E.SAL, E.SAL MAX_VALUE,  NVL(E.EMPNO, 0) MIN_VALUE
                	FROM EMP E
                	WHERE E.SAL > E.MGR AND NVL(E.EMPNO, 0 )< E.MGR
                	UNION
                	SELECT E.EMPNO, E.MGR, E.SAL, E.SAL MAX_VALUE,  NVL(E.MGR,0) MIN_VALUE
                	FROM EMP E
                	WHERE E.SAL > E.EMPNO AND NVL(E.MGR, 0 )< E.EMPNO
                ) UE
                ORDER BY MAX_VALUE DESC, SAL DESC;
                ```

        - 2번
            - 부서 테이블에서 DNAME 컬럼의 길이가 6자리 이상이면 5자리까지 데이터만 보여주고 ‘..’ 뒤에 붙여준다. 아래 예제처럼 출력하세요.
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2021.png?raw=true" />

            - 쿼리

                ```sql
                SELECT DNAME, NVL(DT.DATA, DNAME)
                FROM DEPT D, (
                	SELECT SUBSTR(DNAME, 0,5)||'..' DATA, D.DEPTNO
                	FROM DEPT D
                	WHERE LENGTH(DNAME) >= 6
                ) DT
                WHERE DT.DEPTNO(+) = D.DEPTNO;
                -- (+) 를 해줌으로써 서브쿼리에서 반환되지 않은 NULL을 가진 컬럼을 추가 해주게된다.
                -- 이 경우에는 SALES
                ```

                Comment
                SELECT DNAME, NVL(DT.DATA, DNAME)의 의미는 서브쿼리에서 가져올 DT.DATA가 NULL 이면 DNAME을 넣어 주겠다는 의미 이며 사진의 3행 SALES의 DATA에 DNAME이 들어온것을 볼수 있다.

                서브쿼리를 통해 DNAME이 0~5자 인 값에 .. 를 붙여주고 DEPTNO와 함께 반환
                WHERE 절을 통해 같은 DEPTNO끼리 조인 해주고 (+)를 붙여 매칭이 서브쿼리에서 반환되지 않은 NULL을 가진 컬럼을 추가 해주게 된다.
                이 경우에는 SALES

        - 3번
            - 사원 테이블에서 고용일자(HIREDATE) 해당 년 월의 마지막 날짜(HIREDATE_MONTH_LASTDAY), 
            고용일자부터 2006/08/05 일자까지 근무일(WORK_DAY)을 아래 예제처럼 출력하세요.
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2022.png?raw=true" />

            - 쿼리

                ```sql
                SELECT EMPNO, HIREDATE, 
                LAST_DAY(TO_DATE(HIREDATE)) AS HIREDATE_MONTH_LASTDAY, 
                TO_DATE('2006-08-05')-TO_DATE(HIREDATE) AS WORK_DAY
                FROM EMP;
                ```

                Comment
                SELECT를 통해 가져올 값들을 먼저 파악해야 한다
                EMPNO, HIREDATE와 HIREDATE 에 해당하는 년월의 마지막 DAY와 고용일자 부터 2006-08-05까지의 근무일을 계산해서 반환해야 한다

                년월의 마지막 날짜는 LAST_DAY(TO_DATE(HIREDATE)) 를 통해 구할 수 있고
                근무일은 TO_DATE() - TO_DATE를 통해 형식을 맞게 해서 - 연산 하면 구할 수 있다.

        - 4번
            - 3번 예제에서 구한 각 사원에 근무일(WORK_DAY)이 가장 높은 사람을 한 명을 아래 예제처럼 출력하세요.
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2023.png?raw=true" />

            - 쿼리

                ```sql
                SELECT * FROM
                (
                	SELECT ENAME, EMPNO, HIREDATE, ROWNUM,
                	MAX(TO_DATE('2006-08-05')-TO_DATE(HIREDATE))
                	OVER(ORDER BY HIREDATE ASC) AS  WORKDAY
                	FROM EMP
                )
                WHERE ROWNUM=1;
                ```

                Comment
                서브쿼리를 실행하면 TO_DATE('2006-08-05')-TO_DATE(HIREDATE)의 MAX값이 구해진 상태로 값들이 HIREDATE를 기준으로 ASC정렬되어 반환 된다. 이때 ROWNUM역시 같이 반환이 되어
                상위 쿼리에서 ROWNUM =1을 조건으로 하면 근무일이 가장 높은 사람을 구할 수 있다. 

        - 5번
            - 3번 예제에서 구한 각 사원에 근무일(WORK_DAY)이 가장 높은 사람과 낮은 사람을 각각 한 명씩 아래 예제처럼 출력하세요.
            - IMG

                <img src="https://github.com/strong1133/SQL_Study/blob/main/dlab_test/DB%EC%BF%BC%EB%A6%AC%20%EA%B3%B5%EB%B6%80/Untitled%2024.png?raw=true" />

            - 쿼리

                ```sql
                SELECT * FROM
                (
                	SELECT * FROM
                	(
                		SELECT ENAME, EMPNO, HIREDATE, ROWNUM,
                		MIN(TO_DATE('2006-08-05')-TO_DATE(HIREDATE))
                		OVER(ORDER BY HIREDATE DESC) AS WORKDAY
                		FROM EMP 
                	)
                	WHERE ROWNUM =1
                	UNION
                	SELECT * FROM
                	(
                		SELECT ENAME, EMPNO, HIREDATE, ROWNUM,
                		MAX(TO_DATE('2006-08-05')-TO_DATE(HIREDATE))
                		OVER(ORDER BY HIREDATE ASC)AS WORKDAY
                		FROM EMP 
                	)
                	WHERE ROWNUM=1
                	) MINMAXTAB
                ORDER BY MINMAXTAB.WORKDAY DESC;
                ```

                Comment
                4번에서 구한 최곳값 쿼리에서 정렬을 내림차순으로 바꾸면 최솟값을 구할 수 있다.
                이 두개의 쿼리를 UNION 한 결과를 최상위에 SELECT * FROM() 으로 감싸주면 완료.

    .