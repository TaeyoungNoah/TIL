# SQL 활용



## 연산자 및 함수 

### 연산자

|      구분       | 연산자                                    | 설명                                                         |
| :-------------: | ----------------------------------------- | ------------------------------------------------------------ |
| **비교 연산자** | = / > / < / >= / <= / <>                  | 같음 / 보다 큼 / 보다 작음 / 크거나 같음 / 작거나 같음 / **같지 않음** |
| **논리 연산자** | AND / OR                                  | 앞,뒤조건모두만족 / 하나라도만족                             |
|                 | **NOT**                                   | 뒤에 오는 조건과 **반대**                                    |
| **특수 연산자** | BETWEEN a AND b / **NOT** BETWEEN a AND b | a와b의값사이 / a와b의값사이가 **아님**                       |
|                 | IN (List) / **NOT** IN (List)             | 리스트(List) 값 / 리스트(List) 값이 **아님**                 |
|                 | LIKE ‘비교문자열’                         | 비교문자열과 같음                                            |
|                 | **NOT** LIKE ‘비교문자열’                 | 비교문자열이 **아님**                                        |
|                 | IS NULL                                   | NULL과 같음                                                  |
|                 | IS **NOT** NULL                           | NULL이 **아님**                                              |
| **산술 연산자** | +,-,*,/                                   | 덧셈, 뺄셈, 곱셉, 나눗셈                                     |
| **집합 연산자** | UNION                                     | 2개 이상 테이블 중복된 행 제거 하여 집합(* 열 개수와 데이터 타입 일치) |
|                 | UNION ALL                                 | 2개 이상 테이블 중복된 행 제거 **없이** 집합(* 열 개수와 데이터 타입 일치) |



```sql
USE PRACTICE;

/****************************************************************************/
/************************************연산자***********************************/
/****************************************************************************/

/***************비교 연산자***************/

/* = : 같음 */
SELECT  *
  FROM  CUSTOMER
 WHERE  GENDER = 'MAN';
 
/* <> : 같지 않음 */ 
SELECT  *
  FROM  CUSTOMER
 WHERE  GENDER <> 'MAN';

/* >= : ~보다 크거나 같음 */  
SELECT  *
  FROM  CUSTOMER
 WHERE  YEAR(JOIN_DATE) >= 2020;

/* <= : ~보다 작거나 같음 */  
SELECT  *
  FROM  CUSTOMER
 WHERE  YEAR(JOIN_DATE) <= 2019;
 
/* > : ~보다 큼 */  
SELECT  *
  FROM  CUSTOMER
 WHERE  YEAR(JOIN_DATE) > 2019;
 
/* < : ~보다 작음 */  
SELECT  *
  FROM  CUSTOMER
 WHERE  YEAR(JOIN_DATE) < 2020;
 
/***************논리 연산자***************/

/* AND : 앞, 뒤 조건 모두 만족 */
SELECT  *
  FROM  CUSTOMER
 WHERE  GENDER = 'MAN'
   AND  ADDR = 'Gyeonggi';

/* NOT : 뒤에 오는 조건과 반대 */ 
SELECT  *
  FROM  CUSTOMER
 WHERE  NOT GENDER = 'MAN'
   AND  ADDR = 'Gyeonggi';

/* OR : 하나라도 만족 */    
SELECT  *
  FROM  CUSTOMER
 WHERE  GENDER = 'MAN'
    OR  ADDR = 'Gyeonggi';
    

/***************특수 연산자***************/  

/* BETWEEN a AND b : a와 b의 값 사이 */
SELECT  *
  FROM  CUSTOMER
 WHERE  YEAR(BIRTHDAY) BETWEEN 2010 AND 2011;

/* NOT BETWEEN a AND b : a와 b의 값 사이가 아님 */
SELECT  *
  FROM  CUSTOMER
 WHERE  YEAR(BIRTHDAY) NOT BETWEEN 1950 AND 2020;

/* IN (List) : 리스트 값 */ 
SELECT  *
  FROM  CUSTOMER
 WHERE  YEAR(BIRTHDAY) IN (2010,2011);
 
/* NOT IN (List) : 리스트 값이 아님 */  
SELECT  *
  FROM  CUSTOMER
 WHERE  YEAR(BIRTHDAY) NOT IN (2010, 2011);

/* LIKE ‘비교문자열’ */   
SELECT  *
  FROM  CUSTOMER
 WHERE  ADDR LIKE 'D%'; /* ~로 시작하는 */

SELECT  *
  FROM  CUSTOMER
 WHERE  ADDR LIKE '%N'; /* ~로 끝나는 */
 
SELECT  *
  FROM  CUSTOMER
 WHERE  ADDR LIKE '%EO%'; /* ~를 포함하는 */

/* NOT LIKE ‘비교문자열’ */   
SELECT  *
  FROM  CUSTOMER
 WHERE  ADDR NOT LIKE '%EO%'; /* ~를 제외하는 */ 
 
/* IS NULL : NULL */   
SELECT  *
  FROM  CUSTOMER AS A
  LEFT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
 WHERE  B.MEM_NO IS NULL;
 
/* 확인 */ 
SELECT  *
  FROM  SALES
 WHERE  MEM_NO = '1001736';
 
/* IS NOT NULL : NOT NULL */   
SELECT  *
  FROM  CUSTOMER AS A
  LEFT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
 WHERE  B.MEM_NO IS NOT NULL;
 
 
/***************산술 연산자***************/
  
SELECT  *
	    	,A.SALES_QTY * PRICE AS 결제금액
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE;
    
    
 
  
  
```





### 집합 연산자 - UNION, UNION ALL

- UNION : 2개 이상 테이블의 중복된 행들을 제거하여 집합

- UNION ALL : 2개 이상 테이블의 중복된 행들을 제거 **없이** 집합

  > 집합 연산자는 **테이블들의 열 개수와 데이터 타입이 일치할 때 사용 가능**

```mysql
/***************집합 연산자***************/
 
CREATE TEMPORARY TABLE SALES_2019
SELECT  *
  FROM  SALES
 WHERE  YEAR(ORDER_DATE) = '2019';
 
/* 1235행 */
SELECT  *
  FROM  SALES_2019;

/* 3115행 */
SELECT  *
  FROM  SALES;

/* UNION : 2개 이상 테이블 중복된 행 제거 하여 집합(* 열 개수와 데이터 타입 일치) */
SELECT  *
  FROM  SALES_2019
 UNION
SELECT  *
  FROM  SALES;

/* UNION ALL: 2개 이상 테이블 중복된 행 제거 없이 집합(* 열 개수와 데이터 타입 일치) */
SELECT 3115 + 1235;

SELECT  *
  FROM  SALES_2019
UNION ALL
SELECT  *
  FROM  SALES;
```







### 함수

> 함수는 단일 및 복수 행 그리고 윈도우 함수로 나뉘며, **특정 규칙**에 의해 새로운 결과값으로 반환하는 명령어입니다.

- SQL 함수

  - 단일 행 함수

    > 모든 행에 대하여 **각각** 함수가 적용되어 반환

  - 복수 행 함수

    > 여러 행들이 **하나의 결과값**으로 반환

  - 윈도우 함수

    > **행과 행간의 관계를 정의**하여 결과 값을 반환



#### 단일 행 함수

> 모든 행에 대하여 **각각** 함수가 적용되어 반환

|      구분       | 함수                                                         | 설명                                 |
| :-------------: | ------------------------------------------------------------ | ------------------------------------ |
| **숫자형 함수** | ABS(숫자)                                                    | 절대값 반환                          |
|                 | ROUND(숫자, N)                                               | N 기준으로 반올림 값 반환            |
|                 | SQRT(숫자)                                                   | 제곱근 값 반환                       |
| **문자형 함수** | LOWER(문자) / UPPER(문자)                                    | 소문자 / 대문자 반환                 |
|                 | LEFT(문자, N) / RIGHT(문자, N)                               | 왼쪽 / 오른쪽부터 N만큼 반환         |
|                 | LENGTH(문자)                                                 | 문자수 반환                          |
| **날짜형 함수** | YEAR / MONTH / DAY(날짜)                                     | 연 / 월 / 일 반환                    |
|                 | DATE_ADD(날짜, INTERVAL)                                     | INTERVAL만큼 더한 값 반환            |
|                 | DATEDIFF(날짜a, 날짜b)                                       | 날짜a - 날짜b 일수 반환              |
| **형변환 함수** | DATE_FORMAT(날짜, 형식)                                      | 날짜형식으로 변환                    |
|                 | CAST(형식a, 형식b)                                           | 형식a를 형식b로 변환                 |
|  **일반 함수**  | IFNULL(A, B)                                                 | A가 NULL이면 B를 반환, 아니면 A 반환 |
|                 | CASE WHEN [조건1] THEN [반환1] <br />          WHEN [조건2] THEN [반환2] <br />             ELSE [나머지] END | 여러 조건별로 반환값 지정            |

```mysql
/***************숫자형 함수***************/

/* ABS(숫자) : 절대값 반환 */
SELECT ABS(- 200); 

/* ROUND(숫자, N) : N 기준으로 반올림 값 반환 */
SELECT ROUND(2.18, 1); 

/* SQRT(숫자) : 제곱근 값 반환 */
SELECT SQRT(9);
 

/***************문자형 함수***************/

/* LOWER(문자) / UPPER(문자) : 소문자 / 대문자 반환 */
SELECT LOWER('AB');
SELECT UPPER('ab');

/* LEFT(문자, N) / RIGHT(문자, N) : 왼쪽 / 오른쪽부터 N만큼 반환 */
SELECT LEFT('AB', 1); 
SELECT RIGHT('AB', 1); 

/* LENGTH(문자) : 문자수 반환 */
SELECT LENGTH('AB');


/***************날짜형 함수***************/

/* YEAR / MONTH / DAY(날짜) : 연 / 월 / 일 반환 */
SELECT YEAR('2022-12-31');
SELECT MONTH('2022-12-31');
SELECT DAY('2022-12-31');

/* DATE_ADD(날짜, INTERVAL) : INTERVAL만큼 더한 값 반환 */
SELECT DATE_ADD('2022-12-31', INTERVAL -1 MONTH);

/* DATEDIFF(날짜a, 날짜b) : 날짜a – 날짜b 일수 반환 */
SELECT DATEDIFF('2022-12-31', '2022-12-1');


/***************형변환 함수***************/

/* DATE_FORMAT(날짜, 형식) : 날짜형식으로 변환 */
SELECT DATE_FORMAT('2022-12-31', '%m-%d-%y');
SELECT DATE_FORMAT('2022-12-31', '%M-%D-%Y');

/* CAST(형식a, 형식b) : 형식a를 형식b로 변환 */
SELECT CAST('2022-12-31 12:00:00' AS DATE);


/***************일반 함수***************/

/* IFNULL(A, B) : A가 NULL이면 B를 반환, 아니면 A 반환 */
SELECT IFNULL(NULL, 0);
SELECT IFNULL('NULL이 아님', 0);

/* 
CASE WHEN [조건1] THEN [반환1]
 	   WHEN [조건2] THEN [반환2]
     ELSE [나머지] END
: 여러 조건별로 반환값 지정
*/
SELECT  *
	    	,CASE WHEN GENDER = 'MAN' THEN '남성'
              ELSE '여성' END
  FROM  CUSTOMER;
  
  
/***************함수 중첩 사용***************/
  
SELECT  *
	    	,YEAR(JOIN_DATE) AS 가입연도
        ,LENGTH( YEAR(JOIN_DATE) ) AS 가입연도_문자수
  FROM  CUSTOMER;
  
  

```



#### 복수 행 함수

> - 여러 행들이 **하나의 결과값**으로 반환
> - 주로 **GROUP BY 절과 함께** 사용됨

|     구분     | 함수                          | 설명                                       |
| :----------: | ----------------------------- | ------------------------------------------ |
| **집계함수** | COUNT<br/> \* COUNT(DISTINCT) | 행수<br/> \* 행수(중복제거)                |
|              | SUM                           | 합계                                       |
|              | AVG                           | 평균                                       |
|              | MAX / MIN                     | 최대 / 최소                                |
| **그룹함수** | WITH ROLLUP                   | GROUP BY 열들을 오른쪽에서 왼쪽순으로 그룹 |

```mysql
/***************집계 함수***************/

SELECT  COUNT(ORDER_NO) AS 구매횟수 /* 행수 */
	    	,COUNT(DISTINCT MEM_NO) AS 구매자수 /* 중복제거된 행수  */
        ,SUM(SALES_QTY) AS 구매수량 /* 합계 */
        ,AVG(SALES_QTY) AS 평균구매수량 /* 평균 */
        ,MAX(ORDER_DATE) AS 최근구매일자 /* 최대 */
        ,MIN(ORDER_DATE) AS 최초구매일자 /* 최소 */
  FROM  SALES;
    
/* DISTINCT: 중복제거 */   

 
/***************그룹 함수***************/

/* WITH ROLLUP : GROUP BY 열들을 오른쪽에서 왼쪽순으로 그룹 (소계, 합계)*/
SELECT  YEAR(JOIN_DATE) AS 가입연도
	    	,ADDR
        ,COUNT(MEM_NO) AS 회원수
  FROM  CUSTOMER
 GROUP
    BY  YEAR(JOIN_DATE)
    		,ADDR
WITH ROLLUP;


/***************집계 함수 + GROUP BY***************/

SELECT  MEM_NO
        ,SUM(SALES_QTY) AS 구매수량
  FROM  SALES
 GROUP
    BY  MEM_NO;
    
/* 확인 */
SELECT  *
  FROM  SALES
 WHERE  MEM_NO = '1000970';
```





#### 윈도우 함수

> - **행과 행간의 관계를 정의**하여 결과 값을 반환
> - ORDER BY로 행과 행간의 순서를 정하며, PARTITION BY로 그룹화가 가능

|     구분      | 함수       | 설명                                                         |
| :-----------: | ---------- | ------------------------------------------------------------ |
| **순위 함수** | ROW_NUMBER | 고유한 순위 반환 (1,2,3,**4**,**5** ...)                     |
|               | RANK       | 동일한 값이면 동일한 순위 반환 (1,2,3,3,**5** ...)           |
|               | DENSE_RANK | 동일한 값이면 동일한 순위 반환 **(+ 하나의 등수로) **(1,2,3,3,**4** ...) |
| **집계 함수** | COUNT      | 누적 행수                                                    |
|               | SUM        | 누적 합계                                                    |
|               | AVG        | 누적 평균                                                    |
|               | MAX / MIN  | 누적 최대 / 최소                                             |

```mysql
/***************순위 함수***************/ 
/* ROW_NUMBER: 동일한 값이라도 고유한 순위 반환 (1,2,3,4,5...) */
/* RANK: 동일한 값이면 동일한 순위 반환 (1,2,3,3,5...) */
/* DENSE_RANK: 동일한 값이면 동일한 순위 반환(+ 하나의 등수로 취급) (1,2,3,3,4...) */

SELECT  ORDER_DATE
	    	,ROW_NUMBER() OVER (ORDER BY ORDER_DATE ASC) AS 고유한_순위_반환
        ,RANK() 	    OVER (ORDER BY ORDER_DATE ASC) AS 동일한_순위_반환
        ,DENSE_RANK() OVER (ORDER BY ORDER_DATE ASC) AS 동일한_순위_반환_하나의등수
  FROM  SALES;

/* 순위 함수+ PARTITION BY: 그룹별 순위 */

SELECT  MEM_NO
		    ,ORDER_DATE
    		,ROW_NUMBER() OVER (PARTITION BY MEM_NO ORDER BY ORDER_DATE ASC) AS 고유한_순위_반환
        ,RANK() 	    OVER (PARTITION BY MEM_NO ORDER BY ORDER_DATE ASC) AS 동일한_순위_반환
        ,DENSE_RANK() OVER (PARTITION BY MEM_NO ORDER BY ORDER_DATE ASC) AS 동일한_순위_반환_하나의등수
  FROM  SALES;

/***************집계 함수(누적)***************/ 

SELECT  ORDER_DATE
	    	,SALES_QTY
        ,'-' AS 구분
        ,COUNT(ORDER_NO) OVER (ORDER BY ORDER_DATE ASC) AS 누적_구매횟수
    		,SUM(SALES_QTY)  OVER (ORDER BY ORDER_DATE ASC) AS 누적_구매수량
        ,AVG(SALES_QTY)  OVER (ORDER BY ORDER_DATE ASC) AS 누적_평균구매수량
        ,MAX(SALES_QTY)  OVER (ORDER BY ORDER_DATE ASC) AS 누적_가장높은구매수량
	    	,MIN(SALES_QTY)  OVER (ORDER BY ORDER_DATE ASC) AS 누적_가장낮은구매수량    
  FROM  SALES;
  
/* 집계 함수(누적)+ PARTITION BY: 그룹별 집계 함수(누적) */

SELECT  MEM_NO
	    	,ORDER_DATE
    		,SALES_QTY
        ,'-' AS 구분
        ,COUNT(ORDER_NO) OVER (PARTITION BY MEM_NO ORDER BY ORDER_DATE ASC) AS 누적_구매횟수        
    		,SUM(SALES_QTY)  OVER (PARTITION BY MEM_NO ORDER BY ORDER_DATE ASC) AS 누적_구매수량
        ,AVG(SALES_QTY)  OVER (PARTITION BY MEM_NO ORDER BY ORDER_DATE ASC) AS 누적_평균구매수량
        ,MAX(SALES_QTY)  OVER (PARTITION BY MEM_NO ORDER BY ORDER_DATE ASC) AS 누적_가장높은구매수량
	    	,MIN(SALES_QTY)  OVER (PARTITION BY MEM_NO ORDER BY ORDER_DATE ASC) AS 누적_가장낮은구매수량       
  FROM  SALES;
```







## View 및 Procedure

### View

> - View (가상 테이블) - View는 하나 이상의 테이블을 활용하여, 사용자가 정의한 **가상 테이블**
> - **JOIN의 사용을 최소화**하여, 편의성을 최대화함
> - View 테이블은 **가상테이블** 이기 때문에, **중복되는 열이 저장될 수 없음**

```mysql
USE PRACTICE;

/***************테이블 결합***************/
/* 주문(Sales) 테이블 기준, 상품(Product) 테이블 LEFT JOIN 결합 */

SELECT  A.*
        ,A.SALES_QTY * B.PRICE AS 결제금액
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE;


/***************VIEW 생성***************/
    
CREATE VIEW SALES_PRODUCT AS
SELECT  A.*
        ,A.SALES_QTY * B.PRICE AS 결제금액
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE;


/***************VIEW 실행***************/

SELECT  *
  FROM  SALES_PRODUCT;


/***************VIEW 수정***************/
  
ALTER VIEW SALES_PRODUCT AS
SELECT  A.*
        ,A.SALES_QTY * B.PRICE * 1.1 AS 결제금액_수수료포함
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE;  
    
/* 확인 */    
SELECT  *
  FROM  SALES_PRODUCT;   
  

/***************VIEW 삭제***************/

DROP VIEW SALES_PRODUCT;
    

/***************VIEW 특징(중복되는 열 저장 안됨, 아래는 에러)***************/
       
CREATE VIEW SALES_PRODUCT AS
SELECT  *
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE;
```





### Procedure

> - **매개변수를 활용**해, 사용자가 정의한 작업을 저장함
> - Procedure의 매개변수
>   - IN : 매개변수를 프로시저로 전달
>   - OUT : 프로시저 결과값 반환
>   - INOUT : 매개변수를 프로시저로 전달 + 프로시저 결과값 반환

```mysql
/***************IN 매개변수***************/    

DELIMITER //
CREATE PROCEDURE CST_GEN_ADDR_IN( IN INPUT_A VARCHAR(20), INPUT_B VARCHAR(20) )
BEGIN
   SELECT  *
     FROM  CUSTOMER
    WHERE  GENDER = INPUT_A
      AND  ADDR = INPUT_B;
END //
DELIMITER ;

/* DELIMITER: 여러 명령어들을 하나로 묶어줄때 사용 */

/***************PROCEDURE 실행***************/
    
CALL CST_GEN_ADDR_IN('MAN', 'SEOUL');

CALL CST_GEN_ADDR_IN('WOMEN', 'INCHEON');


/***************PROCEDURE 삭제***************/
    
DROP PROCEDURE CST_GEN_ADDR_IN;


/**************OUT 매개변수***************/    

DELIMITER //
CREATE PROCEDURE CST_GEN_ADDR_IN_CNT_MEM_OUT( IN INPUT_A VARCHAR(20), INPUT_B VARCHAR(20), OUT CNT_MEM INT )
BEGIN
   SELECT  COUNT(MEM_NO)
     INTO  CNT_MEM
     FROM  CUSTOMER
    WHERE  GENDER = INPUT_A
      AND  ADDR = INPUT_B;
END //
DELIMITER ;


/***************PROCEDURE 실행***************/
    
CALL CST_GEN_ADDR_IN_CNT_MEM_OUT('WOMEN', 'INCHEON', @CNT_MEM);
SELECT  @CNT_MEM;


/**************IN/OUT 매개변수***************/    

DELIMITER //
CREATE PROCEDURE IN_OUT_PARAMETER( INOUT COUNT INT)
BEGIN
   SET COUNT = COUNT + 10;
END //
DELIMITER ;


/***************PROCEDURE 실행***************/

SET @counter = 1;
CALL IN_OUT_PARAMETER(@counter);
SELECT  @counter;
```





## 데이터마트

> - **분석에 필요한 데이터**를 가공한 분석용 데이터
>
>   - 요약 변수 : 수집된 데이터를 분석에 맞게 종합한 변수 (**기간별 구매 금액, 횟수, 수량 등**)
>   - 파생 변수 : 사용자가 특정 조건 또는 함수로 의미를 부여한 변수 (**연령대, 선호 카테고리 등**)
>
> - 데이터 마트를 사용할 때 데이터 정합성을 체크해야함
>
>   > 데이터 정합성 : 데이터가 서로 무손 없이 **일관되게 일치함**을 나타낼 때 사용
>
>   - 데이터 마트 회원 수의 중복은 없는가
>   - 데이터 마트의 요약 및 파생 변수의 오류는 없는가
>   - 데이터 마트의 구매자 비중(%)의 오류는 없는가

```mysql
USE PRACTICE;

/****************************************************************************/
/*****************************회원 분석용 데이터 마트******************************/
/****************************************************************************/
/***************회원 구매정보***************/

/* 회원 구매정보 */
SELECT  A.MEM_NO, A.GENDER, A.BIRTHDAY, A.ADDR, A.JOIN_DATE
      ,SUM(B.SALES_QTY * C.PRICE) AS 구매금액
        ,COUNT(B.ORDER_NO)          AS 구매횟수
        ,SUM(B.SALES_QTY)         AS 구매수량
  FROM  CUSTOMER AS A
  LEFT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
  LEFT
  JOIN  PRODUCT AS C
    ON  B.PRODUCT_CODE = C.PRODUCT_CODE
 GROUP
    BY  A.MEM_NO, A.GENDER, A.BIRTHDAY, A.ADDR, A.JOIN_DATE;

/* 회원 구매정보 임시테이블 */
CREATE TEMPORARY TABLE CUSTOMER_PUR_INFO AS
SELECT  A.MEM_NO, A.GENDER, A.BIRTHDAY, A.ADDR, A.JOIN_DATE
      ,SUM(B.SALES_QTY * C.PRICE) AS 구매금액
        ,COUNT(B.ORDER_NO)          AS 구매횟수
        ,SUM(B.SALES_QTY)         AS 구매수량
  FROM  CUSTOMER AS A
  LEFT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
  LEFT
  JOIN  PRODUCT AS C
    ON  B.PRODUCT_CODE = C.PRODUCT_CODE
 GROUP
    BY  A.MEM_NO, A.GENDER, A.BIRTHDAY, A.ADDR, A.JOIN_DATE;
 
/* 확인 */ 
SELECT  * FROM CUSTOMER_PUR_INFO;
 

/***************회원 연령대***************/
 
/* 생년월일 -> 나이 */
SELECT  *
      ,2021-YEAR(BIRTHDAY) + 1 AS 나이
  FROM  CUSTOMER;

/* 생년월일 -> 나이 -> 연령대 */
SELECT  *
      ,CASE WHEN 나이 < 10 THEN '10대 미만'
           WHEN 나이 < 20 THEN '10대'
              WHEN 나이 < 30 THEN '20대'
              WHEN 나이 < 40 THEN '30대'
              WHEN 나이 < 50 THEN '40대'
              ELSE '50대 이상' END AS 연령대         
  FROM  (
      SELECT  *
            ,2021-YEAR(BIRTHDAY) +1 AS 나이
        FROM  CUSTOMER
      )AS A;

/* CASE WHEN 함수 사용시 주의점(위에서부터 거르기 때문에 아래처럼 적으면 원하는 결과 X) */
SELECT  *
      ,CASE WHEN 나이 < 50 THEN '40대'
           WHEN 나이 < 10 THEN '10대 미만'
              WHEN 나이 < 20 THEN '10대'
              WHEN 나이 < 30 THEN '20대'
              WHEN 나이 < 40 THEN '30대'
              ELSE '50대 이상' END AS 연령대         
  FROM  (
      SELECT  *
            ,2021-YEAR(BIRTHDAY) +1 AS 나이
        FROM  CUSTOMER
      )AS A; 
 
/* 회원 연령대 임시테이블 */
CREATE TEMPORARY TABLE CUSTOMER_AGEBAND AS
SELECT  A.*
      ,CASE WHEN 나이 < 10 THEN '10대 미만'
           WHEN 나이 < 20 THEN '10대'
              WHEN 나이 < 30 THEN '20대'
              WHEN 나이 < 40 THEN '30대'
              WHEN 나이 < 50 THEN '40대'
              ELSE '50대 이상' END AS 연령대         
  FROM  (
      SELECT  *
            ,2021-YEAR(BIRTHDAY) + 1 AS 나이
        FROM  CUSTOMER
      )AS A;

/* 확인 */ 
SELECT  * FROM CUSTOMER_AGEBAND;


/***************회원 구매정보 + 연령대 임시테이블***************/
CREATE TEMPORARY TABLE CUSTOMER_PUR_INFO_AGEBAND AS
SELECT  A.*
      ,B.연령대
  FROM  CUSTOMER_PUR_INFO AS A
  LEFT
  JOIN  CUSTOMER_AGEBAND AS B
    ON  A.MEM_NO = B.MEM_NO;

/* 확인 */ 
SELECT  * FROM CUSTOMER_PUR_INFO_AGEBAND;


/***************회원 선호 카테고리***************/

/* 회원 및 카테고리별 구매횟수 순위 */
SELECT  A.MEM_NO
       ,B.CATEGORY
       ,COUNT(A.ORDER_NO) AS 구매횟수
       ,ROW_NUMBER() OVER(PARTITION BY A.MEM_NO ORDER BY COUNT(A.ORDER_NO) DESC) AS 구매횟수_순위
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE   
 GROUP
    BY  A.MEM_NO
       ,B.CATEGORY;

/* 회원 및 카테고리별 구매횟수 순위 + 구매횟수 순위 1위만 필터링 */
SELECT  *
  FROM  (
      SELECT  A.MEM_NO
            ,B.CATEGORY
            ,COUNT(A.ORDER_NO) AS 구매횟수
            ,ROW_NUMBER() OVER(PARTITION BY A.MEM_NO ORDER BY COUNT(A.ORDER_NO) DESC) AS 구매횟수_순위
        FROM  SALES AS A
        LEFT
        JOIN  PRODUCT AS B
         ON  A.PRODUCT_CODE = B.PRODUCT_CODE
       GROUP
         BY  A.MEM_NO
            ,B.CATEGORY
      )AS A
 WHERE  구매횟수_순위 = 1;

/* 회원 선호 카테고리 임시테이블 */
CREATE TEMPORARY TABLE CUSTOMER_PRE_CATEGORY AS
SELECT  *
  FROM  (
      SELECT  A.MEM_NO
            ,B.CATEGORY
            ,COUNT(A.ORDER_NO) AS 구매횟수
            ,ROW_NUMBER() OVER(PARTITION BY A.MEM_NO ORDER BY COUNT(A.ORDER_NO) DESC) AS 구매횟수_순위
        FROM  SALES AS A
        LEFT
        JOIN  PRODUCT AS B
         ON  A.PRODUCT_CODE = B.PRODUCT_CODE
       GROUP
         BY  A.MEM_NO
            ,B.CATEGORY
      )AS A
 WHERE  구매횟수_순위 = 1;

/* 확인 */ 
SELECT  * FROM CUSTOMER_PRE_CATEGORY;


/***************회원 구매정보 + 연령대 + 선호 카테고리 임시테이블***************/
CREATE TEMPORARY TABLE CUSTOMER_PUR_INFO_AGEBAND_PRE_CATEGORY AS
SELECT  A.*
      ,B.CATEGORY AS PRE_CATEGORY
  FROM  CUSTOMER_PUR_INFO_AGEBAND AS A
  LEFT
  JOIN  CUSTOMER_PRE_CATEGORY AS B
    ON  A.MEM_NO = B.MEM_NO;

/* 확인 */ 
SELECT  * FROM CUSTOMER_PUR_INFO_AGEBAND_PRE_CATEGORY;


/***************회원 분석용 데이터 마트 생성(회원 구매정보 + 연령대 + 선호 카테고리 임시테이블)***************/
CREATE TABLE CUSTOMER_MART AS
SELECT  *
  FROM  CUSTOMER_PUR_INFO_AGEBAND_PRE_CATEGORY;
   
/* 확인 */    
SELECT  *
  FROM  CUSTOMER_MART;



/***************데이터 마트 회원 수의 중복은 없는가?***************/

SELECT  *
  FROM  CUSTOMER_MART;

SELECT  COUNT(MEM_NO)
      ,COUNT(DISTINCT MEM_NO)
  FROM  CUSTOMER_MART;


/***************데이터 마트의 요약 및 파생변수의 오류는 없는가?***************/
  
SELECT  *
  FROM  CUSTOMER_MART;  

/* 회원(1000005)의 구매정보 */
/* 구매금액: 408000 / 구매횟수: 3 구매수량: 14 */

SELECT  SUM(A.SALES_QTY * B.PRICE) AS 구매금액
        ,COUNT(A.ORDER_NO)         AS 구매횟수
        ,SUM(A.SALES_QTY)         AS 구매수량
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE
 WHERE  MEM_NO = '1000005';

/* 회원(1000005)의 선호 카테고리 */
/* PRE_CATEGORY: home */
SELECT  *
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE
 WHERE  MEM_NO = '1000005';
 
 
/***************데이터 마트의 구매자 비중(%)의 오류는 없는가?***************/

/* 회원(Customer) 테이블 기준, 주문(Sales) 테이블 구매 회원번호 LEFT JOIN 결합 */
SELECT  *
  FROM  CUSTOMER AS A
  LEFT
  JOIN  (
      SELECT  DISTINCT MEM_NO
          FROM  SALES
      )AS B
    ON  A.MEM_NO = B.MEM_NO;
   
/* 회원(Customer) 테이블 기준, 주문(Sales) 테이블 구매 회원번호 LEFT JOIN 결합 */
/* 구매여부 추가 */
SELECT  *
      ,CASE WHEN B.MEM_NO IS NOT NULL THEN '구매'
           ELSE '미구매' END AS 구매여부
  FROM  CUSTOMER AS A
  LEFT
  JOIN  (
      SELECT  DISTINCT MEM_NO
          FROM  SALES
      )AS B
    ON  A.MEM_NO = B.MEM_NO;
 
/* 구매여부별, 회원수 */
SELECT  구매여부
      ,COUNT(MEM_NO) AS 회원수
  FROM  (
      SELECT  A.*
            ,CASE WHEN B.MEM_NO IS NOT NULL THEN '구매'
                 ELSE '미구매' END AS 구매여부
        FROM  CUSTOMER AS A
        LEFT
        JOIN  (
            SELECT  DISTINCT MEM_NO
              FROM  SALES
            )AS B
         ON  A.MEM_NO = B.MEM_NO
      )AS A
 GROUP
    BY  구매여부; 
 
/* 확인(미구매: 1459 / 구매: 1202) */ 
SELECT  *
  FROM  CUSTOMER_MART
 WHERE  구매금액 IS NULL;
 
SELECT  *
  FROM  CUSTOMER_MART
 WHERE  구매금액 IS NOT NULL; 
```

