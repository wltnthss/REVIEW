# DoIt! 오라클로 배우는 데이터 베이스 입문 - 이지훈

## 1장 SQL 데이터베이스 개념 잡기

### 데이터베이스란?

* 데이터베이스 : 효율적인 관리와 검색을 위해 구조화한 데이터 집합
* 데이터베이스 활용 이유 : 여러 응용 프로그램이 사용할 데이터를 한곳에서 관리하기 위해 테이터 베이스를 활용합니다.
* 데이터 베이스 장점 : 데이터 중복 제거, 데이터 동시 공유

### 관계형 데이터베이스와 SQL

* 관계형 데이터 모델 : 각 데이터의 독립 특성만을 규정하여 데이터를 묶어 relational 을 앞에 붙여 RDBMS 라고 부르는 것을 관계형 데이터베이스 관리 시스템이라고 부릅니다.
* 관계형 데이터 모델의 핵심 구성 요소는 개체, 속성, 관계로 이루어집니다.

### 관계형 데이터베이스의 구성요소

**후보키, 보조키, 기본키**

* 유일한 데이터를 가지고 있고 빈 값이 없는 열은 기본키가 될 수 있는 후보키.
* 후보키 중에서 기본키로 선택한 데이터를 제외한 나머지가 보조키.
* 한 테이블 내에서 중복되지 않는 값만 가질 수 있는 기본키.

**외래키**

* 특정 테이블에 포함되어 있으면서 다른 테이블의 기본키로 지정된 키.
* 데이터의 중복을 피하기 위해 테이블 사이의 관계를 규명하기 위한 필수 요소.

**객체**

* 오라클 데이터베이스에서는 데이터를 저장하고 관리하기 위한 다양한 논리 구조를 가진 구성 요소들이 존재합니다.

1. 테이블 : 데이터 저장 장소
2. 인덱스 : 테이블 검색 효율 높이기 위해 사용
3. 뷰 : 여러 개의 선별된 데이터를 연결하여 하나의 테이블처럼 사용가능하게 해줌.
4. 시퀀스 : 일련번호 생성
5. 시노님 : 오라클 객체의 별칭을 지정함
6. 프로시저 : 프로그래밍 연산 및 기능 수행이 가능함(반환값 X)
7. 함수 : 프로그래밍 연산 및 기능 수행이 가능함(반환값 O)
8. 패키지 : 관련 있는 프로시저와 함수를 보관함
9. 트리거 : 데이터 관련 작업의 연결 및 방지 관련 기능을 제공함

## 2장 실무에서 가장 많이 사용하는 SQL, 조회

### 오라클 함수

```SQL
-- 대문자, 소문자, 첫글자 대문자 나머지 소문자 (UPPER, LOWER, INITCAP)
SELECT ENAME, UPPER(ENAME), LOWER(ENAME), INITCAP(ENAME) FROM EMP;

-- 문자열 길이 (LENGTH)
SELECT ENAME, LENGTH(ENAME) FROM EMP;

-- 문자열 일부 추출 (SUBSTR)
SELECT SUBSTR(JOB, 1, 2), SUBSTR(ENAME, 1, 4), SUBSTR(ENAME, 2)  FROM EMP;

-- 문자열 특정 문자 위치 검색 (INSTR)
SELECT INSTR('SMITH', 'I') FROM EMP WHERE SAL = 800

-- 특정 문자 다른 문자로 변환 (REPLACE)
SELECT REPLACE('010-1111-2222', '-', ''), REPLACE('010-1111-2222', '-') FROM DUAL;

-- 데이터의 빈 공간을 특정 문자로 채우기 (LPAD, RPAD)
SELECT lpad('oracle', 10, '*'), lpad('oracle', 10) FROM dual;
SELECT rpad('951214-', '14', '*') FROM dual;

-- 특정 문자를 지우기 (TRIM, LTRIM, RTRIM)
SELECT '[' || TRIM('_' FROM '_ _ ORACLE_ _') || ']' -- BOTH 와 동일
		,'[' || TRIM(LEADING '_' FROM '_ _ ORACLE _ _') || ']' 
		,'[' || TRIM(TRAILING '_' FROM '_ _ ORACLE _ _') || ']'
		,'[' || TRIM(BOTH '_' FROM '_ _ ORACLE_ _') || ']' 
FROM DUAL;

SELECT '[' || LTRIM(' _ ORACLE _ ') || ']' 
		, '[' || LTRIM('<_ORACLE_>', '_<') || ']' 
		, '[' || RTRIM(' _ ORACLE _ ') || ']'
		, '[' || RTRIM('<%ORACLE%>', '>%') || ']'
FROM DUAL;

-- 특정 위치 반올림 (ROUND)
SELECT ROUND(1234.5678), ROUND(1234.5678, 1), ROUND(1234.5678, -1) FROM DUAL;

-- 특정 위치 버리기 (TRUNC)
SELECT TRUNC(1234.5678), TRUNC(1234.5678, 1), TRUNC(1234.5678, -1) FROM DUAL;

-- 지정한 숫자 가까운 정수 찾기 (CEIL, FLOOR)
SELECT CEIL(3.14), FLOOR(3.14) FROM DUAL;

-- 숫자 나눈 나머지 값 구하기 (MOD)
SELECT MOD(10,2), MOD(11,3) FROM DUAL;

-- 날짜 함수 (SYSDATE)
SELECT SYSDATE, SYSDATE-1, SYSDATE+1 FROM DUAL;

-- 몇 개월 이후 날짜 구하기 (ADD_MONTHS)
SELECT ADD_MONTHS(SYSDATE, 3) FROM DUAL; 
SELECT ENAME, JOB, HIREDATE, ADD_MONTHS(HIREDATE, 120) "10주년" FROM EMP;

-- 두 날짜 간 개월 수 차이 구하기 (MONTHS_BETWEEN)
SELECT ENAME, JOB, HIREDATE, MONTHS_BETWEEN(SYSDATE, HIREDATE), TRUNC(MONTHS_BETWEEN(SYSDATE, HIREDATE)) || '일' FROM EMP; 

-- 날짜, 숫자 데이터 문자 변환 함수 (TO_CHAR)
-- YYYY, YY, MM, MON, MONTH, DD, DY, DAY, HH24, MI, SS
SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS') FROM DUAL;

-- 문자 데이터 숫자 데이터 변환 함수 (TO_NUMBER)
SELECT 1300 - '1500', '1300' + 1500 FROM DUAL;
SELECT TO_NUMBER('1,300', '999,999') - TO_NUMBER('1,500', '999,999') FROM DUAL;

-- 문자 데이터 날짜 변환 (TO_DATE)
SELECT TO_DATE('2024-04-14', 'YYYY-MM-DD'), TO_CHAR(TO_DATE('20240414', 'YYYYMMDD'), 'MM') FROM DUAL;
SELECT TO_DATE('2024-04-14', 'YYYY-MM-DD'), TO_DATE('20240414', 'YYYYMMDD') FROM DUAL;
SELECT * FROM EMP WHERE HIREDATE > TO_DATE('1981-06-01', 'YYYY-MM-DD');

-- NULL 처리 함수 (NVL, NVL2)
SELECT EMP.*, NVL(COMM, 0) FROM EMP;
SELECT ENAME, COMM, NVL2(COMM, 'O', 'X') FROM EMP;  -- (열, NULL이 아닌 경우, NULL인 경우)

-- DECODE, CASE
SELECT EMP.*, DECODE(JOB, 'MANAGER', SAL * 1.1, 'SALESMAN', SAL * 1.05, SAL * 1.03) FROM EMP;

SELECT 
    EMP.*, 
    CASE WHEN JOB = 'MANAGER' THEN SAL * 1.1 
         WHEN JOB = 'SALESMAN' THEN SAL * 1.05
        ELSE SAL * 1.03
		END UPSALE FROM EMP;
```

### 다중행 함수와 데이터 그룹화

```SQL
-- 합계 구하기 (SUM) * DEFAULT 는 ALL 
SELECT SUM(SAL), SUM(ALL SAL), SUM(DISTINCT SAL) FROM EMP GROUP BY JOB;

-- 데이터 개수 구하기 (COUNT)
SELECT COUNT(*) FROM EMP;

-- 최댓값과 최솟값 (MAX, MIN)
SELECT MAX(SAL), MIN(SAL) FROM EMP;
SELECT MAX(HIREDATE) FROM EMP WHERE DEPTNO = 20

-- 평균값 (AVG)
SELECT AVG(SAL) FROM EMP;

-- 순위 (RANK, DENSE_RANK)
SELECT ENAME 
     , SAL 
     , RANK() OVER (ORDER BY SAL DESC)       RANK 
     , DENSE_RANK() OVER (ORDER BY SAL DESC) DENSE_RANK 
  FROM EMP 
 ORDER BY SAL DESC

 -- 순위 (세부 순서 정렬 및 그룹별 순위)
SELECT ENAME 
     , SAL 
     , RANK() OVER (ORDER BY SAL DESC)       RANK 
     , DENSE_RANK() OVER (ORDER BY SAL DESC, COMM DESC) DENSE_RANK 
  FROM EMP 
 ORDER BY SAL DESC

SELECT DEPT 
     , ENAME 
     , SAL 
     , COMM 
     , RANK() OVER (PARTITION BY DEPT ORDER BY SAL DESC, COMM DESC) RANK 
  FROM EMP 
 ORDER BY DEPT, SAL DESC, COMM DESC
 
-- 각 부서별 평균 급여
SELECT AVG(SAL), DEPTNO FROM EMP GROUP BY DEPTNO ORDER BY DEPTNO;

-- 부서 직책별 평균 급여 2000 이상
SELECT AVG(SAL), JOB, DEPTNO FROM EMP GROUP BY JOB, DEPTNO HAVING AVG(SAL) >= 2000;

-- 그룹화 합계 출력(ROLLUP)
SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL) FROM EMP GROUP BY ROLLUP(DEPTNO, JOB) ;
-- DEPTNO  선 그룹화 후 ROLLUP 함수에 JOB 지정
SELECT DEPTNO, JOB, COUNT(*) FROM EMP GROUP BY DEPTNO, ROLLUP(JOB);

-- 그룹화 합계 출력(CUBE) * 지정한 모든 열에서 가능한 조합의 결과 모두 출력
SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL) FROM EMP GROUP BY CUBE(DEPTNO, JOB) ORDER BY DEPTNO ,JOB;

-- 같은 수준 그룹화 열 여러 개인 경우 (GROUPING SETS) 함수
SELECT DEPTNO, JOB, COUNT(*) FROM EMP GROUP BY GROUPING SETS(DEPTNO, JOB) ORDER BY DEPTNO , JOB;
-- GROUPING 의 1의 결과는 그룹화되지 않은 데이터, 0의 결과는 그룹화된 데이터
SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL), GROUPING(DEPTNO), GROUPING(JOB) FROM EMP GROUP BY CUBE(DEPTNO, JOB) ORDER BY DEPTNO, JOB;

-- GROUPING과 달리 한 번에 여러 열 지정 그룹화 (GROUPING_ID) 함수
-- 그롭화 비트 벡터 값으로 출력 0 0 -> 0 / 0 1 -> 1 / 1 0 -> 2 / 1 1 -> 3
SELECT DEPTNO, JOB, COUNT(*), MAX(SAL), SUM(SAL), AVG(SAL), GROUPING(DEPTNO), GROUPING(JOB), GROUPING_ID(DEPTNO, JOB)  FROM EMP GROUP BY CUBE(DEPTNO, JOB) ORDER BY DEPTNO, JOB;

-- 그룹에 속한 데이터 가로 나열 함수 (LISTAGG)
SELECT DEPTNO, LISTAGG(ENAME, ', ') WITHIN GROUP(ORDER BY SAL DESC) AS ENAMES FROM EMP GROUP BY DEPTNO;

-- 행을 열로 바꾸는 함수(PIVOT) * 직책별, 부서별 최고 급여 2차원 표 형태
SELECT
	*
FROM
(
	SELECT 
		DEPTNO, JOB, SAL
	FROM 
		EMP
)
PIVOT(MAX(SAL) FOR DEPTNO IN (10, 20, 30))
ORDER BY JOB;

-- 부서별, 직책별 최고 급여 2차원 표 형태
SELECT 
	*
FROM 
(
	SELECT
		JOB, DEPTNO, SAL
	FROM 
		EMP
)
PIVOT(MAX(SAL) FOR JOB IN ('CLERK' AS CLERK,
							'SALESMAN' AS SALESMAN,
							'PRESIDENT' AS PRESIDENT,
							'MANAGER' AS MANAGER,
							'ANALYST' AS ANALYST
							)
ORDER BY DEPTNO;

-- PIVOT 이전 활용 방법 * 부서별, 직책별 최고 급여 2차원 표 형태
SELECT 
	DEPTNO,
	MAX(DECODE(JOB, 'CLERK', SAL)) AS "CLERK",
	MAX(DECODE(JOB, 'SALESMAN', SAL)) AS "SALESMAN",
	MAX(DECODE(JOB, 'PRESIDENT', SAL)) AS "PRESIDENT",
	MAX(DECODE(JOB, 'MANAGER', SAL)) AS "MANAGER",
	MAX(DECODE(JOB, 'ANALYST', SAL)) AS "ANALYST",
FROM 
	EMP E
GROUP BY 
	DEPTNO 
```

### 조인

```SQL
-- 등가 조인 * 내부 조인 또는 단순 조인 (외부 조인과 같이 명시하지 않으면 등가 조인)
SELECT
	EMPNO, ENAME, E.DEPTNO ,DNAME ,LOC 
FROM
	EMP E, DEPT D
WHERE
	E.DEPTNO = D.DEPTNO
ORDER BY
	EMPNO ;

-- 비등가 조인 * 등가 조인 방식 외의 방식
SELECT
	*
FROM
	SALGRADE s ,
	EMP e
WHERE
	E.SAL BETWEEN S.LOSAL AND S.HISAL ;

-- 자체 조인 * 직속 상관 정보 출력
SELECT 
	E1.EMPNO, E1.ENAME, E1.MGR, E2.EMPNO MGR_EMPNO, E2.ENAME MGR_ENAME
FROM 
	EMP e1, EMP e2
WHERE 
	e1.MGR = e2.EMPNO
ORDER BY 
	E1.EMPNO;

-- 외부 조인 * 
SELECT 
	E1.EMPNO, E1.ENAME, E1.MGR, E2.EMPNO MGR_EMPNO, E2.ENAME MGR_ENAME
FROM 
	EMP e1 LEFT JOIN EMP e2 
	ON E1.MGR = E2.EMPNO;
	
SELECT 
	E1.EMPNO, E1.ENAME, E1.MGR, E2.EMPNO MGR_EMPNO, E2.ENAME MGR_ENAME
FROM 
	EMP e2 LEFT JOIN EMP e1
	ON E2.EMPNO = E1.MGR 
ORDER BY 
	E1.MGR;
```

### 서브쿼리 

```SQL
-- JONES 보다 급여가 높은 사원 조회
SELECT
	SAL
FROM 
	EMP
WHERE 
	ENAME = 'JONES'
	
SELECT 
	*
FROM 	
	EMP 
WHERE 
	SAL > ( SELECT
				SAL
			FROM 
				EMP
			WHERE 
				ENAME = 'JONES')

-- 20번 부서에 속한 사원 중 전체 사원의 평균 급여보다 높은 급여를 받는 사원 정보와 소속 부서 정보
SELECT
	E.*,
	D.*
FROM
	EMP E,
	DEPT D
WHERE
	1=1
	AND E.DEPTNO = D.DEPTNO
	AND E.DEPTNO = 20
	AND E.SAL >= (SELECT 
					AVG(SAL)
				  FROM 
					EMP);

-- 각 부서별 최고 급여와 동일한 급여를 받는 사원 정보 출력 
SELECT
	*
FROM
	EMP
WHERE
	SAL IN (
			SELECT
				MAX(SAL)
			FROM
				EMP
			GROUP BY 
				DEPTNO
			);		

-- 부서번호가 10번인 사원의 이름과 번호, 부서정보 출력
WITH E10 AS
(
	SELECT * FROM EMP WHERE DEPTNO = 10
),
D AS 
(
	SELECT * FROM DEPT
)
SELECT
	E10.EMPNO, E10.ENAME, D.DNAME, D.LOC
FROM
	E10, D
WHERE 
	E10.DEPTNO = D.DEPTNO	

-- 급여가 LOSAL 과 HISAL 사이인 학년, 부서번호가 같은 부서이름 
SELECT 
	E1.EMPNO 
	, E1.ENAME 
	, (SELECT GRADE FROM SALGRADE S WHERE E1.SAL BETWEEN S.LOSAL AND S.HISAL) SALGRADE
	, (SELECT DNAME FROM DEPT D WHERE D.DEPTNO = E1.DEPTNO) DNAME
FROM 
	EMP E1;											
```

## 3장 데이터를 조작, 정의, 제어하는 SQL 배우기

**트랜잭션**

* 트랜잭션 : 더 이상 분할할 수 없는 최소 수행 단위
* 계좌 이체와 같이 하나의 작업 또는 밀접하게 연관된 작업을 수행하기 위한 DML 명령어로 이루어져있습니다.
* 트랙잭션은 여러 명령어를 한 번에 수행하여 작업을 완료하거나 아예 모두 수행하지 않는 상태, 즉 모든 작업을 취소합니다.
* 이러한 트랜잭션을 제어하기 위해 사용하는 명렁어를 TCL 이라고 합니다.(COMMIT, ROLLBACK)

**LOCK**

* 특정 세션에서 조작중인 데이터는 트랜잭션이 COMMIT, ROLLBACK 되기 전까지 다른 세션에서 조작할 수 없는 상태를 **LOCK** 이라고 합니다.
* 즉, 데이터가 잠기는 것이며, 조작 중인 데이터를 다른 세션은 조작할 수 없도록 접근을 보류시키는 것을 뜻합니다.

**SYNONYM**

* SYNONYM은 테이블, 뷰, 시퀀스 등 객체 이름 대신 사용할 수 있는 다른 이름을 부여하는 객체입니다.

```SQL
CREATE SYNONYM E FROM EMP;

SELECT * FROM E;
```

## 4장 PL/SQL 배우기

**프로시저**

* 프로시저 : PL/SQL로 작성된 일련의 SQL 문장을 포함하는 이름이 저장된 블록이며 이를 호출하여 사용이 가능합니다.
* 일반적으로 반복적 수행해야 하는 작업이나 복잡한 로직을 포함합니다.
* 파라미터를 통해 동적으로 데이터를 전달할 수 있습니다.
* 데이터베이스를 조작 및 트랜잭션 제어를 할 수 있습니다.

**PL/SQL**

* PL/SQL : 프로그래밍 언어와 유사한 블록 구조를 갖고 있습니다.
* 변수와 상수를 선언하여 데이터를 저장하고 사용할 수 있습니다.
* 제어 구문과 예외 처리를 통해 특정 동작을 수행할 수 있습니다.
* SQL은 데이터를 조작하고 쿼리하는데 사용, PL/SQL은 이러한 SQL문장을 조건문, 반복문 등과 결합하여 사용합니다.

**PL/SQL 블록의 기본형식은 아래와 같습니다.**

```SQL
DECLARE
	-- [실행에 필요한 여러 요소 선언]
BEGIN
	-- [작업을 위해 실제 실행하는 명령어]
EXCEPTION
	-- [PL/SQL 수행 도중 발생하는 오류 처리]
END;
```