

### Bad 
- using WITH , too long

```sql
-- tiv_2015 밸류가 다른 pid에도 있어야 한다. 
WITH another AS(
    SELECT tiv_2015 
        ,COUNT(pid) AS cnt 
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(pid) > 1  

), locat AS (    
    SELECT lat
        ,lon
    ,COUNT(pid) AS cnt 
    FROM Insurance
    GROUP BY lat,lon
    HAVING COUNT(pid) = 1
)

SELECT ROUND(SUM(tiv_2016),2) AS tiv_2016 
FROM Insurance AS i 
JOIN another ON another.tiv_2015 = i.tiv_2015
JOIN locat ON (locat.lat,locat.lon) = (i.lat,i.lon)                                
```

### GOOD 
- using WINDOW FUNCTION , short, effective

```sql
SELECT ROUND(SUM(tiv_2016),2) AS tiv_2016 
FROM (
        SELECT * 
            , COUNT(*) OVER(PARTITION BY tiv_2015) AS tiv
            , COUNT(*) OVER(PARTITION BY lat,lon) AS loc
        FROM Insurance     
)l
WHERE l.tiv > 1 AND l.loc = 1 
-- 처음에 다 * 집계하면 가능
```
