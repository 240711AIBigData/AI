# 이상치 정제하기
이상치(anomaly)
---
- 정상 범위에서 크게 벗어난 값
  
  - 실제 데이터에 대부분 이상치 들어있음
 
  - 제거하지 않으면 분석 결과 왜곡되므로 분석 전에 제거 작업 필요

<br>

이상치 제거하기 - 존재할 수 없는 값
---
- 논리적으로 존재할 수 없는 값이 있을 경우 결측치로 변환 후 제거

  - ex) 성별 변수에 1(남성), 2(여성) 외 3이 있다면 3을 NaN 으로 변환

<br>

> 이상치가 들어있는 데이터 만들기
```
  df = pd.DataFrame({'sex'   : [1, 2, 1, 3, 2, 1],
                     'score' : [5, 4, 3, 4, 2, 6]})
  df
```

<br>

### 이상치 확인하기
> 빈도표를 만들어 존재할 수 없는 값이 있는지 확인
```
  df['sex'].value_counts(sort = False).sort_index()
```
```
  df['score'].value_counts(sort = False).sort_index()
```

<br>

### 결측 처리하기

> 이상치일 경우 NaN 부여
```
  # sex가 3이면 NaN 부여
  df['sex'] = np.where(df['sex'] == 3, np.nan, df['sex'])
  df
```
```
  # score가 5보다 크면 NaN 부여
  df['score'] = np.where(df['score'] > 5, np.nan, df['score'])
  df
```

<br>

> 결측치 제거 후 분석
```
  df.dropna(subset = ['sex', 'score']) \        # sex, score 결측치 제거
         .groupby('sex') \                      # sex 별 분리
         .agg(mean_score = ('score', 'mean'))   # score 평균 구하기
```

<br>

#### 💡 np.where() 는 문자와 NaN을 함께 반환할 수 없음
- np.where() 사용할 때 반환값 중 문자가 있으면 np.nan 지정해도 문자 'nan' 반환

- 결측치 확인하면 모든 값이 false 로 나타남

<br>

```
  df = pd.DataFrame({'x1' : [1, 1, 2, 2]})
  df['x2'] = np.where(df['x1'] == 1, 'a', np.nan)  # 조건에 맞으면 문자 부여
  print(df)
  df.isna()
```

<br>

##### 변수를 문자와 NaN으로 함께 구성하는 방법
> 1. 결측치로 만들 값에 문자 부여
```
  # 결측치로 만들 값에 문자 부여
  df['x2'] = np.where(df['x1'] == 1, 'a', 'etc')
```

<br>

> 2. df.replace() 를 이용해 결측치로 만들 문자를 np.nan 으로 바꾸기
```
  # 'etc'를 NaN으로 바꾸기
  df['x2'] = df['x2'].replace('etc', np.nan)
  print(df)
  df.isna()
```

<br>

---

<br>

극단치(outlier)
---
- 논리적으로 존재할 수 있지만 극단적으로 크거나 작은 값

  - 극단치 있으면 분석 결과 왜곡될 수 있으므로 제거 후 분석
 
  - 기준 정하기
 
    - 논리적 판단
   
      - ex) 성인 몸무게 40~150kg 벗어나면 매우 드물기에 극단치로 간주
   
    - 통계적 기준
   
      - ex) 상하위 0.3% 또는 ±3 표준편차 벗어나면 극단치로 간주
   
    - 상자 그림(box plot)을 이용해 중심에서 크게 벗어난 값을 극단치로 간주

<br>

이상치 제거하기 - 극단적인 값
---
### 상자 그림으로 극단치 기준 정하기
#### 상자 그림(box plot)
- 데이터의 분포를 상자 모양으로 표현한 그래프

  - 중심에서 멀리 떨어진 값을 점으로 표현
 
  - 상자 그림을 이용해 극단치 기준 구할 수 있음
 
<br>

|box plot|
|-|
|![image](https://github.com/user-attachments/assets/4fd40df3-4630-4aa6-813e-06859d2deffd)|

<br>

### 1. 상자 그림 살펴 보기
```
  mpg = pd.read_csv('mpg.csv')
  
  import seaborn as sns
  sns.boxplot(data = mpg, y = 'hwy')
```

<br>

|box plot|
|-|
|![image](https://github.com/user-attachments/assets/51d70bfd-603c-413e-a1c0-ef904cb63ed0)|

<br>

|상자 그림|값|설명|
|-|-|-|
|상자 아래 세로선|아랫수염|하위 0~25% 내에 해당하는 값|
|상자 밑면|1사분위수(Q1)|하위 25% 위치 값|
|상자 내 굵은 선|2사분위수(Q2)|하위 50% 위치 값(중앙값)|
|상자 윗면|3사분위수(Q3)|하위 75% 위치 값|
|상자 위 세로선|윗수염|하위 75~100% 내에 해당하는 값|
|상자 밖 가로선|극단치 경계|Q1, Q3 밖 1.5 IQR 내 최대값|
|상자 밖 점 표식|극단치|Q1, Q3 밖 1.5 IQR을 벗어난 값|

- IQR(사분위 범위)

  - 1사분위수와 3사분위수의 거리
 
- 1.5 IQR

  - IQR 의 1.5배

<br>

### 2. 극단치 기준값 구하기
> (1) 1사분위수, 3사분위수 구하기
```
  pct25 = mpg['hwy'].quantile(.25)
  pct25
```
```
  pct75 = mpg['hwy'].quantile(.75)
  pct75
```

<br>

> (2) IQR 구하기
```
  iqr = pct75 - pct25
  iqr
```

<br>

> (3) 하한, 상한 구하기
```
  pct25 - 1.5 * iqr  # 하한
```
```
  pct75 + 1.5 * iqr  # 상한
```

<br>

### 3. 극단치를 결측 처리하기
```
  # 4.5 ~ 40.5 벗어나면 NaN 부여
  mpg['hwy'] = np.where((mpg['hwy'] < 4.5) | (mpg['hwy'] > 40.5), np.nan, mpg['hwy'])
  
  # 결측치 빈도 확인
  mpg['hwy'].isna().sum()
```

<br>

### 4. 결측치 제거하고 분석하기
```
  mpg.dropna(subset = ['hwy']) \        # hwy 결측치 제거
     .groupby('drv') \                  # drv 별 분리
     .agg(mean_hwy = ('hwy', 'mean'))   # hwy 평균 구하기
```

<br>

정리하기
---
### 1. 결측치 정제하기
```
  pd.isna(df).sum()                                 # 결측치 확인
  df_nomiss = df.dropna(subset = 'score' )          # 결측치 제거
  df_nomiss = df.dropna(subset = ['score', 'sex'])  # 여러 변수 동시에 결측치 제거
```

<br>

### 2. 이상치 정제하기
```
  # 이상치 확인
  df['sex'].value_counts(sort = False)
  
  # 이상치 결측 처리
  df['sex'] = np.where(df['sex'] == 3, np.nan, df['sex'])
  
  # 상자 그림으로 극단치 기준값 찾기
  pct25 = mpg['hwy'].quantile(.25)  # 1사분위수
  pct75 = mpg['hwy'].quantile(.75)  # 3사분위수
  iqr = pct75 - pct25               # IQR
  pct25 - 1.5 * iqr                 # 하한
  pct75 + 1.5 * iqr                 # 상한
  
  # 극단치 결측 처리
  mpg['hwy'] = np.where((mpg['hwy'] < 4.5) | (mpg['hwy'] > 40.5), np.nan, mpg['hwy'])
```

<br>

