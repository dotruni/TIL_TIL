

나의 코드 
```sql 
SELECT employee_id  
FROM Employees 
WHERE salary < 30000 AND manager_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY employee_id 

-- 그 사람의 manager_id가 employee_id 안에 없어야 한다 
```
- ORDER BY 항상 까먹지 말기 


다른 사람의 코드 

```sql
SELECT e1.employee_id
FROM Employees e1
LEFT JOIN Employees e2
ON e1.manager_id = e2.employee_id
WHERE e1.salary < 30000 AND e2.employee_id IS NULL AND e1.manager_id IS NOT NULL
ORDER BY employee_id;
``` 
- LEFT JOIN 하는 방법도 괜찮은 것 같다. 
