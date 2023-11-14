
```sql
SELECT requester_id AS id
     , COUNT(requester_id) AS num
FROM (
    SELECT requester_id 
    FROM RequestAccepted
    UNION ALL 
    SELECT accepter_id
    FROM RequestAccepted 
)a
GROUP BY requester_id
ORDER BY num DESC 
LIMIT 1 

-- 가장 많이 연결된 id 
```

- 너무 복잡하다고 생각했는데, most vote가 같은 답변 


- 이러한 답변도 있었음 
``` sql
I think the answer missed the case when A send B a friend request, and B send A a friend request, and both requests got approved. In this case, A or B really just gained one friend. But the answer seems to count this case twice.
Isn't union (remove duplicates) should be used instead of union all?

select id1 as id, count(id2) as num
from
(select requester_id as id1, accepter_id as id2 
from request_accepted
union
select accepter_id as id1, requester_id as id2 
```
from request_accepted) tmp1
group by id1 
order by num desc limit 1
```
