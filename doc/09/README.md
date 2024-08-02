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

파이썬에서 사용하는 기호
---
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

외부 변수를 이용해 추출하기
---
> 별도의 변수를 이용해 행을 추출하려면 변수명 앞에 @를 붙여서 조건 입력
```
  var = 3
  exam.query('nclass == @var')
```

<br>

데이터 프레임 출력 제한 설정하기
---
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

# 요약
## 1. 조건에 맞는 데이터만 추출하기
```
  exam.query('english <= 80')
  exam.query('nclass == 1 & math >= 50')    # 여러 조건 동시 충족
  exam.query('math >= 90 | english >= 90')  # 여러 조건 중 하나 이상 충족
  exam.query('nclass in [1, 3, 5]')
```

<br>

## 2. 필요한 변수만 추출하기
```
  exam['math']                              # 한 변수 추출
  exam[['nclass', 'math', 'english']]       # 여러 변수 추출
  exam.drop(columns = 'math')               # 변수 제거
  exam.drop(columns = ['math', 'english'])  # 여러 변수 제거
```

<br>

## 3. pandas 명령어 조합하기
```
  exam.query('math >= 50')[['id', 'math']].head()
```

<br>

## 4. 순서대로 정렬하기
```
  exam.sort_values('math')                     # 오름차순 정렬
  exam.sort_values('math', ascending = False)  # 내림차순 정렬
  
  # 여러 변수 기준 정렬
  exam.sort_values(['nclass', 'math'], ascending = [True, False])
```


<br>

## 5. 파생변수 추가하기
```
  exam.assign(total = exam['math'] + exam['english'] + exam['science'])
  
  # 여러 파생변수 한 번에 추가하기
  exam.assign(total = exam['math'] + exam['english'] + exam['science'],
              mean = (exam['math'] + exam['english'] + exam['science']) / 3)
  
  # assign()에 np.where() 적용하기
  exam.assign(test = np.where(exam['science'] >= 60, 'pass', 'fall'))
  
  # 추가한 변수를 pandas 코드에 바로 활용하기
  exam.assign(total = exam['math'] + exam['english'] + exam['science']) \
      .sort_values('total') \
      .head()
```

<br>

## 6. 집단별로 요약하기
```
  exam.groupby('nclass') \
      .agg(mean_math = ('math', 'mean'))
  
  # 각 집단별로 다시 집단 나누기
  mpg.groupby(['manufacturer', 'drv']) \
     .agg(mean_cty = ('cty', 'mean'))
```

<br>

## 7. 데이터 합치기
```
  pd.merge(test1, test2, how = 'left', on = 'id')  # 가로로 합치기
  pd.concat([group_a, group_b])                    # 세로로 합치기
```

<Br>
