# SQL 문법



## 데이터 조회(SELECT)

> - 데이터 조회(SELECT)는 데이터 조작어(DML)이며, 데이터 분석에서 가장 많이 사용되는 명령어
>
> - 데이터 조회(SELECT)는 여러 절들과 함께 사용되며, 분석에 필요한 데이터를 조회
>
>   - **절**
>
>   - FROM : 테이블 확인 
>
>     > 회원 테이블을
>
>   - WHERE : FROM절 테이블을 특정 조건으로 필터링 
>
>     > 성별이 남성 조건으로 필터링 하여
>
>   - GROUP BY : 열 별로 그룹화 (**이 절을 사용하면 기존 테이블이 새로운 테이블로 전환됨**)
>
>     > 거주지역별로 회원 수 집계
>
>   - HAVING : 그룹화된 새로운 테이블을 특정 조건으로 필터링 (WHERE과 비슷함)
>
>     > 집계 회원 수 100명 미만 조건으로 필터링
>
>   - SELECT : 열 선택
>
>     > 모든 열 조회
>
>   - ORDER BY : 열 정렬
>
>     > 집계 회원수가 높은 순으로
>
> - SQL 실행순서 : FROM → WHERE → GROUP BY
>
>   > 이때 WHERE은 생략 가능
>
> - GROUP BY 는 **집계함수**와 주로 사용되는 명령어
>
>   - **여러 열별**로 그룹화가 가능
>   - GROUP BY에 있는 열들을 SELECT에도 작성해야 <u>원하는 분석 결과를 확인</u> 가능



(강좌에서 제공한 테이블 사용)

```sql
USE PRACTICE;

/***************FROM***************/

/* Customer 테이블 모든 열 조회 */
SELECT  *
  FROM  CUSTOMER;


/***************WHERE***************/

/* 성별이 남성 조건으로 필터링 */
SELECT  *
  FROM  CUSTOMER
 WHERE  GENDER = 'MAN';
 
 
/***************GROUP BY***************/

/* 지역별로 회원수 집계 */
SELECT  ADDR
      ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR;
    /* GROUP BY와 SELECT 뒤에 오는 열은 같은 것이 바람직하다 */
    
/* COUNT: 행들의 개수를 구하는 집계함수 */


/***************HAVING***************/

/* 집계 회원수 100명 미만 조건으로 필터링 */
SELECT  ADDR
      ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR
HAVING  COUNT(MEM_NO) < 100;
    
/* < : 비교 연산자 / ~ 보다 작은*/


/***************ORDER BY***************/

/* 집계 회원수가 높은 순으로 */
SELECT  ADDR
      ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR
HAVING  COUNT(MEM_NO) < 100
 ORDER
   BY  COUNT(MEM_NO) DESC;
    
/* DESC : 내림차순 / ASC : 오름차순 */




/***************FROM -> (WHERE) -> GROUP BY***************/

/* FROM -> GROUP BY 순으로 작성해도 됩니다. */
SELECT  ADDR
      ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
/* WHERE  GENDER = 'MAN' */
 GROUP
    BY  ADDR;


/***************GROUP BY + 집계함수***************/
/* 거주지역을 서울, 인천 조건으로 필터링 */
/* 거주지역 및 성별로 회원수 집계 */
SELECT  ADDR
      ,GENDER
      ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
 WHERE  ADDR IN ('SEOUL', 'INCHEON')
 GROUP
    BY  ADDR
      ,GENDER;

/* IN : 특수 연산자 / IN (List) / 리스트 값만 */

/* GROUP BY에 있는 열들을 SELECT에도 작성해야 원하는 분석 결과를 확인할 수 있습니다. */
SELECT  GENDER
      ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
 WHERE  ADDR IN ('SEOUL', 'INCHEON')
 GROUP
    BY  ADDR
      ,GENDER;
        

/***************SQL 명령어 작성법***************/
/* 회원테이블(Customer)을 */
/* 성별이 남성 조건으로 필터링하여 */
/* 거주지역별로 회원수 집계 */
/* 집계 회원수 100명 미만 조건으로 필터링 */
/* 모든 열 조회 */
/* 집계 회원수가 높은 순으로 */

SELECT  ADDR
        ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR
HAVING  COUNT(MEM_NO) < 100
 ORDER
    BY  COUNT(MEM_NO) DESC;
/* SQL명령어는 띄어쓰기, 줄바꿈, TAB에 대한 자유도가 높기 때문에 위 처럼 딱 봤을 때 보기 좋게 
   space와 tab을 적절히 사용하면 좋음*/
```





## 데이터 결합 (JOIN)

> - 관계는 1:1, 1:N, N:N 세가지 형태로, 테이블간의 연결이 가능하다.
> - 테이블 결합(JOIN)은 두 테이블 관계를 활용하여, 테이블을 결합하는 명령어
> - 테이블 결합(JOIN)을 통해, 여러 테이블을 활용하여 분석이 가능
> - ERM(**E**ntity-**R**elationship **M**odelling)은 개체-관계 모델링이며, 관계형 데이터베이스에 테이블을 모델링할 때 사용
>   - 개체(Entitiy) : 하나 이상의 속성(Attribute)으로 구성된 객체
>   - 관계(Relationship) : 속성(Entity)들의 관계
> - EDM(**E**ntitiy-**R**elationship **D**iagram)은 개체 간의 관계를 도표로 표현할 때 사용



```sql
USE PRACTICE;

/***************INNER JOIN***************/
/* INNER JOIN: 두 테이블의 공통 값이 매칭되는 데이터만 결합 */

/* Customer + Sales Inner JOIN */
SELECT  *
  FROM  CUSTOMER AS A
 INNER
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO;

/* Customer 및 Sales 테이블은 mem_no(회원번호) 기준으로 1:N 관계 */
SELECT  *
  FROM  CUSTOMER AS A
 INNER
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
 WHERE  A.MEM_NO = '1000970';
 
 
/***************LEFT JOIN***************/
/* LEFT JOIN: 두 테이블의 공통 값이 매칭되는 데이터만 결합 + 왼쪽 테이블의 매칭되는 않는 데이터는 NULL */

/* Customer + Sales LEFT JOIN */
SELECT  *
  FROM  CUSTOMER AS A
  LEFT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO;

/* NULL은 회원가입만하고 주문은 하지 않는 회원을 의미 */


/***************RIGHT JOIN***************/
/* RIGHT JOIN: 두 테이블의 공통 값이 매칭되는 데이터만 결합 + 오른쪽 테이블의 매칭되는 않는 데이터는 NULL */

/* Customer + Sales RIGHT JOIN */
SELECT  *
  FROM  CUSTOMER AS A
 RIGHT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
 WHERE  A.MEM_NO IS NULL;

/* 회원번호(9999999)는 비회원 */
/* IS NULL: 비교 연산자 / NULL인 값만 */


/***************테이블 결합(JOIN) + 데이터 조회(SELECT)***************/

/* 회원(Customer) 및 주문(Sales) 테이블 Inner JOIN 결합 */
SELECT  *
  FROM  CUSTOMER AS A
 INNER
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO;

/* 임시테이블 생성 */
CREATE TEMPORARY TABLE CUSTOMER_SALES_INNER_JOIN
SELECT  A.*
        ,B.ORDER_NO
  FROM  CUSTOMER AS A
 INNER
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO;

/* 임시테이블 조회 */
SELECT * FROM CUSTOMER_SALES_INNER_JOIN;

/* 임시테이블(TEMPORARY TABLE)은 서버 연결 종료시 자동으로 삭제됩니다. */
   

/* 성별이 남성 조건으로 필터링하여 */
SELECT  *
  FROM  CUSTOMER_SALES_INNER_JOIN
 WHERE  GENDER = 'MAN';


/* 거주지역별로 구매횟수 집계 */
SELECT  ADDR
      ,COUNT(ORDER_NO) AS 구매횟수
  FROM  CUSTOMER_SALES_INNER_JOIN
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR;


/* 구매횟수 100회 미만 조건으로 필터링 */
SELECT  ADDR
      ,COUNT(ORDER_NO) AS 구매횟수
  FROM  CUSTOMER_SALES_INNER_JOIN
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR
HAVING  COUNT(ORDER_NO) < 100;


/* 모든 열 조회 */
/* 구매횟수가 낮은 순으로 */
SELECT  ADDR
      ,COUNT(ORDER_NO) AS 구매횟수
  FROM  CUSTOMER_SALES_INNER_JOIN
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR
HAVING  COUNT(ORDER_NO) < 100
 ORDER
    BY  COUNT(ORDER_NO) ASC;
    
    
/***************3개 이상 테이블 결합***************/
/* 주문(Sales) 테이블 기준, 회원(Customer) 및 상품(Product) 테이블 LEFT JOIN 결합 */

SELECT  *
  FROM  SALES AS A
  LEFT
  JOIN  CUSTOMER AS B
    ON  A.MEM_NO = B.MEM_NO
  LEFT
  JOIN  PRODUCT AS C
    ON  A.PRODUCT_CODE = C.PRODUCT_CODE;

```

> - Inner Join : 두 테이블의 공통 값이 매칭되는 데이터만 결합
>
>   > 회원가입 후, 주문이력이 있는 회원
>
> - Left Join : 두 테이블의 공통 값이 매칭되는 데이터만 결합 + **왼쪽** 테이블의 매칭되지 않는 데이터는 NULL
>
>   > 회원가입 후, 주문이력이 있는 회원 + 회원가입 후, 주문 이력이 **없는** 회원
>
> - Right Join : 두 테이블의 공통 값이 매칭되는 데이터만 결합 + **오른쪽** 테이블의 매칭되지 않는 데이터는 NULL
>
>   > 회원가입 후, 주문이력이 있는 회원 + **비회원**





## 서브 쿼리 (Sub Query)

> - 서브쿼리(Sub Query)는 **SELECT문 안에 또 다른 SELECT문**이 있는 명령어
>
> - 지금까지 작성한 코드 = 메인 쿼리 (Main Query)
>
>   메인 쿼리 안에 있는 것이 서브 쿼리 (Sub Query)
>
>   - SELECT : SELECT절 서브 쿼리
>   - FROM : FROM절 서브 쿼리
>   - WHERE : WHERE절 서브 쿼리





```sql
USE PRACTICE;


/***************SELECT절 서브 쿼리***************/
/* SELECT 명령문 안에 SELECT 명령문 */

SELECT  *
        ,(SELECT GENDER FROM CUSTOMER WHERE A.MEM_NO = MEM_NO) AS GENDER
  FROM  SALES AS A;

/* 확인 */
SELECT  *
  FROM  CUSTOMER
 WHERE  MEM_NO = '1000970';

/* SELECT절 서브 쿼리 vs 테이블 결합(JOIN) 처리 속도 비교 */
/* SELECT절 서브 쿼리는 실행속도가 매우 느리다! */
SELECT  A.*
      ,B.GENDER
  FROM  SALES AS A
  LEFT
  JOIN  CUSTOMER AS B
    ON  A.MEM_NO = B.MEM_NO;


/***************FROM절 서브 쿼리***************/
/* FROM 명령문 안에 SELECT 명령문 */

/* 원래 FROM에서 알 수 있듯 FROM 뒤에는 테이블 명이 나옴 */
/* 따라서 () 안에는 테이블이 와야함 */
/* GROUP BY를 사용하면 새로운 테이블이 생성됨 */
/* 따라서 아래와 같이 작성하면 된다! */ 

SELECT  *
  FROM  (
      SELECT  MEM_NO
              ,COUNT(ORDER_NO) AS 주문횟수
        FROM  SALES
       GROUP
          BY  MEM_NO
      )AS A;

/* FROM절 서브 쿼리: 열 및 테이블명 지정 */       

            
/***************WHERE절 서브 쿼리***************/
/* WHERE 명령문 안에 SELECT 명령문 */

/* 2019년도에 가입한 회원의 주문 횟수 구하기 */
/* IN 뒤에는 리스트 형태가 나와야 함 */

SELECT  COUNT(ORDER_NO) AS 주문횟수
  FROM  SALES
 WHERE  MEM_NO IN (SELECT  MEM_NO FROM CUSTOMER WHERE YEAR(JOIN_DATE) = 2019);
 
/* YEAR: 날짜형 함수 / 연도 변환 */    

SELECT  *
        ,YEAR(JOIN_DATE)
  FROM  CUSTOMER;

/* 리스트 */
SELECT  MEM_NO FROM CUSTOMER WHERE YEAR(JOIN_DATE) = 2019;
/*위 코드의 결과는 아래와 같음*/
SELECT  COUNT(ORDER_NO) AS 주문횟수
  FROM  SALES
 WHERE  MEM_NO IN (~~) /* ~~ = 회원 번호를 직접 다 입력한 리스트*/

/* WHERE절 서브 쿼리 vs 데이터 결합(JOIN) 결과 값 비교 */
SELECT  COUNT(A.ORDER_NO) AS 주문횟수
  FROM  SALES AS A
 INNER
  JOIN  CUSTOMER AS B
    ON  A.MEM_NO = B.MEM_NO
 WHERE  YEAR(B.JOIN_DATE) = 2019;
 
 
/***************서브 쿼리(Sub Query) + 테이블 결합(JOIN)***************/
/* 임시테이블 생성 */
CREATE TEMPORARY TABLE SALES_SUB_QUERY
SELECT  A.구매횟수
        ,B.*
  FROM  (
         SELECT  MEM_NO
                 ,COUNT(ORDER_NO) AS 구매횟수
           FROM  SALES
          GROUP
             BY  MEM_NO
        )AS A
 INNER
  JOIN  CUSTOMER AS B
    ON  A.MEM_NO = B.MEM_NO;

/* 임시테이블 조회 */
SELECT * FROM SALES_SUB_QUERY;

/* 성별이 남성 조건으로 필터링하여 */
SELECT  *
  FROM  SALES_SUB_QUERY
 WHERE  GENDER = 'MAN';


/* 거주지역별로 구매횟수 집계 */
SELECT  ADDR
      ,SUM(구매횟수) AS 구매횟수
  FROM  SALES_SUB_QUERY
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR;


/* 구매횟수 100회 미만 조건으로 필터링 */
SELECT  ADDR
      ,SUM(구매횟수) AS 구매횟수
  FROM  SALES_SUB_QUERY
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR
HAVING  SUM(구매횟수) < 100;


/* 모든 열 조회 */
/* 구매횟수가 낮은 순으로 */
SELECT  ADDR
      ,SUM(구매횟수) AS 구매횟수
  FROM  SALES_SUB_QUERY
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR
HAVING  SUM(구매횟수) < 100
 ORDER
    BY  SUM(구매횟수) ASC; 
 
 
/***************FROM절 서브 쿼리(Sub Query) 작성법***************/
SELECT  A.구매횟수
      ,B.*
  FROM  (
      SELECT  MEM_NO
            ,COUNT(ORDER_NO) AS 구매횟수
          FROM  SALES
       GROUP
            BY  MEM_NO
      )AS A
 INNER
  JOIN  CUSTOMER AS B
    ON  A.MEM_NO = B.MEM_NO;
```

> 1. SELECT절 서브 쿼리
>    - 테이블의 열 : 스칼라 서브 쿼리
>    - 처리속도가 JOIN보다 느림
> 2. FROM절 서브 쿼리 (JOIN과 함께 분석에서 가장 많이 사용됨)
>    - 테이블 : 열 이름 및 테이블명 지정
> 3. WHERE절 서브쿼리
>    - 리스트 형태로 사용







## SQL 문법 단원 정리

### 데이터 조회(SELECT)와 여러 절들

> 데이터 조회(SELECT)는 여러 절들과 함께 사용되어, 분석에 필요한 데이터를 조회함

- FROM : 테이블 확인
- WHERE : FROM절 테이블을 특정 조건으로 필터링
- GROUP BY : 열 별로 그룹화
- HAVING : 그룹화된 새로운 테이블을 특정 조건으로 필터링
- SELECT : 열 선택
- ORDER BY : 열 정렬



### 테이블 결합(JOIN) - INNER, LEFT, RIGHT JOIN

- Inner Join : 두 테이블의 공통 값이 매칭되는 데이터만 결합
- Left Join : 두 테이블의 공통 값이 매칭되는 데이터만 결합 + **왼쪽** 테이블의 매칭되지 않는 데이터는 NULL
- Right Join : 두 테이블의 공통 값이 매칭되는 데이터만 결합 + **오른쪽** 테이블의 매칭되지 않는 데이터는 NULL



### 서브 쿼리 (Sub Query)

- SELECT절 서브 쿼리
  - 테이블의 열 : 스칼라 서브 쿼리
  - 처리속도가 JOIN보다 느림

- FROM절 서브 쿼리 (JOIN과 함께 분석에서 가장 많이 사용됨)
  - 테이블 : 열 이름 및 테이블명 지정

- WHERE절 서브쿼리
  - 리스트 형태로 사용



```sql
USE PRACTICE;


/***************데이터 조회(SELECT)***************/

/* 1. CUSTOMER 테이블의 가입연도별 및 지역별 회원수를 조회하시오.*/
/* FROM절 / GROUP BY절 / SELECT절 / YEAR 및 COUNT 함수 활용 */

SELECT  YEAR(JOIN_DATE) AS 가입연도
        ,ADDR
        ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
 GROUP
    BY  YEAR(JOIN_DATE)
        ,ADDR;
  
  
/* 2. (1) 명령어에서 성별이 남성회원 조건을 추가한 뒤, 회원수가 50명 이상인 조건을 추가하시오.*/
/* WHERE절 / HAVING절 활용 */
  
SELECT  YEAR(JOIN_DATE) AS 가입연도
        ,ADDR
        ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
 WHERE  GENDER = 'MAN'
 GROUP
    BY  YEAR(JOIN_DATE)
        ,ADDR
HAVING  COUNT(MEM_NO) > 50;


/* 3. (2) 명령어에서 회원수를 내림차순으로 정렬하시오.*/
/* ORDER BY절 활용 */

SELECT  YEAR(JOIN_DATE) AS 가입연도
        ,ADDR
        ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
 WHERE  GENDER = 'MAN'
 GROUP
    BY  YEAR(JOIN_DATE)
        ,ADDR
HAVING  COUNT(MEM_NO) > 50
 ORDER
    BY  COUNT(MEM_NO) DESC;  
    
    
    
/***************데이터 조회(SELECT) + 테이블 결합(JOIN)***************/

/* 1. SALES 테이블 기준으로 PRODUCT 테이블을 LEFT JOIN 하시오.*/
/* LEFT JOIN 활용 */

SELECT  *
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE;

    
/* 2. (1)에서 결합된 테이블을 활용하여, 브랜드별 판매수량을 구하시오. */
/* GROUP BY절 / SUM 함수 활용*/

SELECT  B.BRAND
        ,SUM(SALES_QTY) AS 판매수량
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE
 GROUP
    BY  B.BRAND;  
    
    
/* 3. CUSTOMER 및 SALES 테이블을 활용하여, 회원가입만하고 주문이력이 없는 회원수를 구하시오.*/
/* LEFT JOIN 활용 */    

SELECT  COUNT(A.MEM_NO)
  FROM  CUSTOMER AS A
  LEFT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
 WHERE  B.MEM_NO IS NULL;



/***************데이터 조회(SELECT) + 테이블 결합(JOIN) + 서브 쿼리(Sub Query)***************/

/* 1. FROM절 서브쿼리를 활용하여, SALES 테이블의 PRODUCT_CODE별 판매수량을 구하시오.*/
/* FROM절 서브쿼리 / SUM 함수 활용 */

SELECT  *
  FROM  (
      SELECT  PRODUCT_CODE
              ,SUM(SALES_QTY) AS 판매수량
        FROM  SALES
       GROUP
          BY  PRODUCT_CODE
      )AS A;


/* 2. (1) 명령어를 활용하여, PRODUCT 테이블과 LEFT JOIN 하시오.*/
/* LEFT JOIN 활용 */

SELECT  *
  FROM  (
      SELECT  PRODUCT_CODE
              ,SUM(SALES_QTY) AS 판매수량
        FROM  SALES
       GROUP
          BY  PRODUCT_CODE
      )AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE;
        

/* 3. (2) 명령어를 활용하여, 카테고리 및 브랜드별 판매수량을 구하시오.*/
/* GROUP BY절 / SUM 함수 활용*/

SELECT  CATEGORY
        ,BRAND
        ,SUM(판매수량) AS 판매수량
  FROM  (
      SELECT  PRODUCT_CODE
              ,SUM(SALES_QTY) AS 판매수량
        FROM  SALES
       GROUP
          BY  PRODUCT_CODE
      )AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE
 GROUP
    BY  CATEGORY
        ,BRAND;
```

