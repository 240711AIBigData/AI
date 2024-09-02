# 패키지와 모듈
패키지(packages)
---
- 함수가 여러 개 들어잇는 꾸러미

- 함수 사용하려면 패키지 설치 먼저

- 아나콘다에 주요 패키지 대부분 들어있음

|-|
|-|
|![image](https://github.com/user-attachments/assets/e728e42c-9fa3-4f1c-ac35-c210c22183f9)|

<br>

### 패키지 로드하기
```Python
  import seaborn
```

<br>

### 패키지 함수 사용하기
```Python
  var = ['a', 'a', 'b', 'c']
  var
```
```Python
  seaborn.countplot(x = var)
```

<br>

### 패키지 약어 활용하기
```Python
  import seaborn as sns
  sns.countplot(x = var)
```

<br>

### seaborn의 titanic 데이터로 그래프 만들기
#### load_dataset() 으로 titanic 데이터 불러오기
```Python
  df = sns.load_dataset('titanic')
  df
```

<br>

#### 함수의 다양한 기능 이용하기
```Python
  sns.countplot(data = df, x = 'sex')
```
```Python
  # x축 class
  sns.countplot(data = df, x = 'class')  
```
```Python
  # x축 class, alive별 색 표현
  sns.countplot(data = df, x = 'class', hue = 'alive')
```
```Python
  # y축 class, alive별 색 표현
  sns.countplot(data = df, y = 'class', hue = 'alive')
```

<br>

#### 💡 함수 사용법이 궁금할 땐 Help 함수 활용
```Python
  sns.countplot?
```

<br>

모듈
---
|-|
|-|
|![image](https://github.com/user-attachments/assets/876dac95-592a-4e25-bc6d-b9ba99936f38)|

<br>

> \`패키지명.모듈명.함수명()`으로 함수 사용하기
```Python
  import sklearn.metrics
  sklearn.metrics.accuracy_score()
```

<br>

> \`모듈명.함수명()`으로 함수 사용하기
```Python
  from sklearn import metrics
  metrics.accuracy_score()
```

<br>

> \`함수명()`으로 함수 사용하기
```Python
  from sklearn.metrics import accuracy_score
  accuracy_score()
```

<br>

#### 💡 as로 약어 지정하기
```Python
  import sklearn.metrics as met
  met.accuracy_score()
  
  from sklearn import metrics as met
  met.accuracy_score()
  
  from sklearn.metrics import accuracy_score as accuracy
  accuracy()
```

<br>

### 패키지 설치하기
```Python
  pip install pydataset
```

<br>

### 패키지 함수 사용하기
```Python
  import pydataset
  pydataset.data()
```
```Python
  df = pydataset.data('mtcars')  # mtcars 데이터를 df에 할당
  df                             # df 출력
```

<br>

