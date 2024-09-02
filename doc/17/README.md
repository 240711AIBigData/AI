# 인터렉티브 그래프
|-|
|-|
|![image](https://github.com/user-attachments/assets/be2948f7-7df3-4737-9b19-fcfb46a6bd3e)|

<br>

인터랙티브 그래프(interactive graph) 만들기
---
- 마우스 움직임에 반응하며 실시간으로 모양이 변하는 그래프

- 그래프를 자유롭게 조작하면서 관심 있는 부분을 자세히 살펴볼 수 있음

- HTML 포맷으로 저장하면 웹 브라우저에서 그래프 조작 가능

<br>

### 산점도 만들기
- plotly 로 만든 그래프는 마우스 움직임에 반응함

- 산점도의 표식에 마우스 커서를 올리면 값이 나타남

```Python
  import pandas as pd
  mpg = pd.read_csv('mpg.csv')
  
  import plotly.express as px
  px.scatter(data_frame = mpg, x = 'cty', y = 'hwy', color = 'drv')
```

<br>

### 막대 그래프 만들기
- 막대에 마우스 커서를 올리면 해당 항목의 값이 나타남

- 범례의 항목을 클릭하면 비교할 막대 선택 가능

```Python
  # 자동차 종류별 빈도 구하기
  df = mpg.groupby('category', as_index = False) \
          .agg(n = ('category', 'count'))
  df
```
```Python
  px.bar(data_frame = df, x = 'category', y = 'n', color = 'category')
```

<br>

### 선 그래프 만들기
- 선 위에 마우스 커서를 올리면 날짜와 값이 나타남

- 드래그하여 특정 영역을 지정하면 x, y축의 범위를 지정할 수 있음

```Python
  # economics 불러오기
  economics = pd.read_csv('economics.csv')
  
  # 선 그래프 만들기
  px.line(data_frame = economics, x = 'date', y = 'psavert')
```

<br>

### 상자 그림 만들기
- 상자 그림 위에 마우스 커서를 올리면 요약 통계량이 나타남

- 극단치 표식 위에 마우스 커서를 올리면 극단치가 나타남

- 범례의 항목을 클릭하면 비교할 범주 선택 가능

```Python
  # 상자 그림 만들기
  px.box(data_frame = mpg, x = 'drv', y = 'hwy', color = 'drv')
```

<br>

### HTML 파일로 저장하기
```Python
  # 그래프를 변수에 할당하기
  fig = px.scatter(data_frame = mpg, x = 'cty', y = 'hwy')
  
  # html로 저장하기
  fig.write_html('scatter_plot.html')
```

<br>

#### 💡 plotly 활용하기
> 그래프 크기 조절하기
```Python
  px.scatter(data_frame = mpg, x = 'hwy', y = 'cty', color = 'drv',
             width = 600, height = 400)
```

<br>

> 새 창에 그래프 출력하기
```Python
  import plotly
  plotly.io.renderers.default = 'browser'
  
  # 설정 원래대로 되돌리기
  plotly.io.renderers.default = 'jupyterlab'
```

<br>























