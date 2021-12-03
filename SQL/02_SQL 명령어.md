# SQL 명령어



## SQL 기본 명령어



### SQL의 기본 명령어는 **4가지**로 분류

- 데이터 정의어 (DDL) : 테이블 생성, 변경, 삭제

  > 테이블을 관리

- 데이터 조작어 (DML) : 데이터 삽입, 조회, 수정, 삭제  **(가장 많이 사용)**

  > 데이터를 관리

- 데이터 제어어 (DCL) : 데이터 접근 권한 부여, 제거

  > 데이터 권한 관리

- 트랜젝션 제어어 (TCL) : 데이터 조작어 (DML) 명령어 실행, 취소, 임시저장

  > 트렌젝션을 사용





### DBA, Data Analyst 역할

- DBA (**D**ata**B**ase **A**dministrator) 역할 : 데이터베이스 관리자이며, 기업 내에서 데이터베이스를 관리 (DDL, DCL 주로)
- Data Analyst 역할 : 데이터 분석을 통해, 새로운 인사이트를 도출 (DML, TCL 주로)





## 데이터 정의어 (DDL)

> **테이블**을 생성, 변경, 삭제할 때 사용하는 명령어



### 테이블, 데이터타입

- 테이블은 **각 열마다 반드시 1가지 데이터 타입**으로 정의되어야 함

ex) <회원 테이블>

| 회원번호 | 이름   | 가입일자   | 수신동의 |
| -------- | ------ | ---------- | -------- |
| 1001     | 홍길동 | 2020-01-02 | 1        |
| 1002     | 이순신 | 2020-01-03 | 0        |
| 1003     | 장영실 | 2020-01-04 | 1        |
| 1004     | 유관순 | 2020-01-05 | 0        |

> 회원번호 : 숫자형
>
> 이름 : 문자형
>
> 가입일자 : 날짜형
>
> 수신동의 : 숫자형(논리형)





### 테이블, 제약조건

- 테이블은 각 열마다 **제약 조건**을 정의할 수 있음

ex) <회원 테이블>

| 회원번호 | 이름   | 가입일자   | 수신동의 |
| -------- | ------ | ---------- | -------- |
| 1001     | 홍길동 | 2020-01-02 | 1        |
| 1002     | 이순신 | 2020-01-03 | 0        |
| 1003     | 장영실 | 2020-01-04 | 1        |
| 1004     | 유관순 | 2020-01-05 | 0        |

> 회원번호 : 숫자형 → PK(PRIMARY KEY)
>
> 이름 : 문자형
>
> 가입일자 : 날짜형 → NOT NULL
>
> 수신동의 : 숫자형(논리형)

> PK 
>
> - 중복되어 나타날 수 없는 단일 값
> - NOT NULL
>
> NOT NULL
>
> - NULL 을 허용하지 않음



```mysql
/*Practice 이름으로 데이터 베이스 생성*/
CREATE DATABASE Practice;

/*Practice 데이터 베이스 사용*/
USE Practice;

/*****************테이블 생성(Create)****************/
/*회원테이블 생성*/
CREATE TABLE 회원테이블 (
회원번호 INT PRIMARY KEY,
이름 VARCHAR(20),
가입일자 DATE NOT NULL,
수신동의 BIT
);

/*기본키(PRIMARY KEY): 중복되어 나타날 수 없는 단일 값 + NOT NULL*/
/*NOT NULL : NULL 허옹하지 않음*/

/*회원테이블 조회*/
SELECT *
	FROM 회원테이블;
    
/*****************테이블 열 추가****************/
/*성별 열 추가*/
ALTER TABLE 회원테이블 ADD 성별 VARCHAR(2);

/*회원테이블 조회*/
SELECT *
	FROM 회원테이블;

/*****************테이블 열 추가****************/
/*성별 열 타입 변경*/
ALTER TABLE 회원테이블 MODIFY 성별 VARCHAR(20);


/*****************테이블 열 이름변경****************/
/*성별 -> 성 열 이름 변경*/
ALTER TABLE 회원테이블 CHANGE 성별 성 VARCHAR(2);

/*****************테이블 열 이름변경****************/
ALTER TABLE 회원테이블 RENAME 회원정보;

/*회원테이블 조회*/
SELECT *
	FROM 회원정보;
    
/*****************테이블 삭제****************/
DROP TABLE 회원정보;

/*회원정보 조회 -> 삭제되었기 때문에 조회되지 않음*/
SELECT *
	FROM 회원정보;
```







## 데이터 조작어 (DML)

> 데이터 조작어는 데이터를 삽입, 조회, 수정, 삭제할 때 사용하는 명령어

```mysql
/* Practice 데이터베이스 사용*/
USE Practice;

/***************테이블 생성(Create)***************/
/* 회원테이블 생성 */
CREATE TABLE 회원테이블 (
회원번호 INT PRIMARY KEY,
이름 VARCHAR(20),
가입일자 DATE NOT NULL,
수신동의 BIT
);


/***************데이터 삽입*******************/  
INSERT INTO 회원테이블 VALUES (1001, '홍길동', '2020-01-02', 1);
INSERT INTO 회원테이블 VALUES (1002, '이순신', '2020-01-03', 0);
INSERT INTO 회원테이블 VALUES (1003, '장영실', '2020-01-04', 1);
INSERT INTO 회원테이블 VALUES (1004, '유관순', '2020-01-05', 0);

/* 회원테이블 조회 */
SELECT  *  FROM  회원테이블;


/***************조건 위반*******************/
/* PRIMARY KEY 제약 조건 위반 */
INSERT INTO 회원테이블 VALUES (1004, '장보고', '2020-01-06', 0);

/* NOT NULL 제약 조건 위반 */
INSERT INTO 회원테이블 VALUES (1005, '장보고', NULL, 0);

/* 데이터 타입 조건 위반 */
INSERT INTO 회원테이블 VALUES (1005, '장보고', 1, 0);

 
/***************데이터 조회***************/  
/* 모든 열 조회 */  
SELECT  *  
  FROM  회원테이블;
 
/* 특정 열 조회 */  
SELECT  회원번호,
      이름
  FROM  회원테이블;

/* 특정 열 이름 변경하여 조회 */  
SELECT  회원번호,
      이름 AS 성명
  FROM  회원테이블;


/***************데이터 수정*******************/
/* 모든 데이터 수정 */
UPDATE 회원테이블
   SET 수신동의 = 0;
  
  
/* 회원테이블 조회 */  
SELECT  *
  FROM  회원테이블;

/* 특정 조건 데이터 수정 */ 
UPDATE 회원테이블
   SET 수신동의 = 1
 WHERE 이름 = '홍길동';
 
/* 회원테이블 조회 */  
SELECT  *
  FROM  회원테이블;
  
  
/***************데이터 삭제*******************/  
/* 특정 데이터 삭제 */ 
DELETE 
  FROM 회원테이블
 WHERE 이름 = '홍길동';
 
/* 회원테이블 조회 */  
SELECT  *
  FROM  회원테이블;
  
/* 모든 데이터 삭제 */ 
DELETE 
  FROM 회원테이블;
 
/* 회원테이블 조회 */  
SELECT  *
  FROM  회원테이블;
```









## 데이터 제어어 (DCL)

> - 데이터 제어어는 데이터 접근 권한 부여 및 제거할 때 사용하는 명령어
> - 데이터베이스 관리자 (DBA)가 특정 사용자(User)에게 데이터 <u>접근 권한을 부여 및 제거할 때 사용하는 명령어</u>



```mysql
/***************사용자 확인***************/
/* MYSQL 데이터베이스 사용 */
USE MYSQL;

/* 사용자 확인 */
SELECT  *
  FROM  USER;
  
/***************사용자 추가***************/

/* 사용자 아이디 및 비밀번호 생성 */ 
CREATE USER 'TEST'@LOCALHOST IDENTIFIED BY 'TEST';
/*이때 @LOCALHOST는 로컬에서 접속이 가능하다는 것을 의미 (그림1)*/

/* 사용자 확인 */
SELECT  *
  FROM  USER;
  
/* 사용자 비밀번호 변경 */
SET PASSWORD FOR 'TEST'@LOCALHOST = '1234';

  
/***************권한 부여 및 제거***************/ 
/** 권한: CREATE, ALTER, DROP, INSERT, DELETE, UPDATE, SELECT 등  **/

/* 특정 권한 부여 */
GRANT SELECT, DELETE ON PRACTICE.회원테이블 TO 'TEST'@LOCALHOST;
/*TEST에서 SELECT랑 DELETE를 쓸 수 있게 권한 부여*/

/* 특정 권한 제거 */
REVOKE DELETE ON PRACTICE.회원테이블 FROM 'TEST'@LOCALHOST;
/*TEST에서 DELETE를 쓸 수 없게 권한 제어*/

/* 모든 권한 부여 */
GRANT ALL ON Practice.회원테이블 TO 'TEST'@LOCALHOST;
/*TEST에서 모든 명령어 권한 부여*/

/* 모든 권한 제거 */
REVOKE ALL ON Practice.회원테이블 FROM 'TEST'@LOCALHOST;
/*TEST에서 모든 명령어 권한 제거*/

/***************사용자 삭제***************/ 

/* 사용자 삭제 */
DROP USER 'TEST'@LOCALHOST;

/* 사용자 확인 */
SELECT  *
  FROM  USER;
```

> (그림1)

<img src="/Users/taeyoung/Library/Application Support/typora-user-images/image-20211202105446731.png" alt="image-20211202105446731" style="zoom: 33%;" />> 





## 트랜젝션 제어어(TCL)

> - 트랜젝션 제어어는 데이터 조작어(DML) 명령어 실행, 취소, 임시저장할 때 사용하는 명령어
> - 트랜젝션(Transaction)은 **분할할 수 없는 최소 단위**, 논리적인 작업 단위
> - 실행, 취소
>   - 실행(COMMIT) : 모든 작업을 최종 실행
>   - 취소(ROLLBACK) : 모든 작업 되돌림



```mysql
/* Practice 데이터베이스 사용*/
USE Practice;

/***************테이블 생성(Create)***************/
/* (회원테이블 존재할 시, 회원테이블 삭제) */
DROP TABLE 회원테이블;

/* 회원테이블 생성 */
CREATE TABLE 회원테이블 (
회원번호 INT PRIMARY KEY,
이름 VARCHAR(20),
가입일자 DATE NOT NULL,
수신동의 BIT
);

/* 회원테이블 조회 */
SELECT  *  FROM  회원테이블;


/***************BEGIN + 취소(ROLLBACK)*******************/  
/* 트랜젝션 시작 */
BEGIN;

/* 데이터 삽입 */
INSERT INTO 회원테이블 VALUES (1001, '홍길동', '2020-01-02', 1);

/* 회원테이블 조회 */
SELECT  *  FROM  회원테이블;

/* 취소 */
ROLLBACK;


/* 회원테이블 조회 */
SELECT  *  FROM  회원테이블;


/***************BEGIN + 실행(COMMIT)*******************/  
/* 트랜젝션 시작 */
BEGIN;

/* 데이터 삽입 */
INSERT INTO 회원테이블 VALUES (1005, '장보고', '2020-01-06', 1);

/* 실행 */
COMMIT;

/* 회원테이블 조회 */
SELECT  *  FROM  회원테이블;


/***************임시 저장(SAVEPOINT)*******************/ 
/* (회원테이블에 데이터 존재할 시, 데이터 모두 삭제) */
DELETE FROM 회원테이블;

/* 회원테이블 조회 */
SELECT  *  FROM  회원테이블;

/* 트랜젝션 시작 */
BEGIN;

/* 데이터 삽입 */
INSERT INTO 회원테이블 VALUES (1005, '장보고', '2020-01-06', 1);

/* SAVEPOINT 지정 */
SAVEPOINT S1;

/* 1005 회원 이름 수정 */
UPDATE  회원테이블
   SET  이름 = '이순신';
 
/* SAVEPOINT 지정 */
SAVEPOINT S2;

/* 1005 회원 데이터 삭제 */
DELETE 
  FROM  회원테이블;
 
/* SAVEPOINT 지정 */
SAVEPOINT S3;

/* 회원테이블 조회 */
SELECT  *  FROM  회원테이블;

/* SAVEPOINT S2 저장점으로 ROLLBACK */
ROLLBACK TO S2;
 
/* 회원테이블 조회 */
SELECT  *  FROM  회원테이블;

/* 실행 */
COMMIT;

/* 회원테이블 조회 */
SELECT  *  FROM  회원테이블;
```



