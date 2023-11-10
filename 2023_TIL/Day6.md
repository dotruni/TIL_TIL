
[HackerRank New Companies](https://www.hackerrank.com/challenges/the-company/problem?isFullScreen=true)
```sql 
SELECT c.company_code 
      ,founder
      ,COUNT(DISTINCT l.lead_manager_code)
      ,COUNT(DISTINCT s.senior_manager_code) 
      ,COUNT(DISTINCT m.manager_code)  
      ,COUNT(DISTINCT e.employee_code)  
FROM Company c 
LEFT JOIN Lead_Manager l ON c.company_code=l.company_code 
LEFT JOIN Senior_Manager s ON l.lead_manager_code=s.lead_manager_code 
LEFT JOIN Manager m ON s.senior_manager_code=m.senior_manager_code  
LEFT JOIN Employee e ON m.manager_code=e.manager_code 
GROUP BY c.company_code,founder
ORDER BY c.company_code 
```
