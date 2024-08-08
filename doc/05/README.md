# 외부 데이터

- 엑셀 관련 유용한 라이브러리
  - [openpyxl](https://pypi.org/project/openpyxl/)

<br>

외부 데이터 이용하기
---
### 엑셀 파일 불러오기
```
  df_exam = pd.read_excel('excel_exam.xlsx')  # 엑셀 파일을 불러와 df_exam에 할당
  df_exam                                     # 출력
```
- 워킹 디렉터리에 불러올 파일이 있어야 함 - 확인: !cd

<br>

> 경로 직접 지정
```
  df_exam = pd.read_excel('c:/easy_python/excel_exam.xlsx')
```

<br>

### 분석하기
#### 연산으로 평균 구하기
```
  sum(df_exam['english']) / 20
```
```
  sum(df_exam['science']) / 20
```

<br>

#### len() 이용해 평균 구하기
```
  # 변수의 값 개수 구하기
  x = [1, 2, 3, 4, 5]
  x
```
```
  len(x)
```

<br>

```
  # 데이터 프레임의 행 개수 구하기
  df = pd.DataFrame({'a' : [1, 2, 3],
                     'b' : [4, 5, 6]})
  df
```
```
  len(df)
```

<br>

```
  # english 합계를 행 개수로 나누기
  sum(df_exam['english']) / len(df_exam)
```
```
  # science 합계를 행 개수로 나누기
  sum(df_exam['science']) / len(df_exam)
```

<br>

#### 엑셀 파일의 첫 번째 행이 변수명이 아니라면?
```
  df_exam_novar = pd.read_excel('excel_exam_novar.xlsx', header = None)
  df_exam_novar
```
- None의 첫 글자 N 대문자 유의

<br>

#### 엑셀 파일에 시트가 여러 개 있다면?
```
  # Sheet2 시트의 데이터 불러오기
  df_exam = pd.read_excel('excel_exam.xlsx', sheet_name = 'Sheet2')
  
  # 세 번째 시트의 데이터 불러오기
  df_exam = pd.read_excel('excel_exam.xlsx', sheet_name = 2)
```
- 숫자를 0부터 센다는 점 유의

<br>

---

<br>

### CSV  파일 불러오기
```
  df_csv_exam = pd.read_csv('exam.csv')
  df_csv_exam
```

<br>

### 데이터 프레임을 CSV 파일로 저장하기
#### 1. 데이터 프레임 만들기
```
  df_midterm = pd.DataFrame({'english' : [90, 80, 60, 70],
                           'math'    : [50, 60, 100, 20],
                           'nclass'  : [1, 1, 2, 2]})
  df_midterm
```

<br>

#### 2. CSV 파일로 저장하기
```
  df_midterm.to_csv('output_newdata.csv')
```

<br>

> 인덱스 번호 제외하고 저장
```
  df_midterm.to_csv('output_newdata.csv', index = False)
```

<br>

정리하기
---
### 1. 데이터 프레임 만들기
```
df = pd.DataFrame({'name'    : [' 김지훈 ', ' 이유진 ', ' 박동현 ', ' 김민지 '],
                   'english' : [90, 80, 60, 70],
                   'math'    : [50, 60, 100, 20]})
```

<br>

### 2. 외부 데이터 이용하기
```
  #  엑셀 파일 불러오기
  df_exam = pd.read_excel('excel_exam.xlsx')
  
  # CSV  파일 불러오기
  df_csv_exam = pd.read_csv('exam.csv')
  
  # CSV 파일로 저장하기
  df_midterm.to_csv('output_newdata.csv')
```

<br>
