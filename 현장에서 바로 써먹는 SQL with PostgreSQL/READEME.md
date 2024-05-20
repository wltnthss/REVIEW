# 현장에서 바로 써먹는 SQL with PostgreSQL - 김인용

## 개요

### 회사에서 Oracle 쿼리를 PostgreSQL 로 전환해야하는 업무에 앞서 PostgreSQL 배우고자함

## 1장 데이터베이스와 SQL

## 2장 데이터베이스 설치하기

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



## 4장 데이터 조회하기(고급)

## 5장 데이터 수정하기

## 6장 프로시저와 잡

## 7장 사례 기반 실습

## 8장 데이터베이스 구조와 수행

## 9장 데이터 모델링과 ERD

## 10장 참고할 만한 내용들