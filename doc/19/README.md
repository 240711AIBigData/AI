# 통계 분석 기법을 이용한 가설 검정
|-|
|-|
|![image](https://github.com/user-attachments/assets/7afef5a6-375b-44f4-bc1c-15a647f394dd)|

<br>

가설 검정이란
---
### 기술 통계와 추론 통계
- 기술 통계(descriptive statistics)

  - 데이터를 요약해 설명하는 통계 분석 기법
 
- 추론 통계(inferential statistics)

  - 어떤 값이 발생할 확률을 계산하는 통계 분석 기법

<br>

> ex) 성별에 따른 월급 차이가 우연히 발생할 확률 계산

- 이런 차이가 우연히 나타날 확률이 작다면

  - 성별에 따른 월급 차이가 통계적으로 유의하다(statistically significant)고 결론
 
- 이런 차이가 우연히 나타날 확률이 크다면

  - 성별에 따른 월급 차이가 통계적으로 유의하지 않다(not statistically significant)고 결론

<br>

- 기술 통계 분석에서 집단 간 차이가 있는 것으로 나타났더라도 이는 우연에 의한 차이일 수 있음

- 신뢰할 수 있는 결론을 내리려면 유의확률을 계산하는 통계적 가설 검정 절차 거쳐야 함

<br>

### 통계적 가설 검정(statistical hypothesis test)
- 유의확률을 이용해 가설을 검정하는 방법

- 유의확률(significance probability, p-value)

  - 실제로는 집단 간 차이가 없는데 우연히 차이가 있는 데이터가 추출될 확률

<br>

#### 유의확률이 크면
- '집단 간 차이가 통계적으로 유의하지 않다' 고 해석

- 실제로 차이가 없더라도 우연에 의해 이런 정도의 차이가 관찰될 가능성이 크다는 의미

<br>

#### 유의확률이 작으면
- '집단 간 차이가 통계적으로 유의하다' 고 해석

- 실제로 차이가 없는데 우연히 이런 정도의 차이가 관찰될 가능성이 작다, 우연이라고 보기 힘들다는 의미

<br>

---

<br>

t 검정(t-test) - 두 집단의 평균 비교하기
---
- 두 집단의 평균에 통계적으로 유의한 차이가 있는지 알아볼 때 사용하는 통계 분석 기법

<br>

### compact 자동차와 suv 자동차의 도시 연비 t 검정
#### 기술 통계 분석
```Python
  import pandas as pd
  mpg = pd.read_csv('mpg.csv')
```
```Python
  ## 기술 통계 분석
  
  # compact, suv 추출하기
  # category별 분리
  # 빈도 구하기
  # cty 평균 구하기
  mpg.query('category in ["compact", "suv"]') \
     .groupby('category', as_index = False) \
     .agg(n    = ('category', 'count'),
          mean = ('cty', 'mean'))
```

<br>

#### t 검정
- 비교하는 집단의 분산(값이 퍼져 있는 정도)이 같은지 여부에 따라 적용하는 공식이 다름

- equal_var = True : 집단 간 분산이 같다고 가정

- 일반적으로 유의확률 5%를 판단 기준으로 삼음

  - p-value 가 0.05 미만이면 '집단 간 차이가 통계적으로 유의하다' 고 해석
 
  - 실제로는 차이가 없는데 이런 정도의 차이가 우연히 관찰될 확률이 5% 보다 작다면
 
    - 이 차이를 우연이라고 보기 어렵다고 결론내리는 것

  - 'pvalue=2.3909550904711282e-21'
 
    - 2.3909550904711282 * 10의 -21승
   
  - p-value 가 0.05 보다 작기 때문에 'compact와 suv 간 평균 도시 연비 차이가 통계적으로 유의하다' 고 결론

```Python
  compact = mpg.query('category == "compact"')['cty']
  suv = mpg.query('category == "suv"')['cty']
  
  # t-test
  from scipy import stats
  stats.ttest_ind(compact, suv, equal_var = True)
```

<br>

### 일반 휘발유와 고급 휘발유의 도시 연비 t 검정
#### 기술 통계 분석
```Python
  ## 기술 통계 분석
  
  # r, p 추출하기
  # fl별 분리
  # 빈도 구하기
  # cty 평균 구하기
  mpg.query('fl in ["r", "p"]') \
     .groupby('fl', as_index = False) \
     .agg(n    = ('category', 'count'),
          mean = ('cty', 'mean'))
```

<br>

#### t 검정
```Python
  regular = mpg.query('fl == "r"')['cty']
  premium = mpg.query('fl == "p"')['cty']
  
  # t-test
  stats.ttest_ind(regular, premium, equal_var = True)
```
- p-value 가 0.05 보다 큼

- 실제로는 차이가 없는데 우연에 의해 이런 정도의 차이가 관찰될 확률이 28.75%

- '일반 휘발유와 고급 휘발유를 사용하는 자동차의 도시 연비 차이가 통계적으로 유의하지 않다' 고 결론

- 고급 휘발유 자동차의 연비 평균이 0.6 정도 높지만 이런 차이는 우연히 발생했을 가능성이 크다고 해석

<br>

---

<br>

상관분석(correlation analysis) - 두 변수의 관계 분석하기
---
- 두 연속 변수가 서로 관련이 있는지 검정하는 통계 분석 기법

- 상관계수(correlation coefficient)

  - 두 변수가 얼마나 관련되어 있는지, 관련성의 정도 파악 가능
 
  - 0~1 사이의 값, 1에 가까울수록 관련성이 크다는 의미
 
  - 양수면 정비례, 음수면 반비례 관계

<br>

### 실업자 수와 개인 소비 지출의 상관관계
#### 1. 상관계수 구하기
```Python
  # economics 데이터 불러오기
  economics = pd.read_csv('economics.csv')
  
  # 상관행렬 만들기
  economics[['unemploy', 'pce']].corr()
```
- 출력 결과는 `unemploy`와 `pce`의 상관계수를 나타낸 행렬

- 행과 열에 똑같은 변수가 나열되어 대칭이므로 왼쪽 아래와 오른쪽 위에 표현된 상관계수가 같음

- 상관계수가 양수 0.61

  - 한 변수가 증가하면 다른 변수가 증가하는 정비례 관계

<br>

#### 2. 유의확률 구하기
```Python
  # 상관분석
  stats.pearsonr(economics['unemploy'], economics['pce'])
```
- 출력 결과의 첫 번째 값이 상관계수, 두 번째 값이 유의확률

- 유의확률이 0.05 미만이므로 실업자 수와 개인 소비 지출의 상관관계가 통계적으로 유의하다고 결론

<br>

### 상관행렬 히트맵 만들기
#### 상관행렬(correlation matrix)
- 모든 변수의 상관관계를 나타낸 행렬

- 여러 변수의 관련성을 한꺼번에 알아보고 싶을 때 사용

- 어떤 변수끼리 관련이 크고 적은지 한 눈에 파악 가능

<br>

#### 1. 상관행렬 만들기
```Python
  # 상관분석
  stats.pearsonr(economics['unemploy'], economics['pce'])
```
```Python
  car_cor = mtcars.corr()      # 상관행렬 만들기
  car_cor = round(car_cor, 2)  # 소수점 둘째 자리까지 반올림
  car_cor
```
- mpg(연비)와 cyl(실린더 수)의 상관계수 -0.85

  - 연비가 높을수록 실린더 수가 적은 경향이 있음
 
- cyl(실린더 수)과 wt(무게)의 상관계수 0.78

  - 실린더 수가 많을수록 자동차가 무거운 경향

<br>

#### 2. 히트맵 만들기
- 여러 변수로 상관행렬을 만들면 출력된 값이 너무 많아서 관심있는 변수들의 관계를 파악하기 어려움

- 값의 크기를 색깔로 표현한 히트맵(heatmap)을 만들면 변수들의 관계 쉽게 파악 가능

```Python
  import matplotlib.pyplot as plt
  plt.rcParams.update({'figure.dpi' : '120',           # 해상도 설정
                       'figure.figsize': [7.5, 5.5]})  # 가로 세로 크기 설정
  
  # 히트맵 만들기
  import seaborn as sns
  sns.heatmap(car_cor,
              annot = True,   # 상관계수 표시
              cmap = 'RdBu')  # 컬러맵
```
- 상관계수가 클수록 상자 색깔을 진하게 표현

- 상관계수가 양수면 파란색, 음수면 빨간색 계열로 표현

- 상자 색깔을 보면 상관관계의 정도와 방향 쉽게 파악 가능

<br>

#### 3. 대각 행렬 제거하기
- 히트맵은 대각선 기준으로 왼쪽 아래와 오른쪽 위의 값이 대칭하여 중복됨

- sns.heatmap() 의 mask 를 이용해 중복된 부분 제거

<br>

##### (1) mask 만들기
> 상관행렬의 행과 열의 수 만큼 0으로 채운 배열(array) 만듦
```Python
  # mask 만들기
  import numpy as np
  mask = np.zeros_like(car_cor)
  mask
```

<br>

> mask 의 오른쪽 위 대각 행렬을 1로 변경
```Python
  # 오른쪽 위 대각 행렬을 1로 바꾸기
  mask[np.triu_indices_from(mask)] = 1
  mask
```

<br>

##### (2) 히트맵에 mask 적용하기
> sns.heatmap() 에 mask 적용
```Python
  # 히트맵 만들기
  sns.heatmap(data = car_cor,
              annot = True,   # 상관계수 표시
              cmap = 'RdBu',  # 컬러맵
              mask = mask)    # mask 적용
```
- mask 의 1에 해당하는 위치의 값이 제거되어 왼쪽 아래의 상관계수만 표현됨

<br>

##### (3) 빈 행과 열 제거하기
- 앞에서 만든 히트맵의 왼쪽 위 mpg행, 오른쪽 아래 carb열에 아무 값도 표현 X

  - 행과 열의 변수가 같아서 상관계수가 항상 1이 되는 위치이기 때문
 
- mask 와 상관행렬의 첫 번째 행과 마지막 열을 제거해서 빈 행과 열 제거

```Python
  mask_new = mask[1:, :-1]         # mask 첫 번째 행, 마지막 열 제거
  cor_new = car_cor.iloc[1:, :-1]  # 상관행렬 첫 번째 행, 마지막 열 제거
  
  sns.heatmap(data = cor_new,
              annot = True,       # 상관계수 표시
              cmap = 'RdBu',      # 컬러맵
              mask = mask_new)    # mask 적용
```

<br>

> 보기 좋게 수정하기
```Python
  sns.heatmap(data = cor_new,
              annot = True,               # 상관계수 표시
              cmap = 'RdBu',              # 컬러맵
              mask = mask_new,            # mask 적용
              linewidths = .5,            # 경계 구분선 추가
              vmax = 1,                   # 가장 진한 파란색으로 표현할 최대값
              vmin = -1,                  # 가장 진한 빨간색으로 표현할 최소값
              cbar_kws = {'shrink': .5})  # 범례 크기 줄이기
```

<br>

#### 💡 히트맵 모양 바꾸기
- sns.heatmap() 의 파라미터를 이용하면 히트맵의 모양을 다양하게 바꿀 수 있음

- [seaborn 공식 문서](bit.ly/easypy_142) 참고

<br>

