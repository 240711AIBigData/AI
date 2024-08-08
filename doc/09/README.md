|-|
|-|
|![image](https://github.com/user-attachments/assets/2adc97c5-d553-4e3e-9fc2-284670d2720c)|

# 데이터 전처리(data preprocessing)
- 분석에 적합하게 데이터를 가공하는 작업
- pandas : 전처리 작업에 가장 많이 사용되는 패키지

|함수|기능|
|:-:|-|
|query()|행 추출|
|df[]|열(변수) 추출|
|sort_values()|정렬|
|groupby()|집단별로 나누기|
|assign()|변수 추가|
|agg()|통계치 구하기|
|merge()|데이터 합치기(열)|
|concat()|데이터 합치기(행)|

<br>

### df.query() : 조건에 맞는 데이터만 추출하기
|-|
|-|
|![image](https://github.com/user-attachments/assets/18be4cd0-4c4e-4f22-adad-1efea8420a06)|

<br>

#### 데이터 불러오기
```
  import pandas as pd
  exam = pd.read_csv('exam.csv')
  exam
```
```
  # exam에서 nclass가 1인 경우만 추출
  exam.query('nclass == 1')
```
```
  # 2반인 경우만 추출
  exam.query('nclass == 2')
```
```
  # 1반이 아닌 경우
  exam.query('nclass != 1')
```
```
  # 3반이 아닌 경우
  exam.query('nclass != 3')
```

<br>

### 초과, 미만, 이상, 이하 조건 걸기
```
  # 수학 점수가 50점을 초과한 경우
  exam.query('math > 50')
```
```
  # 수학 점수가 50점 미만인 경우
  exam.query('math < 50')
```
```
  # 영어 점수가 50점 이상인 경우
  exam.query('english >= 50')
```
```
  # 영어 점수가 80점 이하인 경우
  exam.query('english <= 80')
```

<br>

### 여러 조건을 충족하는 행 추출하기
```
  # 1반이면서 수학 점수가 50점 이상인 경우
  exam.query('nclass == 1 & math >= 50')
```
```
  # 2반이면서 영어 점수가 80점 이상인 경우
  exam.query('nclass == 2 & english >= 80')
```

<br>

### 여러 조건 중 하나 이상 충족하는 행 추출하기
```
  # 수학 점수가 90점 이상이거나 영어 점수가 90점 이상인 경우
  exam.query('math >= 90 | english >= 90')
```
```
  # 영어 점수가 90점 미만이거나 과학 점수가 50점 미만인 경우
  exam.query('english < 90 | science < 50')
```

<br>

### 목록에 해당하는 행 추출하기
```
  # 1, 3, 5반에 해당하면 추출
  exam.query('nclass == 1 | nclass == 3 | nclass == 5')
```
```
  # 1, 3, 5반에 해당하면 추출
  exam.query('nclass in [1, 3, 5]')
```

<br>

### 추출한 행으로 데이터 만들기
```
  # nclass가 1인 행 추출해 nclass1에 할당
  nclass1 = exam.query('nclass == 1')
  
  # nclass가 2인 행 추출해 nclass2에 할당
  nclass2 = exam.query('nclass == 2')
```
```
  nclass1['math'].mean()  # 1반 수학 점수 평균 구하기
```
```
  nclass2['math'].mean()  # 2반 수학 점수 평균 구하기
```

<br>

### 문자 변수를 이용해 조건에 맞는 행 추출하기
> \'전체 조건'과 '추출할 문자'에 서로 다른 모양 따옴표 입력
```
  df = pd.DataFrame({'sex'     : ['F', 'M', 'F', 'M'],
                     'country' : ['Korea', 'China', 'Japan', 'USA']})
  df
```
```
  # 전체 조건에 작은따옴표, 추출할 문자에 큰따옴표 사용
  df.query('sex == "F" & country == "Korea"')
```
```
  # 전체 조건에 큰따옴표, 추출할 문자에 작은따옴표 사용
  df.query("sex == 'M' & country == 'China'")
```

<br>

#### 💡 같은 모양 따옴표 사용하면 에러
```
# 전체 조건과 추출할 문자에 모두 작은따옴표 사용
df.query('sex == 'F' & country == 'Korea'')
```

<br>

### 

<br>

#### 💡 파이썬에서 사용하는 기호
|논리 연산자|기능|     |산술 연산자|기능|
|:-:|-|-|:-:|-|
|<|작다|   |+|더하기|
|<=|작거나 같다|   |-|빼기|
|>|크다|   |*|곱하기|
|>=|크거나 같다|   |**|제곱|
|==|같다|   |/|나누기|
|!=|같지 않다|   |//|나눗셈의 몫|
|\||또는(or)|   |%|나눗셈의 나머지|
|&|그리고(and)|   |   |   |
|in|매칭 확인|   |   |   |

<br>

#### 💡 외부 변수를 이용해 추출하기
> 별도의 변수를 이용해 행을 추출하려면 변수명 앞에 @를 붙여서 조건 입력
```
  var = 3
  exam.query('nclass == @var')
```

<br>

#### 💡 데이터 프레임 출력 제한 설정하기
- 데이터 프레임 크면 중간 생략함
  - 60행 넘기면 위아래 5행씩 10행만 출력
  - 20열 넘기면 좌우 10열씩 20열만 출력

<br>

- 행, 열 제한 없이 모두 출력하기
```
  pd.set_option('display.max_rows', None)    # 모든 행 출력하도록 설정
  pd.set_option('display.max_columns' None)  # 모든 열 출력하도록 설정
```
- JupyterLab 이나 커널(Kernel)을 새로 실행하면 원래대로 돌아감
  - 커널 새로 실행 : JupyterLab 상단 메뉴 [Kernel → Restart Kernel...] 클릭

<br>

- 새로 실행하지 않고 설정 되돌리기
```
  pd.reset_option('display.max_rows')     # 행 출력 제한 되돌리기
  pd.reset_option('display.max_columns')  # 열 출력 제한 되돌리기
  pd.reset_option('all')                  # 모든 설정 되돌리기
```

<br>

df.loc[]와 df.iloc[]의 차이점
---
|기능|df.loc[]|df.iloc[]|
|-|-|-|
|행 추출 인덱스|문자열, 번호|번호|
|열 추출 인덱스|문자열|번호
|조건 지정해서 행 추출|O|X|
|연속된 행 추출 x:y|이상:이하|이상:미만|

<br>

