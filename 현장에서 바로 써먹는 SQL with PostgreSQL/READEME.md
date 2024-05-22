# 현장에서 바로 써먹는 SQL with PostgreSQL - 김인용

## 개요

### 회사에서 Oracle 쿼리를 PostgreSQL 로 전환해야하는 업무에 앞서 PostgreSQL 배우고자함

## 1장 데이터베이스와 SQL

### 기초 아는 내용 패스

## 2장 데이터베이스 설치하기

### 기초 아는 내용 패스

## 3장 데이터 조회하기(기초)

### 기초 아는 내용 패스

**1. 원하는 개수 데이터 조회**

```sql
SELECT 열이름 FROM 테이블명 ORDER BY 정렬 기준 열 이름 LIMIT 숫자;

-- 처음부터 5번째가 아닌 2번째부터 5번째까지 총 4개만 조회하는 경우
SELECT 
    chick_no, hatchday, egg_weight 
FROM 
    chick_info
ORDER BY 
    egg_weight DESC, hatchday ASC, chick_no ASC
LIMIT 4 OFFSET 1;
```

**2. 데이터 타입 변환(TO_CHAR)**

```SQL
SELECT TO_CHAR(NOW(), 'YYYY');
SELECT TO_NUMBER('125', '999');
SELECT TO_DATE('05 Dec 2022', 'DD Mon YYYY');
SELECT NOW();
```

**3. NULL 변환(COALESCE, NULLIF)**

```SQL
COALESCE(열 이름, 반환값)
NULLIF(열 이름, 특정값)

-- COALESCE
SELECT 
    farm, date, humid, COALESCE(humid, 60)
FROM 
    env_cond
WHERE 
    date BETWEEN '2023-01-23' AND '2023-01-27'
    AND farm = 'A';

-- NULLIF
SELECT 
    farm, date, humid, NULLIF(humid, 60)
FROM 
    env_cond
WHERE 
    date BETWEEN '2023-01-23' AND '2023-01-27'
    AND farm = 'A';    
```

## 4장 데이터 조회하기(고급)

**1. UNION, UNION ALL**

* UNION : 중복 제거 O
* UNION ALL : 중복 제거 X

**UNION 사용 시 중복을 제거하기 위하여 데이터를 모두 검사하기 때문에 속도가 느리다. 왠만하면 UNION ALL 을 사용하자.**

```SQL
테이블 A UNION 테이블 B
테이블 A UNION ALL 테이블 B

(
    SELECT 
        chick_no, gender, hatchday
    FROM 
        chick_info
    WHERE 
        farm = 'A'
        AND gender = 'F'
        AND hatchday = '2023-01-03'
)
UNION
(
    SELECT 'A2300021','F','2023-01-05'
);
```

**2. 서브쿼리와 뷰 테이블**

```sql
-- 평균이 66.75 이상인 EGG_WEIGHT 대상건 조회
SELECT 
	CHICK_NO, EGG_WEIGHT
FROM 
	CHICK_INFO
WHERE 
	EGG_WEIGHT > (
					SELECT 
						AVG(EGG_WEIGHT) 
					FROM 
						CHICK_INFO CI 
				  )
```

**스칼라 서브쿼리**

```SQL
-- INNER JOIN 
SELECT 
	A.CHICK_NO , A.BREEDS , B.CODE_DESC BREEDS_NM
FROM 
	CHICK_INFO A, MASTER_CODE B
WHERE
	1=1
	AND A.BREEDS = B.CODE
	AND B.COLUMN_NM = 'breeds'

-- JOIN 대신에 스칼라 서브쿼리 (SELECT절 내에 단일 행 반환 경우 사용)
SELECT 
	CI.CHICK_NO 
	, CI.BREEDS 
	, (SELECT
			MC.CODE_DESC
		FROM
			MASTER_CODE MC
		WHERE
			MC.CODE = CI.BREEDS
			AND MC.COLUMN_NM = 'breeds'
	   )
FROM 
	CHICK_INFO CI 
```

**인라인 뷰**

```SQL
-- JOIN 대신 인라인 뷰(FROM 절 후 SELECT 절 사용)
SELECT 
	A.CHICK_NO ,
	A.BREEDS ,
	B.BREEDS_NM
FROM

	CHICK_INFO A,
(
	SELECT 
		MC.CODE CODE, MC.CODE_DESC BREEDS_NM
	FROM 
		MASTER_CODE MC
	WHERE 
		MC.COLUMN_NM = 'breeds'
) B
WHERE 
	A.BREEDS = B.CODE;
```

**적재적소에 서브쿼리를 사용함으로써 불필요한 JOIN을 줄여 성능 향상을 위하여 서브쿼리를 사용한다.**

**3. 가상 테이블 View**

```SQL
CREATE OR REPLACE VIEW 뷰테이블 이름 
(열 이름1, 열 이름2 ...)
AS 
SELECT 문;

-- 뷰 테이블 생성
CREATE OR REPLACE VIEW VIW_BREEDS_PROD 
(
	PROD_DATE, BREEDS_NM, TOTAL_SUM 
)
AS 
SELECT 
	A.PROD_DATE,
	(
		SELECT
			MC.CODE_DESC
		FROM
			MASTER_CODE MC
		WHERE
			1=1
			AND MC.COLUMN_NM = 'breeds'
			AND MC.CODE = B.BREEDS
	),
	SUM(A.RAW_WEIGHT) "TOTAL_SUM"
FROM 
	PROD_RESULT A,
	CHICK_INFO B
WHERE 
	A.CHICK_NO = B.CHICK_NO
	AND A.PASS_FAIL = 'P'
GROUP BY 
	A.PROD_DATE, B.BREEDS;
```

**4. 테이블 형태 변환(PIVOT)**

```SQL
-- PIVOT 사용 X 집계 쿼리 (부화일자별 병아리 마릿수)
SELECT 
	A.HATCHDAY,
	SUM(CASE WHEN GENDER = 'M' THEN A.CNT ELSE 0 END) MALE,
	SUM(CASE WHEN GENDER = 'F' THEN A.CNT ELSE 0 END) FEMALE
FROM 
(
	SELECT 
		HATCHDAY , GENDER , COUNT(*) CNT
	FROM 
		CHICK_INFO CI 
	GROUP BY 
		HATCHDAY , GENDER
) A
GROUP BY 
	HATCHDAY
ORDER BY 
	HATCHDAY

-- PIVOT 에 사용할 CROSSTAB 함수 사용방법
SELECT *
FROM CROSSTAB(
    '행열 변경할 쿼리'
)
AS 새 테이블명(열 이름1, 데이터 타입1, 열 이름2, 데이터 타입2...)

-- 테이블 함수 기능 확장 후 사용 가능
CREATE EXTENSION TABLEFUNC;

-- PIVOT 사용 O 집계 쿼리 (부화일자별 병아리 마릿수)
SELECT 
	*
FROM crosstab
(
	'SELECT 
		HATCHDAY , GENDER , COUNT(chick_no)::int AS CNT
	FROM 
		CHICK_INFO CI 
	GROUP BY 
		HATCHDAY , GENDER
	ORDER BY 
		HATCHDAY, GENDER DESC'
)
AS pivot_r(HATCHDAY date, "MALE" int, "FEMALE" int);
```

## 5장 데이터 수정하기

### 기초 아는 내용 패스(CRUD)

## 6장 프로시저와 잡

* 프로시저 : 일련의 쿼리를 하나의 함수처럼 실행하기 위한 쿼리의 집합.
* 잡 : 정해진 시간에 반복적으로 작업을 실행할 수 있도록 스케줄링해 주는 기능.
* 매일 09시 기준으로 창고의 재고 현황을 물류 관리 측면에서 기록하고 보고하는 경우를 구현하기 위해서는 프로시저와 잡이 필요하다.

```SQL
-- BREEDS_PROD_PROC 테이블 INSERT, SELECT문 프로시저 생성
INSERT INFO BREEDS_PROD_TBL(PROD_DATE, BREEDS_NM, TOTAL_SUM, SAVE_TIME)
(
    SELECT 
        PROD_DATE, BREEDS_NM, TOTAL_SUM ,CURRENT_TIMESTAMP AS SAVE_TIME
    FROM 
        BREEDS_PROD
    WHERE 
        PROD_DATE = '2023-01-31'
);

-- 프로시저 실행
CALL BREEDS_PROD_TBL();
```

**실무에서 프로시저를 많이 사용하니까 DataBase REPO에 프로시저 따로 공부하자..**

## 8장 데이터베이스 구조와 수행

**연산 및 절 우선순위**

* SELECT(6) - FROM(1) - JOIN(2) - WHERE(3) - GROUP BY(4) - HAVING(5) - ORDER BY(7) - LIMIT(8)
* ORDER BY 전 SELECT 가 온다는 사실에 유의하자.

**조인 방법**

1. **Nested Loop Join** 
	* 조인을 수행하는 가장 간단한 기본적인 방법
	* 하나의 테이블이 반복되고, 각 행에 대해서 다른 테이블에서 검색이 수행되는 방법
	* 대형 테이블 적용에는 적합하지 않음 
2. **Merge Join**
	* 조인 키를 기준으로 두 테이블을 정렬 후 병합 알고리즘을 사용해 함께 병합하는 방법
	* 대형 테이블이나 조인 키가 인덱싱된 경우에 적합
3. **Sort-Merge Join**
	* 두 테이블이 모두 조인 키에 대해서 정렬되어있을 때 sort-merge 알고리즘으로 두 테이블 병합
	* 해시 테이블 작성 비용보다 적은 대형 테이블에 가장 적합
4. **Index Join**
	* 테이블 중 하나 또는 둘 모두에 조인 키에 대한 인덱스가 있을 때 인덱스를 사용해 조인 수행
	* 조인 키에 대한 선택도가 높은 테이블에 가장 적합

* 오라클은 직접 강제로 지정할 수 있는 힌트가 존재하지만 PostgreSQL은 기본적으로 해당 기능을 제공하지 않는다.
* 인덱스와 조인은 사용자가 지정하지만 Planner 가 무엇을 쓸지 결정하므로 Planner가 최적의 방법을 선택할 수 있도록 사용자가 적합한 인덱스를 설정하고, 조인 쿼리를 작성하는 것이 성능 향상을 위해 중요하다.

## 9장 데이터 모델링과 ERD

### 기초 아는 내용 패스

## 10장 참고할 만한 내용들

### 기초 아는 내용 패스