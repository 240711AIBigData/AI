# 필요한 변수만 추출하기
df[] : 변수 추출하기
---
|-|
|-|
|![image](https://github.com/user-attachments/assets/fb5dbbbf-a1bb-4385-b3e2-c8b039da17b6)|

<br>

변수 추출하기
---
> 변수 추출하기
```
  exam['math']  # math 추출

  exam['english']  # english 추출
```

<br>

> 여러 변수 추출하기
```
  exam[['nclass', 'math', 'english']]  # nclass, math, english 추출
```

<br>

### 📌 변수가 1개일 때 데이터 프레임 유지하기
- 변수명을 []로 한번 더 감싸기

> 시리즈로 추출
```
  exam['math']
```

> 데이터 프레임으로 추출
```
  exam[['math']]
```

<br>

변수 제거하기
---
> 변수 제거하기
```
  exam.drop(columns = 'math')  # math 제거

  exam.drop(columns = ['math', 'english'])  # math, english 제거
```

<br>

pandas 함수 조합하기
---
### query()와 [] 조합하기
> 1반 학생의 영어 점수 추출
```
  # nclass가 1인 행만 추출한 다음 english 추출
  exam.query('nclass == 1')['english']
```

<br>

> 수학 점수가 50점 이상인 학생의 id와 math 변수 추출
```
  # math가 50 이상인 행만 추출한 다음, id와 math 추출
  exam.query('math >= 50')[['id', 'math']]
```

<br>

> 일부만 출력하기
```
  # math가 50 이상인 행만 추출한 다음, id와 math 앞부분 5행까지 추출
  exam.query('math >= 50')[['id', 'math']].head()

  # math가 50 이상인 행만 추출한 다음, id와 math 앞부분 10행까지 추출
  exam.query('math >= 50')[['id', 'math']].head(10)
```

<br>

가독성 있게 코드 줄 바꾸기
---
- 명령어 끝난 부분 뒤에 백슬래시(\) 입력 후 enter 로 줄바꿈

- Spacebar 나 Tab 을 이용해 간격 띄우고 다음 명령어 입력

  - \ 뒤에 아무것도 입력하면 안 됨(주석, 띄어쓰기 X)

<br>

> 실제 코드에서는 \ 뒤에 주석 달기 불가
```
  exam.query('math >= 50')\  # math 가 50 이상인 행만 추출
      [['id', 'math']]\      # id, math 추출
      .head()                # 앞부분 10행 추출
```

<br>

# 파생변수 추가하기
df.assign() : 파생변수 추가하기
---
|-|
|-|
|![image](https://github.com/user-attachments/assets/f78239c9-5ebc-430b-9a59-6a8218ff04a8)|

<br>

파생변수 추가하기
---
> 파생변수 추가
```
  # total 변수 추가
  exam.assign(total = exam['math'] + exam['english'] + exam['science'])
```

<br>

> 여러 파생변수 한번에 추가하기
```
  exam.assign(
  total = exam['math'] + exam['english'] + exam['science'],        # total 추가
  mean = (exma['math'] + exam['english'] + exam['science'] / 3)    # mean 추가
```

<br>

> df.assign()에 np.where() 적용하기
```
  import numpy as np
  exam.assign(test = np.where(exam['science'] >= 60, 'pass', 'fall'))
```

<br>

> 추가한 변수를 pandas 함수에 바로 활용하기
```
  # total 변수 추가, total 기준 정렬
  exam.assign(total = exam['math'] + exam['english'] + exam['science'])\
      .sort_values('total)
```

<br>

> lambda 이용해 데이터 프레임명 줄여쓰기
```
  # 긴 데이터 프레임명 지정
  long_name = pd.read_csv('./input/exam.scv')

  # long_name 직접 입력
  long_name.assign(new = long_name['math'] + long_name['english'] + long_name['science'])

  # long_name 대신 x 입력
  long_name.assign(new = lambda x: x['math'] + x['english'] + x['science'])
```

<br>

> 앞에서 만든 변수를 활용해 다시 변수 만들기
```
  # 반드시 lambda 를 이용해 데이터 프레임명을 약어로 입력
  exam.assign(total = exam['math'] + exam['english'] + exam['science'],
              mean = lambda x: x['total'] / 3)
```
- 위 코드는 데이터 프레임명을 서로 다른 문자로 입력하여 읽기 불편

<br>

- 파생 변수를 만드는 행에 lambda를 각각 입력하면 데이터 프레임명 통일하여 가독성 증가
```
  exam.assign(total = lambda x: x['math'] + x['english'] + x['science'],
              mean = lambda x: x['total'] / 3
```

<br>

#### ✨ lambda 이용하지 않고 데이터 프레임명 직접 입력하면 에러
```
  exam.assign(total = exam['math'] + exam['english'] + exam['science'],
              mean = exam['total'] / 3)
```
> 실행결과
```
  KeyError: 'total'
```

<br>

df.groupby(), df.agg() : 집단별로 요약하기
---
|-|
|-|
|![image](https://github.com/user-attachments/assets/97e00852-34df-49c5-96c2-fe8012451df2)|

<br>

집단별로 요약하기
---
> 전체 요약 통계량 구하기
```
  # math 평균 구하기
  exam.agg(mean_math = ('math', 'mean'))
```
- 함수명 뒤에 () 넣지 않음

<br>

> 집단별 요약 통계량 구하기
```
  exam.groupby('nclass')\                  # nclass 별로 분리하기
      .agg(mean_math = ('math', 'mean'))   # math 평균 구하기
```

<br>

#### 📌 변수를 인덱스로 바꾸지 않기
- as_index = False : 변수를 인덱스로 바꾸지 않고 원래대로 유지

  - 앞 코드의 출력 결과에서 변수명 nclass 가 인덱스(index)로 바뀌어 mean_math 보다 밑에 표시됨
 
  - groupby() 의 기본값이 변수를 인덱스로 바꾸도록 설정되어 있기 때문
 
  - 인덱스(index) : 값이 데이터 프레임의 어디에 있는지 '값의 위치를 나타낸 값'

> ex
```
  exam.groupby('nclass', as_index = False)\
      .agg(mean_math = ('math', 'mean'))
```

<br>

> 여러 요약 통계량 한 번에 구하기
```
  exam.groupby('nclass')\                      # nclass 별로 분리
      .agg(mean_math = ('math', 'mean'),       # 수학 점수 평균
           sum_math = ('math', 'sum'),         # 수학 점수 합계
           median_math = ('math', 'median'),   # 수학 점수 중앙값
           n = ('nclass', 'count'))            # 빈도(학생 수)
```

<br>

agg()에 자주 사용하는 요약 통계량 함수
---
|함수|통계량|
|-|-|
|mean()|평균|
|std()|표준편차|
|sum()|합계|
|median()|중앙값|
|min()|최소값|
|max()|최대값|
|count()|빈도(개수)|

<br>

#### 📌 모든 변수의 요약 통계량 한 번에 구하기
- df.groupby() 에 agg() 대신 mean(), sum() 과 같은 요약 통계량 함수 적용

```
  exam.groupby('nclass').mean()
```


<br>

집단별로 다시 집단 나누기
---
> 집단을 나눈 다음 다시 하위 집단으로 나누기
```
  mpg.groupby(['manufacturer', 'drv'])\   # 제조 회사 및 구동 방식별 분리
     .agg(mean_cty = ('cty', 'mean))      # cty 평균 구하기
```

<br>

#### 📌 value_countes() 로 집단별 빈도 간단하게 구하기
```
  mpg.groupby('drv')\
     .agg(n = ('drv', 'count'))

  mpg['drv'].value_counts()
```
- 짧은 코드로 빈도 구하기 가능, 자동으로 빈도 기준 내림차순 정렬
- 출력 결과가 데이터 프레임이 아닌 시리즈(series) 자료 구조이므로 qeury() 적용 불가

<br>

> ex
```
  mpg['drv'].value_counts().query('n > 100')
```

> 실행결과
```
  'Series' object has no attribute 'query'
```

<br>

pandas 함수 조합하기
---
- 제조 회사별로 'suv' 자동차의 도시 및 고속도로 합산 연비 평균을 구해 내림차순으로 정렬하고, 1~5위까지 출력
> 함수 사용 절차

|절차|기능|pandas 함수|
|1|suv 추출|query()|
|2|합산 연비 변수 만들기|assign()|
|3|회사별로 분리|groupby()|
|4|합산 연비 평균 구하기|agg()|
|5|내림차순 정렬|sort_values()|
|6|1~5위까지 출력|head()|

<br>

> 코드
```
  mpg.query('category == "suv"') \                        # suv 추출
     .assign(total = (mpg['hwy'] + mpg['cty']) / 2) \     # 합산 연비 변수 만들기
     .groupby('manufacturer') \                           # 제조 회사별로 분리
     .agg(mean_tot = ('total', 'mean')) \                 # 합산 연비 평균 구하기
     .sort_values('mean_tot', ascending = False) \        # 내림차순 정렬
     .head()                                              # 1~5위까지 출력
```

<br>

# 데이터 합치기
df.merge() : 가로로 합치기
---
|-|
|-|
|![image](https://github.com/user-attachments/assets/16d14840-e73d-4a92-8223-be42369ad0b1)|

<br>

df.concat() : 세로로 합치기
---
|-|
|-|
|![image](https://github.com/user-attachments/assets/787f6653-61d3-4ea0-bd03-6bfcf4117fcd)|

<br>

### 가로로 합치기
- pd.merge()에 결합할 데이터 프레임명 나열

- how  = 'left': 오른쪽에 입력한 데이터 프레임을 왼쪽 데이터 프레임에 결합

- on: 데이터를 합칠 때 기준으로 삼을 변수명 입력

```
  # 중간고사 데이터 만들기
  test1 = pd.DataFrame({'id'      : [1, 2, 3, 4, 5],
                        'midterm' : [60, 80, 70, 90, 85]})
  
  # 기말고사 데이터 만들기
  test2 = pd.DataFrame({'id'    : [1, 2, 3, 4, 5],
                        'final' : [70, 83, 65, 95, 80]})

  # id 기준으로 합쳐서 total에 할당
  total = pd.merge(test1, test2, how = 'left', on = 'id')
  total
```

<br>

> 다른 데이터를 활용해 변수 추가하기
```
  name = pd.DataFrame({'nclass'  : [1, 2, 3, 4, 5],
                       'teacher' : ['kim', 'lee', 'park', 'choi', 'jung']})

  # nclass 기준으로 합쳐서 exam_new에 할당
  exam_new = pd.merge(exam, name, how = 'left', on = 'nclass')
  exam_new
```

<br>

### 세로로 합치기
- 결합할 데이터 프레임명을 []를 이용해 나열
  
- 인덱스 중복 안되도록 새로 부여하려면 pd.concat()에 ignore_index = True

```
  # 학생 1~5번 시험 데이터 만들기
  group_a = pd.DataFrame({'id'   : [1, 2, 3, 4, 5],
                          'test' : [60, 80, 70, 90, 85]})
  
  # 학생 6~10번 시험 데이터 만들기
  group_b = pd.DataFrame({'id'   : [6, 7, 8, 9, 10],
                          'test' : [70, 83, 65, 95, 80]})
  
  # 데이터 합쳐서 group_all에 할당
  group_all = pd.concat([group_a, group_b])
  group_all
```

<br>

📌 pandas 더 알아보기
- 치트 시트(cheat sheet) : 패키지 사용법을 요약한 매뉴얼

  - [Pandas Cheat Sheet](bit.ly/easypy_pandas)

- pandas 공식 문서 검색하기

  - [Pandas Documentation](pandas.pydata.org/docs)
 
<br>

