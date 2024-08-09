![image](https://github.com/user-attachments/assets/7618a426-5156-45c1-9d36-9f91792211bf)# 마크다운으로 데이터 분석 보고서 만들기
|-|
|-|
|![image](https://github.com/user-attachments/assets/326ca044-c84d-43c0-951f-a6699d3c880f)|

<br>

신뢰할 수 있는 데이터 분석 보고서 만들기
---
### 코드와 결과물이 설명 글과 함께 어우러진 데이터 분석 보고서
- 독자가 분석 과정을 명확히 이해 가능

- 보고서의 코드를 직접 실행하면서 똑같은 결과가 출력되는지 확인 가능

- 자신의 분석 작업에 활용 가능

<br>

### 마크다운(markdown) 활용
- 데이터 분석 전 과정을 담은 보고서 작성 가능

- HTML, 워드, PDF 등 다양한 포맷으로 저장 가능

- 별도의 문서 작성 소프트웨어 사용하지 않아도 OK

<br>

### 신뢰받는 데이터 분석 보고서
- 재현성(reproducibility) 갖춰야 함

- 똑같은 분석 과정을 거쳤을 때 똑같은 분석 결과 반복

- 마크다운 이용하면 재현성 갖춘 데이터 분석 보고서 작성 가능

<br>

---

<br>

마크다운 문서 만들기
---
- 노트북의 셀 타입을 마크다운으로 변경

- [Shift + Enter] 로 셀을 실행하면 글자 양식이 적용되어 보기 좋게 바뀜

<br>

### 마크다운 문법 이용하기
- 문자 앞뒤에 특수 문자를 넣어 문자 양식 정함

<br>

#### 입력 및 출력 확인
- 문자 앞뒤에 \*특수문자* 넣으면 기울임체 (*특수문자*)

- 문자 앞뒤에 \*\*특수문자** 넣으면 강조체 (**특수문자**)

- 문자 앞뒤에 \~\~특수문자~~ 넣으면 취소선 (~~특수문자~~)

- 문자 앞뒤에 [특수문자]\(http://www.google.com/search?q=special+character) 넣으면 하이퍼링크 ([특수문자](http://www.google.com/search?q=special+character))

- 단계별 제목

  - # : 1단계 (#)
 
  - ## : 2단계 (##)
 
  - ### : 3단계 (###)
 
  - #### : 4단계 (####)

- 코드에 백틱 기호 입력시 `pandas` 음영 (\`pandas`)

<br>

#### 입력한 내용
|-|
|-|
|![image](https://github.com/user-attachments/assets/61de231e-e6b6-4b77-ab95-196a8fc2bf52)|

- 첫 번째 셀은 마크다운 타입

- 두 번째, 세 번째 셀은 코드 타입

<br>

#### 출력된 내용
|-|
|-|
|![image](https://github.com/user-attachments/assets/f7cce07d-4b19-4b23-b7d0-920880f4c832)|

<br>

### 문서 파일로 저장하기
#### HTML 파일로 저장하기
- File → Save and Export Notebook As... → HTML

<br>

#### PDF 파일로 저장하기
- File → Print → '대상'을 'PDF로 저장'으로 바꾸기 → 저장

<br>

#### 워드 파일로 저장하기
- [pandoc](pandoc.org/installing.html)에서 설치 파일 다운로드 및 설치

- JupyterLab 재실행

- 새 노트북에서 다음 명령어 실행

```
  !pandoc report.ipynb -s -o report.docx
```

<br>

#### 💡 마크다운 활용하기
- 마크다운 목차
  - \# 을 넣어 제목을 만든 다음 JupyterLab 사이드바 Table of Contents 아이콘 클릭
 
  - 목차 클릭하면 노트북에서 제목이 있는 셀로 바로 이동

|-|
|-|
|![image](https://github.com/user-attachments/assets/dcc3b3aa-9d5f-43ae-b63d-db2e6e34bf62)|

<br>

- 마크다운 치트 시트

  - [Markdown Cheat Sheet](bit.ly/easypy_131)

<br>




