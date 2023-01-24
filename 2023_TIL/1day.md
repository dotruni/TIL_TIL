## TIL
### 백준 기본부터 다시 다지기로 했다.
- 재밌다..ㅎㅎ
- 그냥 별거 없이, 재밌는 수학 문제집 ex) 인수분해로 스트레스 푼다고 생각하기 :) 
#### 문제 하나를 다양하게 뜯고 맛보기! 
- 1000번 : A+B


![image](https://user-images.githubusercontent.com/89775352/214284751-7cf11e1b-d950-4d7b-8470-2ae2395076ac.png)
- 첫째 줄에 A와 B가 주어진다. --> 첫째줄에 A와 B 변수 선언해야 한다는 것 
- (0 < A, B < 10) --> A 와 B가 정수(int) 여야 한다는 것  
```python
1. 

A,B = input().split() # 띄어쓰기를 기준으로 split하여 입력값을 변수에 저장함
print(int(A)+int(B)) # 입력된 변수를 정수로 저장해서 프린트함 

2. 

A,B = input().split() # 띄어쓰기를 기준으로 split해서 변수 저장
A = int(A)
B = int(B) # 따로 따로 정수로 저장
print(A+B) # 코드가 길어져서 이렇게 하고 싶지는 않음

3. 
a, b = map(int,input().split()) # map(변환 함수, 순회 가능한 데이터), 여러 개의 데이터를 한번에 다른 형태로 바꾸기 위해 사용 , 편리
print(a+b)


```
- 1001 : A-B
![image](https://user-images.githubusercontent.com/89775352/214288054-d5526eae-666b-4e70-8ad6-787946b73867.png)

```python
1. 
A, B = input().split() 
print(int(A)-int(B))

2.
A, B = input().split()
A = int(A)
B= int(B)
print(A-B)

3.
A, B = map(int,input().split())
print(A-B)
```
