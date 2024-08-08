# 데이터 분석 기초
데이터 파악하기
---
### 데이터를 파악할 때 사용하는 명령어
|함수|기능|
|-|-|
|head()|앞부분 출력|
|tail()|뒷부분 출력|
|shape|행, 열 개수 출력|
|info()|변수 속성 출력|
|describe()|요약 통계량 출력|

<br>

### exam 데이터 파악하기
#### 준비하기
```
  import pandas as pd
  exam = pd.read_csv('exam.csv')
```

<br>

#### head() - 데이터 앞부분 확인하기
```
  exam.head()  # 앞에서부터 5행까지 출력
```
```
  exam.head(10)  # 앞에서부터 10행까지 출력
```

<br>

#### tail() - 데이터 뒷부분 확인하기
```
  exam.tail()  # 뒤에서부터 5행까지 출력
```
```
  exam.tail(10)  # 뒤에서부터 10행까지 출력
```

<br>

#### shape - 데이터가 몇 행, 몇 열로 구성되는지 알아보기
```
  exam.shape
```

<br>

#### info() - 변수 속성 파악하기
```
  exam.info()
```
- Non-Null Count : 결측치(누락된 값)을 제외하고 구한 값의 개수

- 변수 속성

  - int64(정수)
 
  - float64(실수)
 
  - object(문자)
 
  - datetime64(날짜 시간)
 
- 64 : 64비트

  - 1 비트로 두 개의 값 표현 가능
 
  - int64 : 2^64개의 정수 표현 가능

<br>

#### describe() - 요약 통계량 구하기
```
  exam.describe()
```

|출력값|통계량|설명|
|-|-|-|
|count|빈도(frequency)|값의 개수|
|mean|평균(mean)|모든 값을 더해 값의 개수로 나눈 값|
|std|표준편차(standard deviation)|변수의 값들이 평균엥서 떨어진 정도를 나타낸 값|
|min|최소값(minimum)|가장 작은 값|
|25%|1사분위수(1st quantile)|하위 25%(4분의 1) 지점에 위치한 값|
|50%|중앙값(median)|하위 50%(중앙) 지점에 위치한 값|
|75%|3사분위수(3rd quantile)|하위 75%(4분의 3) 지점에 위치한 값|
|max|최대값(maximum)|가장 큰 값|

<br>

### mpg 데이터 파악하기
```
  # mpg 데이터 불러오기
  mpg = pd.read_csv('mpg.csv')
```
```
  mpg.head()  # mpg 앞부분 확인
```
```
  mpg.tail()  # mpg 뒷부분 확인
```
```
  mpg.shape  #  행, 열 출력
```
```
  mpg.info()  #  데이터 속성 확인
```
```
  mpg.describe()  #  요약 통계량 출력
```
```
  mpg.describe(include = 'all')  #  문자 변수 요약 통계량 함께 출력
```

|출력값|통계량|설명|
|-|-|-|
|count|빈도|값의 개수|
|unique|고유값 빈도|중복을 제거한 범주의 개수|
|top|최빈값|개수가 가장 많은 값|
|freq|최빈값 빈도|개수가 가장 많은 값의 개수|

<br>

---

<br>

메서드와 어트리뷰트
---
|-|
|-|
|![image](https://github.com/user-attachments/assets/825648fb-4345-475f-ab98-8bed8847f057)|

<br>

### [메서드 vs 함수](./02)

<br>

### 어트리뷰트(attribute)
- 변수가 지니고 있는 값

<br>

> 메서드
```
  df.head()
```

<br>

> 어트리뷰트
```
  df.shape
```

<br>

#### 변수의 자료 구조에 따라 지니고 있는 어트리뷰트가 다름
> 데이터 프레임
```
  df.shape
```

<br>

> 리스트
```
  var.shape
```

<br>

---

<br>

변수명 바꾸기
---
### 1. 데이터 프레임 만들기
```
  df_raw = pd.DataFrame({'var1' : [1, 2, 1],
                         'var2' : [2, 3, 2]})
  df_raw
```

<br>

### 2. 데이터 프레임 복사본 만들기
- 오류가 발생하더라도 원 상태로 되돌리기 가능

- 데이터를 비교하면서 변형되는 과정 검토 가능

```
  df_new = df_raw.copy()  # 복사본 만들기
  df_new                  # 출력
```

<br>

### 3. 변수명 바꾸기
```
  df_new = df_new.rename(columns = {'var2' : 'v2'})  # var2를 v2로 수정
  df_new
```

<br>

> 비교하기
```
  df_raw
```

<br>

#### 💡 데이터 프레임을 복사할 때 df.copy()를 사용하는 이유
- df_new = df_raw 와 같이 작성하면

  - df_new 와 df_raw 는 이름만 다를 뿐 한 몸처럼 항상 같은 값 가짐

- 어느 한쪽 수정하면 다른 한쪽도 수정됨

- 복사본을 수정해도 원본은 영향받지 않도록 df.copy() 사용

<br>

파생변수(derived variable) 만들기
---
- 기존의 변수를 변형해 만든 변수

|파생변수|
|-|
|![image](https://github.com/user-attachments/assets/f0890f6d-8ff1-4ac8-992a-44d84cd613ee)|

<br>

### 변수 조합해 파생변수 만들기
```
  df = pd.DataFrame({'var1' : [4, 3, 8],
                     'var2' : [2, 6, 1]})
  df
```
```
  df['var_sum'] = df['var1'] + df['var2']         # var_sum 파생변수 만들기
  df
```
```
  df['var_mean'] = (df['var1'] + df['var2']) / 2  # var_mean 파생변수 만들기
  df
```

<br>

### mpg 통합 연비 변수 만들기
```
  mpg['total'] = (mpg['cty'] + mpg['hwy']) / 2  # 통합 연비 변수 만들기
  mpg.head()
```

<br>

#### 파생변수 분석하기
```
  sum(mpg['total']) / len(mpg)  # total 합계를 행 수로 나누기
```
```
  mpg['total'].mean()           # 통합 연비 변수 평균
```

<br>

### 조건문을 활용해 파생변수 만들기
#### 1. 기준값 정하기
```
  mpg['total'].describe()  # 요약 통계량 출력
```
```
  mpg['total'].plot.hist()  # 그래프 만들기
```

<br>

#### 2. 합격 판정 변수 만들기
|-|
|-|
|![image](https://github.com/user-attachments/assets/eff53ae5-2cb4-4cb2-a496-252051dde177)|

```
  import numpy as np
  
  # 20 이상이면 pass, 그렇지 않으면 fail 부여
  mpg['test'] = np.where(mpg['total'] >= 20, 'pass', 'fail')
  mpg.head()
```

<br>

#### 3. 빈도표로 합격 판정 자동차 수 살펴보기
```
  mpg['test'].value_counts()  # 연비 합격 빈도표 만들기
```

<br>

#### 4. 막대 그래프로 빈도 표현하기
```
  count_test = mpg['test'].value_counts()  # 연비 합격 빈도표를 변수에 할당
  count_test.plot.bar()                    # 연비 합격 빈도 막대 그래프 만들기
```

<br>

> 축 이름 회전하기
```
  count_test.plot.bar(rot = 0)  # 축 이름 수평으로 만들기
```

<br>

### 중첩 조건문 활용하기
|등급|기준|
|:-:|:-:|
|A|30 이상|
|B|20 ~ 29|
|C|20 미만|

#### 1. 연비 등급 변수 만들기
```
  # total 기준으로 A, B, C 등급 부여
  mpg['grade'] = np.where(mpg['total'] >= 30, 'A',
                 np.where(mpg['total'] >= 20, 'B', 'C'))
  
  # 데이터 확인
  mpg.head()
```

<br>

#### 2. 빈도표와 막대 그래프로 연비 등급 살펴보기
```
  count_grade = mpg['grade'].value_counts()  # 등급 빈도표 만들기
  count_grade
```
```
  count_grade.plot.bar(rot = 0)              # 등급 빈도 막대 그래프 만들기
```

<br>

##### 알파벳순으로 막대 정렬하기
```
  # 등급 빈도표 알파벳순 정렬
  count_grade = mpg['grade'].value_counts().sort_index()
  count_grade
```
```
  count_grade.plot.bar(rot = 0)
```

<br>

#### 3. 필요한 만큼 범주 만들기 : '범주의 수 - 1'
```
  # A, B, C, D 등급 변수 만들기
  mpg['grade2'] = np.where(mpg['total'] >= 30, 'A',
                  np.where(mpg['total'] >= 25, 'B',
                  np.where(mpg['total'] >= 20, 'C', 'D')))
```

<br>

### 목록에 해당하는 행으로 변수 만들기
- | : 버티컬 바(vertical bar)

  -  '또는(or)' 을 의미하는 기호
 
  -  [Shift] + [\] 로 입력
 
```
  mpg['size'] = np.where((mpg['category'] == 'compact') |
                         (mpg['category'] == 'subcompact') |
                         (mpg['category'] == '2seater'),
                         'small', 'large')
  
  mpg['size'].value_counts()
```
- np.where()에 여러 조건 입력할 때 각 조건에 괄호 입력 주의

<br>

#### df.isin() 사용하기
```
  mpg['size'] = np.where(mpg['category'].isin(['compact', 'subcompact',
  '2seater']), 'small', 'large')
  
  mpg['size'].value_counts()
```

<br>







