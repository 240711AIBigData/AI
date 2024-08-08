데이터프레임
---
- 데이터를 다룰 때 가장 많이 사용하는 데이터 형태
- 행과 열로 구성된 사각형 모양의 표처럼 생김(이차원 배열 형식)

|-|
|-|
|![image](https://github.com/user-attachments/assets/22aa2b55-5284-4b61-ae66-0f97744dd799)|

<br>

열 = 속성
---
- 컬럼(column) 또는 변수(variable)
  
|-|
|-|
|![image](https://github.com/user-attachments/assets/683ca9d7-9738-418d-996d-88e0b28375e8)|

<br>

행 = 한 사람의 정보
---
- 로우(row) 또는 케이스(case)
  
|-|
|-|
|![image](https://github.com/user-attachments/assets/d6fa03b5-a6bf-4d5e-8a4d-f374313905fb)|

<br>


###  데이터가 크다 = 행이 많다 or 열이 많다
```
    - 행이 많다 -> 컴퓨터가 느려짐 -> 고사양 장비 구축
    - 열이 많다 -> 분석 방법의 한계 -> 고급 분석 방법
```

|-|
|-|
|![image](https://github.com/user-attachments/assets/496d4812-53a9-46b8-9056-42d3079b9aa2)|

<br>

---

<br>

데이터 프레임 만들기
---
### 데이터를 입력해 데이터 프레임 만들기
```
  import pandas as pd
  
  df = pd.DataFrame({'name'    : ['김지훈', '이유진', '박동현', '김민지'],
                     'english' : [90, 80, 60, 70],
                     'math'    : [50, 60, 100, 20]})
  df
```

<br>

### 데이터 프레임으로 분석하기
#### 특정 변수의 값 추출하기
```
  df['english']
```

<br>

#### 변수의 값으로 합계 구하기
```
  sum(df['english'])
```
```
  sum(df['math'])
```

<br>

#### 변수의 값으로 평균 구하기
```
  sum(df['english']) / 4  # 영어 점수 평균
```
```
  sum(df['math']) / 4     # 수학 점수 평균
```

<br>







