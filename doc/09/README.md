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

