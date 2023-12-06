
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
