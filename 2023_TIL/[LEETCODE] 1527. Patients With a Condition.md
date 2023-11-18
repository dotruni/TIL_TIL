
### BAD & WRONG 
```sql
ELECT patient_id 
      ,patient_name
      ,conditions
FROM patients  
WHERE SUBSTRING_INDEX(conditions ,' ',1) LIKE 'DIAB1%' 
  OR SUBSTRING_INDEX(conditions ,' ',-1) LIKE 'DIAB1%' 
-- Write a solution to find the patient_id, patient_name, and conditions of the patients 
--  who have Type I Diabetes. Type I Diabetes always starts with DIAB1 prefix.
```
-- 눈앞에 있는 sample case만 충족하지 다른 케이스들은 만족하지 못한다. (3개의 공백등) 

### GOOD 
```sql
SELECT patient_id 
      ,patient_name
      ,conditions
FROM patients  
WHERE conditions LIKE '% DIAB1%' 
  OR conditions LIKE 'DIAB1%'
```
- 공백이 있으면 %는 사용 못한다고 생각했는데 LIKE '% DIA' 이렇게 띄우면 어디서든지 사용가능하다. 
