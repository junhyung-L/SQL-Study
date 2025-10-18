# SQL-Study
# 데이터베이스 요약 정리
---

## 목차
1. [핵심 용어](#핵심-용어)  
2. [대표 DBMS와 클라우드](#대표-dbms와-클라우드)  
3. [엔티티/관계 (학사 예시 도메인)](#엔티티관계-학사-예시-도메인)  
4. [전통 파일시스템 vs 데이터베이스 접근](#전통-파일시스템-vs-데이터베이스-접근)  
5. [데이터 모델의 역사/유형](#데이터-모델의-역사유형)  
6. [관계형 모델 기본](#관계형-모델-기본)  
7. [키와 무결성 제약](#키와-무결성-제약)  
8. [관계대수 치트시트](#관계대수-치트시트)  
9. [SQL 기초 문법 요약](#sql-기초-문법-요약)  
10. [스키마/DDL 예시](#스키마ddl-예시)  
11. [JOIN/서브쿼리/VIEW 요약](#join서브쿼리view-요약)  
12. [데이터 타입 요약](#데이터-타입-요약)  
13. [부록: 관계 vs NoSQL 한눈 비교](#부록-관계-vs-nosql-한눈-비교)

---

## 핵심 용어
- **데이터(Data)**: 기록 가능한 사실/정보  
- **데이터베이스(DB)**: 관련 데이터를 **체계적**으로 모아둔 **공유 저장소**  
- **DBMS**: DB를 **저장/관리/접근**할 수 있게 해주는 소프트웨어  
- **데이터베이스 시스템**: **DB + DBMS**의 통합 개념

---

## 대표 DBMS와 클라우드
- **MySQL**: 가장 널리 쓰이는 오픈소스 RDBMS, 동시접속/안정성 우수 (Zoom, Airbnb, Boeing 등)  
- **SQLite**: 설치 없이 쓰는 경량형 DBMS (iOS/Android/브라우저에 내장)  
- **MongoDB**: 문서(Document) 지향 **NoSQL** (JSON 유사, 스키마 유연) — eBay, LG U+, Lyft 등  
- **Cloud Databases**: 예) **Snowflake**, AWS 기반 서비스 등

---

## 엔티티/관계 (학사 예시 도메인)
**주요 엔티티**
- `STUDENT(student_id, name, major, year, gpa)`
- `INSTRUCTOR(instructor_id, name, department)`
- `COURSE(course_id, title, credits, …)`
- `DEPARTMENT(dept_id, dept_name, courses_offered)`

**주요 관계**
- `STUDENT` **takes** `COURSE` (학생은 강의를 수강)  
- `COURSE` **has prerequisite** `COURSE` (강의는 선수과목을 가질 수 있음)  
- `INSTRUCTOR` **teaches** `COURSE` (교수는 강의를 담당)  
- `STUDENT` **majors in** `DEPARTMENT` (학생은 학과를 전공)

---

## 전통 파일시스템 vs 데이터베이스 접근
**전통 파일시스템(문제점)**  
중복·불일치, 프로그램-데이터 결합, 동시성 제어 어려움

**데이터베이스 접근(특징)**  
1) **자기 기술성**(메타데이터 포함)  
2) **프로그램-데이터 분리**(데이터 독립성)  
3) **다중 뷰 지원**(역할별 맞춤 보기)  
4) **공유/다중 사용자 트랜잭션 처리** — **ACID** 특성
- **Atomicity**: 전부 수행 또는 전부 취소  
- **Consistency**: 일관성 유지  
- **Isolation**: 트랜잭션 간 간섭 없음  
- **Durability**: 장애 후에도 결과 보존

---

## 데이터 모델의 역사/유형
- **계층형(1960s)**: 트리 구조  
- **네트워크형**: 복잡한 관계 표현  
- **관계형(1970s~현재 주류)**: **테이블 + SQL** (MySQL, Oracle, DB2 등)  
- **OODBMS**: 객체 지향, 복잡성으로 제한적 사용  
- **ORDBMS**: 관계형 + 객체 개념 혼합  
- **XML 모델**: 웹 데이터 저장  
- **NoSQL(2000s~)**: 비정형/대용량 — 키-값/컬럼/그래프/문서  
  - 관계형(SQL): **정형 스키마**, 조인/정합성 강점  
  - NoSQL: **스키마 유연**, 확장성/다양 데이터에 유리

---

## 관계형 모델 기본
- **Relation(테이블)** = 행(Row, Tuple) + 열(Column, Attribute)  
- **Domain**: 속성에 허용된 값의 집합  
- **Schema**: 테이블 구조 정의(이름, 열 목록, 타입, 제약)  
- **Instance**: 특정 시점의 실제 데이터

**특징**
1) 튜플(행) **순서 없음**  
2) 속성(열) **이름 고유**  
3) 각 셀은 **원자값(Atomic)**  
4) **중복 튜플 금지**  
5) **NULL** 허용(미확정/결측)

---

## 키와 무결성 제약
- **도메인 제약**: 값은 자료형/범위에 적합해야 함  
- **키 제약**
  - **슈퍼키**: 튜플을 고유 식별하는 속성 집합  
  - **후보키**: **최소(superkey의 최소)** 식별자  
  - **기본키(PK)**: 대표 후보키, **NULL 불가**, 변경 적음  
  - **외래키(FK)**: 다른 테이블 **PK 참조**
- **개체 무결성**: PK는 NULL 불가  
- **참조 무결성**: FK는 참조 PK와 일치하거나 NULL

**예시**  
`instructor(ID, name, dept_name, salary)`  
- 슈퍼키: (ID), (ID, name), (ID, dept_name), (ID, name, dept_name, salary)…  
- 후보키: (ID) *(또는 고유한 email이 있다면 (email))*  
- 기본키: (ID)

`department(dept_name)` — **PK**  
`course(dept_name)` — **FK** → `department.dept_name`

---

## 관계대수 치트시트
**단항 연산**
- **선택(σ)**: 행 필터 → `σ조건(관계)`  
  - 예) `σdept_name='Physics'(Instructor)`
- **투영(π)**: 열 선택(중복 제거) → `π열목록(관계)`  
  - 예) `πname, salary(Instructor)`
- **이름변경(ρ)**: 테이블/열 이름 변경 → `ρ새이름(관계)` 또는 `ρ새이름(열목록)(관계)`

**집합 연산** *(두 테이블은 union-compatible: 열 수/타입 동일)*  
- **합집합(∪)**, **교집합(∩)**, **차집합(−)**

**이항 연산**
- **카티션곱(×)**: 모든 조합 → `|R| × |S|`  
- **조인(⋈)**: 조건 일치 튜플 결합 → `R ⋈조건 S`  
  - 예) `Instructor ⋈dept_name Department`
- **나눗셈(÷)**: “S의 모든 튜플과 관련된 R의 튜플” 질의에 사용  
  - 예) 특정 학과(IME) **모든 과목**을 수강한 학생 찾기

**표현 예시**
- **물리/수학과 교수 이름**  
  `πname(σdept_name='Physics'(Instructor)) ∪ πname(σdept_name='Math'(Instructor))`  
  또는 `πname(σdept_name ∈ {'Physics','Math'}(Instructor))`
- **ScienceHall이 아닌 건물의 교수 이름**  
  `πname(σbuilding≠'ScienceHall'(Instructor ⋈dept_name Department))`

---

## SQL 기초 문법 요약

### SELECT 기본 구조
```sql
SELECT 컬럼
FROM 테이블
[JOIN ...]
[WHERE 조건]
[GROUP BY 컬럼들 [HAVING 조건]]
[ORDER BY 컬럼 ASC|DESC]
[LIMIT N];
```

### 분류
- **DDL**: `CREATE`, `ALTER`, `DROP`  
- **DML**: `INSERT`, `UPDATE`, `DELETE`, `SELECT`  
- **DCL**: `GRANT`, `REVOKE`

### 조건/논리/집합/정렬
```sql
-- 조건/논리
=, <>, <, >, <=, >=
AND, OR, NOT, BETWEEN

-- 집합 조건
WHERE major IN ('CS', 'Math');

-- 정렬/제한
ORDER BY gpa DESC;
ORDER BY major ASC, gpa DESC;
LIMIT 3;

-- 패턴 매칭
WHERE name LIKE 'J%';     -- J로 시작
WHERE name LIKE 'J_n%';   -- J + 1글자 + 그 뒤

-- NULL
WHERE gpa IS NULL;
WHERE gpa IS NOT NULL;
```

### 집계 함수
```sql
SELECT COUNT(*), SUM(x), AVG(x), MAX(x), MIN(x)
FROM T
[GROUP BY key]
[HAVING 조건];
```

### 간단 DML 예시
```sql
-- INSERT
INSERT INTO Students VALUES 
(1, 'Alice Kim', 20, 3.8, 'alice@inu.ac.kr', 'CS', 45),
(2, 'Bob Lee',   22, 2.9, 'bob@gmail.com',   'Math', 30),
(7, 'Grace Han', 20, 3.9, 'grace@inu.ac.kr', 'CS',   75);

-- 명시적 INSERT
INSERT INTO Students(sid, name, age, gpa, email, major, credits) VALUES
(1, 'Alice Kim', 20, 3.8, 'alice@inu.ac.kr', 'CS', 45);

-- DELETE
DELETE FROM Students WHERE gpa > 3.0;
DELETE FROM Students WHERE major IS NULL;

-- UPDATE
UPDATE Students
SET credits = credits + 5
WHERE gpa > 3.6;

-- SELECT
SELECT * FROM Students;
SELECT DISTINCT major FROM Students;
SELECT name, gpa FROM Students ORDER BY gpa DESC LIMIT 3;
```

---

## 스키마/DDL 예시

### 스키마 생성
```sql
CREATE SCHEMA PURCHASE AUTHORIZATION user_name;
```

### 기본 스키마에 테이블 생성
```sql
CREATE TABLE PRODUCT (
  pid   CHAR(10),
  pname VARCHAR(50),
  price REAL
);
```

### 특정 스키마에 테이블 생성
```sql
CREATE TABLE PURCHASE.PRODUCT (
  pid   CHAR(10),
  pname VARCHAR(50),
  price REAL
);
```

### 학사 예제 테이블
```sql
CREATE TABLE Students (
  sid   CHAR(20)  NOT NULL,
  name  CHAR(20)  NOT NULL,
  login CHAR(10),
  age   INTEGER,
  gpa   REAL,
  PRIMARY KEY (sid)
);

CREATE TABLE Courses (
  Cid   CHAR(20)  NOT NULL,
  DName CHAR(20),
  DNum  INTEGER,
  Cname CHAR(10),
  PRIMARY KEY (Cid)
);

CREATE TABLE Enrolled (
  sid   VARCHAR(20) NOT NULL,
  cid   VARCHAR(20) NOT NULL,
  grade CHAR(2),
  PRIMARY KEY (sid, cid),
  FOREIGN KEY (sid) REFERENCES Students(sid),
  FOREIGN KEY (cid) REFERENCES Courses(Cid)
);
```

### ALTER / DROP
```sql
-- 컬럼 추가
ALTER TABLE Students ADD email VARCHAR(50);

-- 타입 변경
ALTER TABLE Students MODIFY gpa DOUBLE;

-- 이름+타입 변경
ALTER TABLE Students CHANGE gpa student_gpa FLOAT;

-- 기본키 제거
ALTER TABLE Students DROP PRIMARY KEY;

-- 삭제
DROP TABLE IF EXISTS Students;
DROP SCHEMA PURCHASE;
```

---

## JOIN/서브쿼리/VIEW 요약

### JOIN
```sql
-- CROSS JOIN(카티션곱)
SELECT * FROM Students, Enrolled;

-- INNER JOIN
SELECT * FROM Students s
JOIN Enrolled e ON s.sid = e.sid;

-- LEFT OUTER JOIN
SELECT s.sid, s.name, e.cid, e.grade
FROM Students s
LEFT JOIN Enrolled e ON s.sid = e.sid;

-- RIGHT OUTER JOIN
SELECT e.sid, s.name, e.cid, e.grade
FROM Students s
RIGHT JOIN Enrolled e ON s.sid = e.sid;

-- FULL OUTER JOIN (DBMS별 지원 상이)
SELECT s.sid, s.name, e.cid, e.grade
FROM Students s
LEFT JOIN Enrolled e ON s.sid = e.sid
UNION
SELECT s.sid, s.name, e.cid, e.grade
FROM Students s
RIGHT JOIN Enrolled e ON s.sid = e.sid;

-- SELF JOIN (같은 테이블 내 관계)
SELECT c1.Cname AS course1, c2.Cname AS course2, c1.DName
FROM Courses c1
JOIN Courses c2
  ON c1.DName = c2.DName AND c1.Cid < c2.Cid;
```

### 서브쿼리
```sql
-- IN
SELECT s.name
FROM Students s
WHERE s.sid IN (SELECT e.sid FROM Enrolled e WHERE e.cid = '101');

-- 중첩
SELECT s.name
FROM Students s
WHERE s.sid IN (
  SELECT e.sid
  FROM Enrolled e
  WHERE e.cid = (
    SELECT c.cid FROM Courses c WHERE c.Cname = 'Database Systems'
  )
);

-- EXISTS / NOT EXISTS
SELECT s.name
FROM Students s
WHERE EXISTS (
  SELECT 1 FROM Enrolled e WHERE e.sid = s.sid AND e.cid = '101'
);

SELECT s.name
FROM Students s
WHERE NOT EXISTS (
  SELECT 1 FROM Enrolled e WHERE e.sid = s.sid AND e.cid = '101'
);
```

### VIEW
```sql
CREATE VIEW v_enrolled_101 AS
SELECT sid FROM Enrolled WHERE cid = '101';

SELECT name
FROM Students
WHERE sid IN (SELECT sid FROM v_enrolled_101);
```

---

## 데이터 타입 요약
- **정수**: `TINYINT`, `SMALLINT`, `INTEGER`, `BIGINT`  
- **실수**: `REAL`, `FLOAT`, `DOUBLE`  
- **문자열**: `CHAR(n)`, `VARCHAR(n)`, `TEXT`  
- **비트열**: `BIT(n)`, `BIT VARYING(n)`  
- **기타**: `BOOLEAN`, `DATE`, `TIMESTAMP`

---

## 부록: 관계 vs NoSQL 한눈 비교
| 구분 | 관계형(SQL) | NoSQL |
|---|---|---|
| 스키마 | 고정/정형 | 유연/스키마리스(문서 등) |
| 질의 | SQL/조인 강력 | 문서 탐색/중첩 구조 유리 |
| 정합성 | 강한 정합성 선호 | 가용성/확장성 우선 설계 가능 |
| 사용처 | OLTP/정규화된 도메인 | 로그/이벤트/콘텐츠/그래프 등 |
