1. 해커 랭크_[Weather Observation Station 18](https://www.hackerrank.com/challenges/weather-observation-station-18/problem?isFullScreen=true)
```sql
SELECT ROUND(ABS(MIN(LAT_N)-MAX(LAT_N)) + ABS(MIN(LONG_W)-MAX(LONG_W)),4)
FROM STATION;
```
- 5분만에 쉽게 답이 나왔다. 너무 겁먹을 필요 없다. 
- ABS 를 알고 잇었기 때문에, ROUND 알고 있었기 때문에, 다른 문제들도 이것 만큼 빠르게 나올 수 있다!  
- 다만, 역시 영어 공부도 되는 코딩 공부! Plane? 비행기가 아니라 평면, 2D Plane은 2차원 

2. 해커랭크_ [Weather Observation Station 19](https://www.hackerrank.com/challenges/weather-observation-station-19/problem?isFullScreen=true&h_r=next-challenge&h_v=zen)
```sql
-- MIN(LAT_N), MIN(LONG_W) p1(a,c)
-- MAX(LAT_N), MAX(LONG_W) p2(b,d)
SELECT ROUND(SQRT(POW(MIN(LAT_N)-MAX(LAT_N),2) + POW(MIN(LONG_W)-MAX(LONG_W),2)),4)
FROM STATION;
```
- 유클리드 이런 개념에 갑자기 당황해서 겁먹지만 않으면 된다! 

3. 해커랭크_[Weather Observation Station 20](https://www.hackerrank.com/challenges/weather-observation-station-20/problem?isFullScreen=true)

```sql
SELECT ROUND(LAT_N,4)
FROM (SELECT LAT_N, PERCENT_RANK() OVER (ORDER BY LAT_N ASC) p
      FROM STATION) A
WHERE p=0.5;
```
- PERCENT_RANK() : 임의의 행의 백분율 순위를 계산 0 ~ 1 로 계산 
- order by 해서 p=0.5 : median 이겠죠? 

- sql 스터디로 보완! 
