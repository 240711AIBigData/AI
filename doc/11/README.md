# 데이터 정제
- 빠진 데이터, 이상한 데이터 제거하기

|-|
|-|
|![image](https://github.com/user-attachments/assets/270281e7-7fb4-4fcb-bb35-2f10a9d59843)|


<br>

결측치 정제하기
---
### 결측치(missing value)
- 누락된 값, 비어있는 값

- 데이터 수집 과정에서 발생한 오류로 포함될 가능성

- 함수가 적용되지 않거나 분석 결과가 왜곡되는 문제 발생

- 실제 데이터 분석시 결측치 확인, 제거 후 분석해야 함

<br>

### 결측치 찾기
#### 결측치 만들기
- **NumPy** 패키지의 **np.nan** 입력

- 파이썬에서 결측치는 **NaN**으로 표시

- 불러온 데이터 파일에 결측치가 있으면 자동으로 **NaN**이 됨

<br>

> ex
```Python
  import pandas as pd
  import numpy as np
  df = pd.DataFrame({'sex'   : ['M', 'F', np.nan, 'M', 'F'],
                     'score' : [5, 4, 3, 4, np.nan]})
  df
```

<br>

#### NaN 있는 상태로 연산하면 출력 결과도 NaN
```Python
  df['score'] + 1
```

<Br>

### 결측치 확인하기
```Python
  pd.isna(df)  # 결측치 확인
```

<br>

#### 결측치 빈도 확인
```Python
  pd.isna(df).sum()  # 결측치 빈도 확인
```

<br>

### 결측치 제거하기
#### 결측치 있는 행 제거하기
```Python
  df.dropna(subset = 'score')  # score 결측치 제거

  df_nomiss = df.dropna(subset = 'score')  # score 결측치 제거된 데이터 만들기
  df_nomiss['score'] + 1                   # score로 연산
```

<br>

#### 여러 변수에 결측치 없는 데이터 추출하기
```Python
  df_nomiss = df.dropna(subset = ['score', 'sex'])  # score, sex 결측치 제거
  df_nomiss
```

<br>

#### 결측치가 하나라도 있으면 제거하기

- df.dropna()에 아무 변수도 지정하지 않음

- 모든 변수에 결측치가 없는 행만 남김

```Python
  df_nomiss2 = df.dropna()  # 모든 변수에 결측치 없는 데이터 추출
  df_nomiss2
```

- 간편하지만 분석에 사용할 수 있는 데이터까지 제거될 수 있음

  - ex) 성별, 소득, 지역 변수가 있을 때, 성별에 따른 소득 차이 분석 목적
 
  - 지역이 결측치면 분석하는데 문제 없으나 제거됨
 
- 분석에 사용할 변수 직접 지정하여 결측치 제거하는 방법 권장

<br>

#### 📌 결측치 제거하지 않고 분석하기
- **pd.mean(), pd.sum()** 은 결측치 있으면 자동으로 제거하고 연산함

- **groupby(), agg()** 도 결측치 제거하고 연산함

> ex
```Python
  df['score'].mean()
  df['score'].sum()
  df.groupby('sex').agg(mean_score = ('score', 'mean'),
                        sum_score  = ('score', 'sum'))
```

- 자동 결측치 제거 기능은 편리하나, 결측치가 있는지 모른채 데이터 다루게 되는 위험 有

- 직접 결측치 확인 후 **df.dropna()** 로 명시적으로 제거 권장

<br>

### 결측치 대체하기
- 결측치가 적고 데이터가 크면 결측치 제거 후 분석 가능

- 데이터가 작고 결측치 많으면 데이터 손실로 인해 분석 결과 왜곡 발생

- 결측치 대체법을 이용하면 보완 가능

<br>

### 결측치 대체법(imputation)
- 결측치를 제거하는 대신 다른 값을 채워 넣는 방법

  - 대표값(평균값, 최빈값 등)을 구해 일괄 대체
 
  - 통계 분석 기법으로 결측치의 예측값 추정 후 대체
 
<br>

#### 평균값으로 결측치 대체하기
```Python
  exam = pd.read_csv('exam.csv')           # 데이터 불러오기
  exam.loc[[2, 7, 14], ['math']] = np.nan  # 2, 7, 14행의 math에 NaN 할당
  exam
```

<br>

> 평균값 구하기
```Python
  exam['math'].mean()
```

<br>

> df.fillna()로 결측치를 평균값으로 대체하기
```Python
  exam['math'] = exam['math'].fillna(55)  # math가 NaN이면 55로 대체
  exam                                    # 출력
```

<br>

> df.fillna()로 결측치를 평균값으로 대체하기
```Python
  exam['math'].isna().sum()  # 결측치 빈도 확인
```

<br>










