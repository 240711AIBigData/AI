# 외부 데이터 가져오기

- 엑셀 관련 유용한 라이브러리
  - [openpyxl](https://pypi.org/project/openpyxl/)

<br>


데이터 프레임 만들기
---
```
  df = pd.DataFrame({'name'    : [' 김지훈 ', ' 이유진 ', ' 박동현 ', ' 김민지 '],
                     'english' : [90, 80, 60, 70],
                     'math'    : [50, 60, 100, 20]})
```

<br>

외부 데이터 이용하기
---
### 엑셀 파일 불러오기
```
  df_exam = pd.read_excel('excel_exam.xlsx')
```

<br>


### CSV  파일 불러오기
```
  df_csv_exam = pd.read_csv('exam.csv')
```

<br>

### CSV 파일로 저장하기
```
  df_midterm.to_csv('output_newdata.csv')
```

<br>
