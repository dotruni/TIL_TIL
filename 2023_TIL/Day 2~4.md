LeetCode 1731. The Number of Employees Which Report to Each Employee

```sql
SELECT e2.employee_id
      ,e2.name
      ,COUNT(e1.employee_id) AS reports_count 
      ,ROUND(AVG(e1.age),0) AS average_age
FROM Employees e1
JOIN Employees e2 ON e1.reports_to=e2.employee_id
GROUP BY 1
ORDER BY 1 
```

1789. Primary Department for Each Employee
```sql
SELECT employee_id
      ,department_id
FROM Employee
WHERE (primary_flag='Y') 

UNION 

SELECT employee_id
      ,department_id
FROM Employee
GROUP BY employee_id 
HAVING COUNT(department_id)=1 
```
- 중복된 test case가 있을 수 있음으로 UNION ALL을 쓰면 안된다
- HAVING 절에 COUNT를 SELECT절에 쓰지 않아도 SQL 처리 순서 상 문제가 없다.

1907. Count Salary Categories

처음 풀이 
```sql
SELECT(CASE WHEN income <20000 THEN 'Low Salary' 
            WHEN income >=20000 AND income <50000 THEN 'Average Salary'
            WHEN income >=50000 THEN 'High Salary' 
            ELSE NULL END
      )AS category
      ,COUNT(account_id) AS accounts_count        
FROM Accounts
GROUP BY category
```
Try running the inner query and see the output.
As there is no account_id with salary as Average Salary, the inner query will not return 'Average Salary' as a category. So, when you group by category in the outer query, there is no such category as 'Average Salary' to result in output.

0인 카테고리를 포함하려면 해당 CASE 절을 쓰지 못한다. 

두번째 풀이 
```sql
WITH CTE AS(
SELECT account_id
      ,(CASE WHEN income <20000 THEN 'Low Salary' 
             WHEN income >50000 THEN 'High Salary' 
             ELSE 'Average Salary' END
      )AS category
FROM Accounts
)
,CTE_T AS(
SELECT 'Low Salary' AS Category
UNION
SELECT 'High Salary' AS Category
UNION
SELECT 'Average Salary' AS Category       

)
SELECT CTE_T.Category AS category
      ,COUNT(account_id) AS accounts_count
FROM CTE_T
LEFT JOIN CTE ON CTE.category=CTE_T.Category  
GROUP BY CTE_T.Category
```
https://chobopark.tistory.com/117

inclusive range
- And an inclusive range is one where the limits are included along with what lies in between: a survey of “20-40 year-olds, inclusive” tells us 20 and 40 year-olds were counted, too.



프로그래머스 
년, 월, 성별 별 상품 구매 회원 수 구하기
https://school.programmers.co.kr/learn/courses/30/lessons/131532
- 처음에 틀린 답을 내놓았다.. 그 이유는, 년,월,성별 별 판매 회원수를 구해야하는데 DISTINCT 를 빼먹은 것..!
- 고유한 값을 원하는지, 중복도 허용하는지 잊지 말고 적용하자 
```sql
SELECT YEAR(OS.SALES_DATE) AS YEAR
      ,MONTH(OS.SALES_DATE) AS MONTH
      ,U.GENDER 
      ,COUNT(DISTINCT OS.USER_ID) AS USERS
FROM ONLINE_SALE OS 
JOIN USER_INFO U ON U.USER_ID=OS.USER_ID 
WHERE U.GENDER IS NOT NULL
GROUP BY 1,2,3
ORDER BY 1,2,3 ASC
```

GROUP BY를 했는데, COUNT가 0인 경우도 포함해줘야 하는 경우 다양한 대처법

### [프로그래머스 입양 시각 구하기(2)](https://school.programmers.co.kr/learn/courses/30/lessons/59413)

-   처음 풀이
    
    ```
    SELECT HOUR(DATETIME) AS HOUR
        ,COUNT(DISTINCT ANIMAL_ID) AS COUNT
    FROM ANIMAL_OUTS
    GROUP BY 1 
    ORDER BY 1
    ```
    

-   여기서 문제는 0~23시까지, 카운트가 없는 HOUR도 테이블을 만들어 줘야 한다는 것...!

그러면 아 0~24까지의 HOUR 칼럼을 만들어야 겠네..! 라는 생각이 든다

1.  이 경우 아래 리트코드 문제 처럼 노가다로 UNION ~ 으로 23까지 숫자를 만드는 노가다도 할 수 있겠지만..Python이 아니라서 반복문 못쓴다고 해도 다른 방법이 있지 않을까..? 하는 생각이 들었다.
2.  SET 변수 쓰기

### SET 이란..?

SET¶  
SET 문은 시스템 파라미터의 값을 지정하거나 사용자 정의 변수의 값을 지정하는 구문이다.  
[출처](https://www.cubrid.org/manual/ko/10.2/sql/query/set.html)

```
SET @HOUR := -1; 
-- HOUR라는 사용자 정의 변수를 할당한다. 
-- 왜 -1로 먼저 정의하는가? 값을 0부터 만들어주고 싶기 때문에 

SELECT (@HOUR := @HOUR +1) AS HOUR
-- 1씩 더해준다. 
     ,(SELECT COUNT(DISTINCT ANIMAL_ID)
       FROM ANIMAL_OUTS
       WHERE HOUR(DATETIME)=@HOUR 
       ) AS COUNT
FROM ANIMAL_OUTS
WHERE @HOUR < 23
-- @HOUR <23 로 조건절을 끊는 이유는 해당 22일 때 까지 1씩 더해주기 때문 
```

[참고, \[프로그래머스\] 입양 시각 구하기(1), (2) (GROUP BY, HAVING, SET)](https://chanhuiseok.github.io/posts/db-6/)

[프로그래머스 특정 기간동안 대여 가능한 자동차들의 대여비용 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/157339)


- 첫번째 풀이
```SQL
SELECT C.CAR_ID
      ,C.CAR_TYPE
      ,ROUND(((C.DAILY_FEE*30)*(100-DP.DISCOUNT_RATE)/100)) AS FEE
FROM CAR_RENTAL_COMPANY_CAR C
JOIN CAR_RENTAL_COMPANY_RENTAL_HISTORY RH ON RH.CAR_ID=C.CAR_ID 
JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN DP ON C.CAR_TYPE=DP.CAR_TYPE 
     AND (DP.DURATION_TYPE='30일 이상') 
WHERE C.CAR_TYPE IN ('세단','SUV') 
AND (RH.START_DATE NOT BETWEEN '2022-11-01' AND '2022-11-30')
AND (RH.END_DATE NOT BETWEEN '2022-11-01' AND '2022-11-30')
AND ((C.DAILY_FEE*30)*(100-DP.DISCOUNT_RATE)/100) BETWEEN 500000 AND 2000000
GROUP BY C.CAR_ID
ORDER BY FEE DESC, C.CAR_TYPE ASC, C.CAR_ID

-- 2022년 11월 1일부터 2022년 11월 30일까지 대여 가능하고
-- START DATE와 END DATE 사이에 겹치는 날짜 중에 '2022-11-01' AND '2022-11-30'가 없다 

```
- 이렇게 중복되는 행이 많게 작성했다면 GROUP BY를 해주어야 한다..!
- BETWEENM 막쓰면 안된다 왜냐하면 둘다 포함 관계이기 때문에 이상 미만인 문제 등에서는 만족시키지를 못한다.
- "START DATE와 END DATE 사이에 겹치는 날짜 중에 '2022-11-01' AND '2022-11-30'가 없다"를
```
AND (RH.START_DATE NOT BETWEEN '2022-11-01' AND '2022-11-30')
AND (RH.END_DATE NOT BETWEEN '2022-11-01' AND '2022-11-30')
```
는 만족시키지 못한다 왜냐하면 START_DATE : 2022-09-30, END_DATE : 2022-12-02 인 경우도 포함된다. 
- ACTION ITEM : 확실하지 않을때는 날짜 칼럼이랑 같이 보자!

- 두번째 풀이

```SQL
SELECT  CAR.CAR_ID,
        CAR.CAR_TYPE,
        ROUND(CAR.DAILY_FEE * 30 * (100 - PLAN.DISCOUNT_RATE)/100) AS FEE
  FROM  CAR_RENTAL_COMPANY_CAR AS CAR
  JOIN  CAR_RENTAL_COMPANY_DISCOUNT_PLAN AS PLAN
    ON  CAR.CAR_TYPE = PLAN.CAR_TYPE 
        AND PLAN.DURATION_TYPE = '30일 이상'
 WHERE  1=1 
        AND CAR.CAR_TYPE IN ('세단', 'SUV')
        AND CAR.CAR_ID NOT IN (
        SELECT CAR_ID
        FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
        WHERE START_DATE <= '2022-11-30' AND END_DATE >= '2022-11-01'
        )
HAVING  FEE >= 500000 AND FEE < 2000000
 ORDER 
    BY  FEE DESC
        , CAR.CAR_TYPE ASC
        , CAR.CAR_ID DESC;
```
[참고풀이](https://kkw-da.tistory.com/entry/SQL-%ED%8A%B9%EC%A0%95-%EA%B8%B0%EA%B0%84%EB%8F%99%EC%95%88-%EB%8C%80%EC%97%AC-%EA%B0%80%EB%8A%A5%ED%95%9C-%EC%9E%90%EB%8F%99%EC%B0%A8%EB%93%A4%EC%9D%98-%EB%8C%80%EC%97%AC%EB%B9%84%EC%9A%A9-%EA%B5%AC%ED%95%98%EA%B8%B0%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4MySQLLevel-4)


[프로그래머스 5월 식품들의 총매출 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/131117)

```sql
SELECT O.PRODUCT_ID 
      ,P.PRODUCT_NAME
      ,P.PRICE*SUM(O.AMOUNT) AS TOTAL_SALES
FROM FOOD_PRODUCT P
JOIN FOOD_ORDER O ON P.PRODUCT_ID=O.PRODUCT_ID
WHERE YEAR(PRODUCE_DATE)=2022 AND MONTH(PRODUCE_DATE)=5
GROUP BY O.PRODUCT_ID, P.PRODUCT_NAME
ORDER BY TOTAL_SALES DESC, O.PRODUCT_ID
-- 테이블에서 생산일자가 2022년 5월
-- 식품들의 식품 ID, 식품 이름, 총매출
-- 결과는 총매출을 기준으로 내림차순 정렬해주시고 총매출이 같다면 식품 ID를 기준으로 오름차순 정렬
```
- Amount는 SUM 해줘야 함 ! 

조건에 맞는 도서와 저자 리스트 출력하기
```sql
SELECT B.BOOK_ID
      ,A.AUTHOR_NAME
      ,DATE_FORMAT(B.PUBLISHED_DATE,'%Y-%m-%d') AS PUBLISHED_DATE 
FROM BOOK B
JOIN AUTHOR A ON A.AUTHOR_ID=B.AUTHOR_ID
WHERE B.CATEGORY='경제'
ORDER BY B.PUBLISHED_DATE 
```







