# 그래프 만들기
|다양한 그래프들|
|-|
|![image](https://github.com/user-attachments/assets/ee26dd91-4356-4d12-88cc-4b08131200a4)|

<br>

파이썬으로 만들 수 있는 그래프
---
### 그래프(graph)
> 데이터를 보기 쉽게 그림으로 표현한 것

- 추세와 경향성이 드러나 데이터의 특징을 쉽게 이해할 수 있음

- 새로운 패턴 발견, 데이터의 특징을 잘 전달

- 다양한 그래프

  - 2차원 그래프, 3차원 그래프
 
  - 지도 그래프
 
  - 네트워크 그래프
 
  - 모션 차트
 
  - 인터랙티브 그래프
 
<br>

### seaborn 패키지
- 그래프를 만들 때 자주 사용되는 패키지

- 코드가 쉽고 간결함

<br>

산점도 - 변수 간 관계 표현하기
---
### 산점도(scatter plot)
- 데이터를 x축과 y축에 점으로 표현한 그래프

- 나이와 소득처럼 연속값으로 된 두 변수의 관계를 표현할 때 사용

|산점도|
|-|
|![image](https://github.com/user-attachments/assets/335fd56c-7309-424c-a49d-f7f629290578)|

<br>

### 산점도 만들기
```
  import pandas as pd
  mpg = pd.read_csv('mpg.csv')
```
```
  # x축은 displ, y축은 hwy를 나타낸 산점도 만들기
  import seaborn as sns
  sns.scatterplot(data = mpg, x = 'displ', y = 'hwy')
```

<br>

#### 축 범위 설정하기
```
  # x축 범위 3~6으로 제한
  sns.scatterplot(data = mpg, x = 'displ', y = 'hwy') \
     .set(xlim = [3, 6])
```
```
  # x축 범위 3~6, y축 범위 10~30으로 제한
  sns.scatterplot(data = mpg, x = 'displ', y = 'hwy') \
     .set(xlim = [3, 6], ylim = [10, 30])
```

<br>

#### 종류별로 표식 색깔 바꾸기
```
  # drv별로 표식 색깔 다르게 표현
  sns.scatterplot(data = mpg, x = 'displ', y = 'hwy', hue = 'drv')
```

<br>

#### 💡 그래프 활용하기
> 그래프 설정 바꾸기
```
  import matplotlib.pyplot as plt
  plt.rcParams.update({'figure.dpi' : '150'})             # 해상도, 기본값 72
  plt.rcParams.update({'figure.figsize' : [8, 6]})        # 그림 크기, 기본값 [6, 4]
  plt.rcParams.update({'font.size' : '15'})               # 글자 크기, 기본값 10
  plt.rcParams.update({'font.family' : 'Malgun Gothic'})  # 폰트, 기본값 sans-serif
  
  # 여러 요소를 한 번에 설정하기
  plt.rcParams.update({'figure.dpi'     : '150',
                       'figure.figsize' : [8, 6],
                       'font.size'      : '15',
                       'font.family'    : 'Malgun Gothic'})
  
  # 모든 설정 되돌리기
  plt.rcdefaults()
```

<br>

> 설명 메시지 숨기기 - 코드 뒤에 ; 삽입
```
  sns.scatterplot(data = mpg, x = 'displ', y = 'hwy');
```

<br>

---

<br>

막대 그래프 - 집단 간 차이 표현하기
---
### 막대 그래프(bar chart)
- 데이터의 크기를 막대의 길이로 표현한 그래프

- 성별 소득 차이처럼 집단 간 차이를 표현할 때 사용

|막대그래프|
|-|
|![image](https://github.com/user-attachments/assets/a4786f1e-7004-4f77-85a0-87ebdfca6d81)|

<br>

### 평균 막대 그래프 만들기
#### 1. 집단별 평균표 만들기
```
  df_mpg = mpg.groupby('drv') \
              .agg(mean_hwy = ('hwy', 'mean'))
  df_mpg
```

<br>

##### 💡 as_index = False : 변수를 인덱스로 바꾸지 않고 원래대로 유지
- 앞 코드의 출력 결과

  - 집단을 나타낸 변수 drv가 인덱스로 바뀌어 mean_hwy보다 아래에 표시됨

- seaborn 으로 그래프를 만들려면 값이 변수에 담겨있어야 함

```
  df_mpg = mpg.groupby('drv', as_index = False) \
              .agg(mean_hwy = ('hwy', 'mean'))
  df_mpg
```

<br>

#### 2. 그래프 만들기
```
  sns.barplot(data = df_mpg, x = 'drv', y = 'mean_hwy')
```

<br>

#### 3. 크기순으로 정렬하기
```
  # 데이터 프레임 정렬하기
  df_mpg = df_mpg.sort_values('mean_hwy', ascending = False)
  
  # 막대 그래프 만들기
  sns.barplot(data = df_mpg, x = 'drv', y = 'mean_hwy')
```

<br>

### 빈도 막대 그래프 만들기
- 빈도 막대 그래프 : 값의 빈도(개수)를 막대 길이로 표현한 그래프

- 여러 집단의 빈도를 비교할 때 사용

<br>

#### 1. 집단별 빈도표 만들기
```
  # 집단별 빈도표 만들기
  df_mpg = mpg.groupby('drv', as_index = False) \
              .agg(n = ('drv', 'count'))
  
  df_mpg
```

<br>

#### 2. 그래프 만들기
```
  # 막대 그래프 만들기
  sns.barplot(data = df_mpg, x = 'drv', y = 'n')
```

<br>

### sns.countplot() 으로 빈도 막대 그래프 만들기
- 집단별 빈도표 만드는 작업 생략하고 원자료를 이용해 곧바로 빈도 막대 그래프 만듦

```
  # 빈도 막대 그래프 만들기
  sns.countplot(data = mpg, x = 'drv')
```

<br>

#### 💡 sns.barplot()와 sns.countplot() x축 순서가 다른 이유
- mpg 의 drv : f, 4, r 순

  - 데이터 프레임에서 변수의 값 순서는 데이터 프레임에 입력된 행 순서를 따름
 
  - mpg의 0~6행 f, 7~17행 4, 18~27행 r

```
  mpg['drv'].unique()
```

- df_mpg 의 drv : 알파벳순

  - groupby()로 데이터 프레임을 요약하면 값의 순서가 알파벳순으로 바뀜

```
  df_mpg['drv'].unique()
```

<br>

### 막대 정렬하기
```
  # 4, f, r 순으로 막대 정렬
  sns.countplot(data = mpg, x = 'drv', order = ['4', 'f', 'r'])
```

<br>

> 빈도 높은 순으로 정렬하기
```
  # drv의 값을 빈도가 높은 순으로 출력
  mpg['drv'].value_counts().index
```
```
  # drv 빈도 높은 순으로 막대 정렬
  sns.countplot(data = mpg, x = 'drv', order = mpg['drv'].value_counts().index)
```

<br>

---

<br>

선 그래프 - 시간에 따라 달라지는 데이터 표현하기
---
### 선 그래프(line chart)
- 데이터를 선으로 표현한 그래프

- 시간에 따라 달라지는 데이터를 표현할 때 사용

  - ex) 환율, 주가지수 등 경제지표가 시간에 따라 변하는 양상
 
- 시계열 데이터(time series data)

  - 일별 환율처럼, 일정 시간 간격을 두고 나열된 데이터
 
- 시계열 그래프(time series chart)

  - 시계열 데이터를 선으로 표현한 그래프

<br>

|선 그래프|
|-|
|![image](https://github.com/user-attachments/assets/5c928ae9-b9ac-47b8-84ac-9e7b02ce44f2)|

<br>

### 시계열 그래프 만들기
```
  # economics 데이터 불러오기
  economics = pd.read_csv('economics.csv')
  economics.head()
```
```
sns.lineplot(data = economics, x = 'date', y = 'unemploy')
```
- x축에 굵은 선이 표시되어 있음

- '연월일'을 나타낸 문자가 x축에 여러 번 겹쳐 표시되었기 때문

<br>

### x축에 연도 표시하기
#### 1. 날짜 시간 타입 변수 만들기
```
  # 날짜 시간 타입 변수 만들기
  economics['date2'] = pd.to_datetime(economics['date'])
  
  # 변수 타입 확인
  economics.info()
```
```
  economics[['date', 'date2']]
```
```
  # 연 추출
  economics['date2'].dt.year
```
```
  # 월 추출
  economics['date2'].dt.month
```
```
# 일 추출
economics['date2'].dt.day
```

<br>

#### 2. 연도 변수 만들기
```
  # 연도 변수 추가
  economics['year'] = economics['date2'].dt.year
  economics.head()
```

<br>

#### 3. x축에 연도 표시하기
```
  # x축에 연도 표시
  sns.lineplot(data = economics, x = 'year', y = 'unemploy')
```
```
  # 신뢰구간 제거
  sns.lineplot(data = economics, x = 'year', y = 'unemploy', errorbar = None)
```

<br>

---

<Br>

상자 그림 - 집단 간 분포 차이 표현하기
---
### 상자 그림(box plot)
- 데이터의 분포 또는 퍼져 있는 형태를 직사각형 상자 모양으로 표현한 그래프

- 데이터가 어떻게 분포하고 있는지 알 수 있음

- 평균값만 볼 때보다 데이터의 특징을 더 자세히 이해할 수 있음

<br>

|상자 그림|
|-|
|![image](https://github.com/user-attachments/assets/4408eb2a-05a3-4678-9f8f-66992f80d69c)|

<br>

### 상자 그림 만들기
```
  sns.boxplot(data = mpg, x = 'drv', y = 'hwy')
```

|결과|
|-|
|![image](https://github.com/user-attachments/assets/3bfc1242-5e7d-483e-bf15-be61bcf0c909)|


- 전륜구동(f)

  - 26~29 사이의 좁은 범위에 자동차가 모여 있는 뾰족한 형태의 분포
 
  - 수염의 위아래에 점 표식이 있으므로 연비가 극단적으로 높거나 낮은 자동차들

- 4륜구동(4)

  - 17~22 사이에 자동차 대부분이 모여있음
 
  - 중앙값이 상자 밑면에 가까우므로 낮은 값 쪽으로 치우친 형태의 분포
 
- 후륜구동(r)

  - 17~24 사이의 넓은 범위에 자동차가 분포
 
  - 수염이 짧고 극단치가 없으므로 자동차 대부분이 사분위 범위에 해당

<br>

> 해석

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

#### 💡 seaborn 더 알아보기
- seaborn 치트 시트

  - [DataCamp seaborn cheat sheet](bit.ly/easypy_86)

- seaborn으로 만든 다양한 그래프와 코드들

  - [seaborn Example gallery](bit.ly/easypy_87)
 
  - [The Python Graph Gallery - Seaborn](bit.ly/easypy_seaborn)
 
- matplotlib 패키지 함께 사용하기

  - [matplotlib 공식 문서](matplotlib.org)

<br>

---

<br>

정리
---
|seaborn 함수|그래프|
|-|-|
|sns.scatterplot()|산점도|
|sns.barplot()|막대 그래프 - 요약표 활용|
|sns.countplot()|빈도 막대 그래프 - 원자료 활용|
|sns.lineplot()|선 그래프|
|sns.boxplot()|상자 그림|

<br>

### 1. 산점도
```
sns.scatterplot(data = mpg, x = 'displ', y = 'hwy')

# 축 제한
sns.scatterplot(data = mpg, x = 'displ', y = 'hwy') \
   .set(xlim = [3, 6], ylim = [10, 30])

# 종류별로 표식 색깔 바꾸기
sns.scatterplot(data = mpg, x = 'displ', y = 'hwy', hue = 'drv')
```

<br>

### 2. 막대 그래프

#### 평균 막대 그래프
```
# 1단계. 평균표 만들기
df_mpg = mpg.groupby('drv', as_index = False) \
            .agg(mean_hwy = ('hwy', 'mean'))

# 2단계. 그래프 만들기
sns.barplot(data = df_mpg, x = 'drv', y = 'mean_hwy')
```

<br>

#### 빈도 막대 그래프
```
sns.countplot(data = mpg, x = 'drv')
```

<br>

### 3. 선 그래프
```
sns.lineplot(data = economics, x = 'date', y = 'unemploy')
```

<br>

### 4. 상자 그림
```
sns.boxplot(data = mpg, x = 'drv', y = 'hwy')
```

<br>
