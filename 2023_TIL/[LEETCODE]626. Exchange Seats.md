```sql
SELECT CASE 
            WHEN id % 2 = 0 THEN id-1
            WHEN id % 2 = 1 AND id < (SELECT COUNT(*) FROM Seat) THEN id+1 
            ELSE id    
        END AS id
        ,student 
FROM Seat 
ORDER BY id 

-- 마지막 친구가 짝수면 교환, 홀수면 id 그대로
```
- 논리 하나 하나를 적용시키기! 
