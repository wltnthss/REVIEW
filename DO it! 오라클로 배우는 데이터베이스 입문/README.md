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



## 3장 데이터를 조작, 정의, 제어하는 SQL 배우기

## 4장 PL/SQL 배우기