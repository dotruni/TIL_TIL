LeetCode 1731. The Number of Employees Which Report to Each Employee

```
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
```
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
```
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
```
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
