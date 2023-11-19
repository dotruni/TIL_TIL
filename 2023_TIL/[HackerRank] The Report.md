![image](https://github.com/dotruni/TIL_TIL/assets/89775352/b5bf0539-44d3-4f80-8a31-e6fa33bafa24)
![image](https://github.com/dotruni/TIL_TIL/assets/89775352/371d2ced-9f3b-4326-a63b-4e7a629956e9)

### BAD 
```sql 
SELECT (CASE WHEN marks >= 70 THEN name ELSE NULL END) AS name  
      ,(CASE WHEN marks >= 0 AND marks < 10 THEN 1 
             WHEN marks>= 10 AND marks < 20 THEN 2
             WHEN marks>= 20 AND marks < 30 THEN 3  
             WHEN marks>= 30 AND marks < 40 THEN 4
             WHEN marks>= 40 AND marks < 50 THEN 5
             WHEN marks>= 50 AND marks < 60 THEN 6
             WHEN marks>= 60 AND marks < 70 THEN 7
             WHEN marks>= 70 AND marks < 80 THEN 8        
             WHEN marks>= 80 AND marks < 90 THEN 9        
             WHEN marks>= 90 AND marks <= 100 THEN 10   
        ELSE NULL END) AS grade 
      ,marks 
FROM Students 
ORDER BY grade DESC
       ,CASE WHEN name IS NULL THEN marks END ASC
       ,CASE WHEN name IS NOT NULL THEN name END ASC
```
       
### GOOD 

-- Grades랑 Students 테이블 합치고 싶은데.. 방법 없을까...여기서 BETWEEN을 떠올리기..! 

```sql
SELECT IF(grade>= 8,name,NULL) AS name
      ,g.grade
      ,marks 
FROM Students AS s  
JOIN Grades AS g 
WHERE s.marks BETWEEN g.min_mark AND max_mark 
ORDER BY grade DESC, name , marks
```
