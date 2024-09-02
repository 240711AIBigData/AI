# 데이터 파악하기
### 1. 패키지 로드
```Python
  import pandas as pd
  import numpy as np
```

<br>

### 2. 데이터 불러오기
```Python
  mpg = pd.read_csv('mpg.csv')
```

<br>

### 3. 데이터 파악하기
```Python
  mpg.head()      # 데이터 앞부분
  mpg.tail()      # 데이터 뒷부분
  mpg.shape       # 행, 열 수
  mpg.info()      # 속성
  mpg.describe()  # 요약 통계량
```

<br>

### 4. 변수명 바꾸기
```Python
  mpg = mpg.rename(columns = {'manufacturer' : 'company'})
```

<br>

### 5. 파생변수 만들기
```Python
  mpg['total'] = (mpg['cty'] + mpg['hwy'])/2                  # 변수 조합
  mpg['test'] = np.where(mpg['total'] >= 20, 'pass', 'fail')  # 조건문 활용
```

<br>

### 6. 빈도 확인하기
```Python
  count_test = mpg['test'].value_counts()  # 빈도표 만들기
  count_test.plot.bar(rot = 0)             # 빈도 막대 그래프 만들기
```

<br>
