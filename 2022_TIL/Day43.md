## 감사일기 
- 나: 열심히 SQL 공부에 재미를 느낀 나에게 감사하다
- 타인:  예란이와 초등학교 여름 캠프 같은 시간을 보내서 즐거웠다. 함께 고민을 나누고 즐겁게 논 예란이에게 감사하다.
- 물질: 맥주 4캔을 살 수 있는 금전적 여유와 돈에 쪼들리지 않는 나와 부모님에게 감사하다.
- 경험: 코드를 치고, 문제를 풀면서 행복감을 느낀 경험에 감사하다. 별장에서 자연 속에서 힐링을 하면서 책을 읽을 수 있어서 행복했다. 

## TIL 
[180. Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers/)
​
```sql
SELECT a.num as ConsecutiveNums
FROM 
        (  
        SELECT id
              ,num
              ,(LEAD(num,1) OVER (ORDER BY id))l
              ,(LEAD(num,2) OVER (ORDER BY id))m
        FROM logs
        )a
WHERE a.num=a.l=a.m
```
​
- mysql로 했을 때는 WHERE a.num=a.l=a.m 이게 되는데 mssql로 하니까 WHERE a.num=a.l=a.m 이게 에러가 난다.
​
- 왜 그럴까? 곧 처리하기!
​
181. Employees Earning More Than Their Managers
​
```sql
SELECT e.name as Employee
FROM Employee e
    JOIN Employee e2 ON e.managerid=e2.id
WHERE e.salary > e2.salary
```
​
- 오타도 실력이다! 
​
- more than 은 영어에서 초과!
​
182. Duplicate Emails
​
```sql
SELECT email 
FROM Person
GROUP BY email
HAVING COUNT(*)>1
```
​
[183. Customers Who Never Order](https://leetcode.com/problems/customers-who-never-order/)
​
```sql
SELECT c.name as Customers
FROM Customers c
    LEFT JOIN Orders o ON c.id=o.customerid
WHERE customerid IS NULL
```
​
Runtime: 876 ms, faster than 10.91% of MySQL online submissions for Customers Who Never Order.
​
```sql
SELECT name as Customers
FROM Customers
WHERE id NOT IN ( SELECT customerid FROM Orders )
```
​
- Runtime: 456 ms, faster than 74.53% of MySQL online submissions for Customers Who Never Order.
​
- 성능을 높이기 위해 여러가지 방법을 시도해 보고 있는데.. 의문 덩어리가 남았다. 
​
- 왜 같은 문제, 같은 쿼리인데 성능이 천차만별 인 것일까?
- 그래서 질문을 남겼다. 

![image](https://user-images.githubusercontent.com/89775352/175099856-9f9e2beb-d16c-4cc8-b2d9-16f5e8051a78.png)

지엽적인 문제일 수 있으니 일단 저장해 놓고 가겠다.
