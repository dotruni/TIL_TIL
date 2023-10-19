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
