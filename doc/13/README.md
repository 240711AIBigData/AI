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







