
### 7일 이동 평균 구하기 
#### ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
- PRECEDING 이란? : 그 전의 , 출처 : 네이버 사전 
![image](https://github.com/dotruni/TIL_TIL/assets/89775352/27e37b2d-a806-4e08-9ab3-e0aeddbe8c7b)

- ROWS BETWEEN 6 PRECEDING AND CURRENT ROW : 6개 전 행과 현재 행 _ 의 카운트 _ 의 평균 

```sql

SELECT dt 
      ,SUM(purchase_amount)
      ,AVG(SUM(purchase_amount)) OVER(ORDER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
       AS seven_day_avg
      ,CASE 
        WHEN 
          7 = COUNT(*) OVER(ORDER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
        THEN 
          AVG(SUM(purchase_amount)) OVER(ORDER BY dt ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
        END 
        AS seven_day_avg_strict 
FROM purchase_log
GROUP BY dt
ORDER BY dt ;

```

### 당월 매출 누계 구하기 
#### ROWS UNBOUNDED PRECEDING 
- UNBOUNDED란? , 출처 : 네이버 사전  
![image](https://github.com/dotruni/TIL_TIL/assets/89775352/c431afbf-d431-446b-b4c9-e1092cb8fba2) 
- ROWS UNBOUNDED PRECEDING : 무한한 행 전의
- The purpose of the ROWS clause is to specify the window frame in relation to the current row. T
- he syntax is:
-   ROWS BETWEEN lower_bound AND upper_bound

- The bounds can be any of these five options:
  - UNBOUNDED PRECEDING – All rows before the current row.
  - n PRECEDING – n rows before the current row.
  - CURRENT ROW – Just the current row.
  - n FOLLOWING – n rows after the current row.
  - UNBOUNDED FOLLOWING – All rows after the current row.

![image](https://github.com/dotruni/TIL_TIL/assets/89775352/e56b716a-7d7b-46a0-ace5-09f5a6cb1393)
[출처: 5 Practical Examples of Using ROWS BETWEEN in SQL](https://learnsql.com/blog/sql-window-functions-rows-clause/)


```sql
SELECT dt 
       -- 연-월
      ,substring(dt,1,7) AS year_month 
      ,SUM(purchase_amount) AS total_amount
      ,SUM(SUM(purchase_amount)) OVER(PARTITION BY substring(dt,1,7)
                                      ORDER BY dt ROWS UNBOUNDED PRECEDING) AS agg_amount
FROM purchase_log
GROUP BY dt
ORDER BY dt ;

```

### 행을 열로 반환하기 
```sql
SELECT dt 
      ,MAX(CASE WHEN indicator = 'impressions' THEN val END) AS impressions
      ,MAX(CASE WHEN indicator = 'sessions' THEN val END) AS sessions
      ,MAX(CASE WHEN indicator = 'users' THEN val END) AS users     
FROM daily_kpi
GROUP BY dt
ORDER BY dt; 
```

월별 매출과 작대비를 계산하는 쿼리 
WITH daily_purchase AS ( 
  SELECT dt
        ,substr(dt,1,4) AS year
        ,substr(dt,6,2) AS month
        ,substr(dt,9,2) AS day
        ,SUM(purchase_amount) AS purchase_amount
        ,COUNT(order_id) AS orders
  FROM purchase_log
  GROUP BY dt 
)

SELECT month
      ,SUM(CASE WHEN year ='2014' THEN purchase_amount END) AS amount_2014
      ,SUM(CASE WHEN year ='2015' THEN purchase_amount END) AS amount_2015
      , SUM(CASE WHEN year = '2015' THEN purchase_amount END) * 100.0 
      / SUM(CASE year WHEN '2014' THEN purchase_amount END) 
      AS rate
FROM daily_purchase
GROUP BY month
ORDER BY month


### Z 차트로 업적의 추이 확인하기 
- Z차트는 월별매출, 매출 누계, 이동 년계 라는 3가지 지표로 구성됩니다. 
https://blog.naver.com/socialmedia/220194062598
```sql
WITH daily_purchase AS ( 
  SELECT dt
        ,substr(dt,1,4) AS year
        ,substr(dt,6,2) AS month
        ,substr(dt,9,2) AS day
        ,SUM(purchase_amount) AS purchase_amount
        ,COUNT(order_id) AS orders
  FROM purchase_log
  GROUP BY dt 
  
), monthly_amount AS (
  
  --  월별 매출 집계하기   
  SELECT year
        ,month
  		,SUM(purchase_amount) AS amount
  FROM daily_purchase
  GROUP BY year,month 
  
),calc_index AS (
  SELECT 
  	year
   ,month
   ,amount
   -- 2015년의 누계 매출 집계하기 
  ,SUM(CASE WHEN year = '2015' THEN amount END)
   OVER(ORDER BY year,month ROWS UNBOUNDED PRECEDING)
   AS agg_amount
 -- 당월부터 11개월 이전까지의 매출 합계(이동 년계) 집계하기 
  ,SUM(amount) OVER(ORDER BY year,month ROWS BETWEEN 11 PRECEDING AND CURRENT ROW)
  AS year_avg_amount 
  FROM monthly_amount
  ORDER BY year,month 
 )
 
 ### 마지막으로 2015년의 데이터만 압축하기
SELECT concat(year, '-' , month) AS yearandmonth
      ,amount
      ,agg_amount
      ,year_avg_amount
FROM calc_index
WHERE year = '2015'
ORDER BY yearandmonth
``` 
