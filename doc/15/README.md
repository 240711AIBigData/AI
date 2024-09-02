# 텍스트 마이닝

|-|
|-|
|![image](https://github.com/user-attachments/assets/15153e3d-96a7-4c22-a236-f105aa31ec7e)|

<br>

대통령 연설문 텍스트 마이님
---
### 형태소 분석(morphology analysis)
- 문장을 구성하는 어절들의 품사를 분석

- 텍스트 마이닝할 때 가장 먼저 하는 작업

- 명사, 동사, 형용사 등 의미를 지닌 품사를 추출해 빈도 확인

<br>

### 문재인 대통령의 출마 선언문
- 대통령 연설문은 문법 오류 없고 문장이 정제됨

- 전처리 작업을 많이 하지 않아도 되므로 텍스트 마이닝 익히는 데 적합

<br>

### KoNLPy 패키지 설치하기
#### 1. 자바 설치하기
- 윈도우 설정 → 시스템 → 정보 → 장치 사양 → 시스템 종류

  - 운영 체제 버전 확인
 
- [JDK](abit.ly/easypy_101)에서 운영체제 버전에 맞는 설치 파일 다운로드 및 설치

  - \'플랫폼' 항목에서 맞는 운영 체제 버전 선택
 
    - 64비트 : Windows x64
   
    - 32비트 : Windows x86
   
<br>

#### 2. KoNLPy 의존성 패키지 설치하기
```Python
  pip install jpype1
```
- 의존성 패키지 : 패키지가 의존하는 패키지

- 어떤 패키지는 다른 패키지의 기능을 이용하므로 의존성 패키지를 먼저 설치해야 작동함

- KoNLPy 의 의존성 패키지인 jpype1 설치하기

<br>

#### 3. KoNLPy 설치하기
```Python
  pip install konlpy
```

<br>

### 가장 많이 사용된 단어 알아보기
#### 1. 연설문 불러오기
- open() 으로 파일을 열고 read() 로 불러오기

- encoding = 'UTF-8' : 불러올 텍스트 파일의 인코딩을 'UTF-8'로 지정

```Python
  moon = open('speech_moon.txt', encoding = 'UTF-8').read()
  moon
```

<br>

#### 2. 불필요한 문자 제거하기
- 특수 문자, 한자, 공백은 분석 대상 아니므로 제거하기

- re.sub() 로 한글이 아닌 모든 문자를 공백으로 바꾸기

- [^가-힣] : '한글이 아닌 모든 문자'를 의미하는 정규 표현식(regular expression)

```Python
  # 불필요한 문자 제거하기
  import re
  moon = re.sub('[^가-힣]', ' ', moon)
  moon
```

<br>

#### 3. 명사 추출하기
- 명사를 보면 텍스트가 무엇에 관한 내용인지 파악 가능

```Python
  # hannanum 만들기
  import konlpy
  hannanum = konlpy.tag.Hannanum()
  
  # 명사 추출하기
  hannanum.nouns("대한민국의 영토는 한반도와 그 부속도서로 한다")
```
```Python
  # 연설문에서 명사 추출하기
  nouns = hannanum.nouns(moon)
  nouns
```

<br>

##### 📌 리스트를 다루기 쉽게 데이터 프레임으로 변환하기
```Python
  # 데이터 프레임으로 변환
  import pandas as pd
  df_word = pd.DataFrame({'word' : nouns})
  df_word
```

<br>

#### 4. 단어 빈도표 만들기
- str.len() : 글자 수 세기

```Python
  # 글자 수 추가
  df_word['count'] = df_word['word'].str.len()
  df_word
```
```Python
  # 두 글자 이상 단어만 남기기
  df_word = df_word.query('count >= 2')
  df_word.sort_values('count')
```
```Python
  # 단어 빈도 구하기
  df_word = df_word.groupby('word', as_index = False) \  # 단어 별 분리
                   .agg(n = ('word', 'count')) \         # 빈도 구하기
                   .sort_values('n', ascending = False)  # 내림차순 정렬
  df_word
```

<br>

#### 5. 단어 빈도 막대 그래프 만들기
```Python
  # 단어 빈도 상위 20개 추출
  top20 = df_word.head(20)
  top20
```
```Python
  import seaborn as sns
  import matplotlib.pyplot as plt
  
  plt.rcParams.update({'font.family'    : 'Malgun Gothic',  # 한글 폰트 설정
                       'figure.dpi'     : '120',            # 해상도 설정
                       'figure.figsize' : [6.5, 6]})        # 가로 세로 크기 설정
  
  # 막대 그래프 만들기
  sns.barplot(data = top20, y = 'word', x = 'n')
```
- 맥 사용시 'Malgun Gothic' 대신 'AppleGothic' 폰트 설정

<br>

### 워드 클라우드(word cloud) 만들기
- 단어의 빈도에 따라 글자의 크기와 색깔을 다르게 표현함

- 어떤 단어가 얼마나 많이 사용됐는지 한눈에 파악할 수 있음

<br>

#### 1. wordcloud 패키지 설치하기
```Python
  pip install wordcloud
```

<br>

##### 💡 wordcloud 패키지 설치 오류 해결하기
> wordcloud 패키지 설치 중 다음 에러 메시지가 출력되면 'Microsoft Visual C++' 설치
```Python
  error: Microsoft Visual C++ 14.0 or greater is required. Get it with “Microsoft C++ Build Tools”
```
- (1) [여기](bit.ly/easypy_103)에서 왼쪽 위 [Build Tools 다운로드] 버튼 클릭해 설치 파일 다운로드
 
- (2) 설치 파일 실행, 설정 화면에서 왼쪽 위 'C++를 사용한 데스크톱 개발' 체크

  - 오른쪽 아래 [Install] 클랙히 설치 시작

- (3) 설치 끝나면 아나콘다 프롬프트에서 wordcloud 패키지 설치

<br>

#### 2. 한글 폰트 설정하기
```Python
  font = 'DoHyeon-Regular.ttf'
```

<br>

#### 3. 단어와 빈도를 담은 딕셔너리 만들기
> 단어는 키(key), 빈도는 값(value)으로 구성된 딕셔너리 만들기
```Python
  # 데이터 프레임을 딕셔너리로 변환
  dic_word = df_word.set_index('word').to_dict()['n']
  dic_word
```

<br>

#### 4. 워드 클라우드 만들기
```Python
  # wc 만들기
  from wordcloud import WordCloud
  wc = WordCloud(random_state = 1234,         # 난수 고정
                 font_path = font,            # 폰트 설정
                 width = 400,                 # 가로 크기
                 height = 400,                # 세로 크기
                 background_color = 'white')  # 배경색
```
```Python
  # 워드 클라우드 만들기
  img_wordcloud = wc.generate_from_frequencies(dic_word);
  
  # 워드 클라우드 출력하기
  plt.figure(figsize = (10, 10))  # 가로, 세로 크기 설정
  plt.axis('off')                 # 테두리 선 없애기
  plt.imshow(img_wordcloud)       # 워드 클라우드 출력
```

<br>

##### 💡 워드 클라우드 만들 때 주의하기
- 워드 클라우드 이미지 출력 코드는 한 셀에 넣어 함께 실행해야 함

- WordCloud() 는 실행할 때마다 난수를 이용해 워드 클라우드를 매번 다른 모양으로 만듦

- 항상 같은 모양으로 만들려면?

  - random_state 를 이용해 난수 고정
 
  - wc 만드는 코드를 먼저 실행
 
  - wc.generate_from_frequencies() 실행

<br>

### 워드 클라우드 모양 바꾸기
#### 1. mask 만들기
> 이미지 파일 불러오기
```Python
  import PIL  
  icon = PIL.Image.open('cloud.png')
```

<br>

> mask 만들기
```Python
  import numpy as np
  img = PIL.Image.new('RGB', icon.size, (255, 255, 255))
  img.paste(icon, icon)
  img = np.array(img)
```

<br>

#### 2. 워드 클라우드 만들기
- mask = img : mask 적용하기

```Python
  # wc 만들기
  wc = WordCloud(random_state = 1234,         # 난수 고정
                 font_path = font,            # 폰트 설정
                 width = 400,                 # 가로 크기
                 height = 400,                # 세로 크기
                 background_color = 'white',  # 배경색
                 mask = img)                  # mask 설정
```
```Python
  # 워드 클라우드 만들기
  img_wordcloud = wc.generate_from_frequencies(dic_word);
  
  # 워드 클라우드 출력하기
  plt.figure(figsize = (10, 10))  # 가로, 세로 크기 설정
  plt.axis('off')                 # 테두리 선 없애기
  plt.imshow(img_wordcloud)       # 워드 클라우드 출력
```

<br>

### 워드 클라우드 색깔 바꾸기
- colormap = 'inferno' : 워드 클라우드에 inferno 컬러맵 적용

- 컬러맵(colormaps) : 색깔 목록

```Python
  # wc 만들기
  wc = WordCloud(random_state = 1234,         # 난수 고정
                 font_path = font,            # 폰트 설정
                 width = 400,                 # 가로 크기
                 height = 400,                # 세로 크기
                 background_color = 'white',  # 배경색
                 mask = img,                  # mask 설정
                 colormap = 'inferno')        # 컬러맵 설정
```
```Python
  # 워드 클라우드 만들기
  img_wordcloud = wc.generate_from_frequencies(dic_word);
  
  # 워드 클라우드 출력하기
  plt.figure(figsize = (10, 10))  # 가로, 세로 크기 설정
  plt.axis('off')                 # 테두리 선 없애기
  plt.imshow(img_wordcloud)       # 워드 클라우드 출력
```

<br>

##### 💡 다양한 컬러맵 이용하기
- 워드 클라우드에 적용할 수 있는 다양한 컬러맵

  - [Choosing Colormaps in Matplotlib](bit.ly/easypy_104)
 
<br>

##### 💡 워드 클라우드는 좋은 그래프인가?
- 워드 클라우드는 분석 결과를 정확하게 표현하는 그래프는 아님

- 단어 빈도를 크기와 색으로 표현

  - '어떤 단어가 몇 번 사용됐는지' 정확히 알 수 없음
 
- 단어 배치가 산만

  - '어떤 단어가 다른 언어보다 얼마나 더 많이 사용됐는지' 비교 어려움
 
- 아름답게 표현하는게 아니라 정확하게 표현하려면 워드 클라우드보다 막대 그래프 이용

<br>

---

<br>

기사 댓글 텍스트 마이닝
---
### 가장 많이 사용된 단어 알아보기
#### 1. 기사 댓글 불러오기
- news_comment_BTS.csv 

  - 방탄소년단 '빌보드 핫 100 차트' 1위 소식을 다룬 네이버 뉴스 기사 댓글

```Python
  # 데이터 불러오기
  df = pd.read_csv('news_comment_BTS.csv', encoding = 'UTF-8')
  
  # 데이터 살펴보기
  df.info()
```

<br>

#### 2. 불필요한 문자 제거하기
- reply 는 데이터 프레임에 담겨 있는 변수이므로 str.replace() 를 사용해 불필요 문자 제거

```Python
  # 불필요한 문자 제거하기
  df['reply'] = df['reply'].str.replace('[^가-힣]', ' ', regex = True)
  df['reply'].head()
```

<br>

#### 3. 명사 추출하기
- 꼬꼬마(Kkma) 형태소 분석기 이용해 명사 추출

  - 띄어쓰기 오류 있는 문장에서도 형태소를 잘 추출하는 장점
 
  - 댓글처럼 정제되지 않은 텍스트 분석에 적합
 
```Python
  # kkma 만들기
  import konlpy
  kkma = konlpy.tag.Kkma()
```
- Kkma() 의 첫 글자 K 대문자 주의

<br>

> 명사 추출

- reply 는 데이터 프레임에 들어있는 변수이므로 kkma.nouns() 에 바로 적용 불가

- 함수가 각 행의 값을 따로따로 처리하도록 apply() 사용

```Python
  # 명사 추출 - apply() 활용
  nouns = df['reply'].apply(kkma.nouns)
  nouns
```

<br>

#### 4. 단어 빈도표 만들기
- nouns 는 행마다 여러 단어가 리스트 자료 구조로 들어 있음

- df.explode() 를 이용해 한 행에 한 단어만 들어가도록 변경

```Python
  # 한 행에 한 단어가 들어가도록 구성
  nouns = nouns.explode()
  nouns
```
```Python
  # 데이터 프레임 만들기
  df_word = pd.DataFrame({'word' : nouns})
  
  # 글자 수 추가
  df_word['count'] = df_word['word'].str.len()
  
  # 두 글자 이상 단어만 남기기
  df_word = df_word.query('count >= 2')
  df_word
```
```Python
  # 빈도표 만들기
  df_word = df_word.groupby('word', as_index = False) \  # 단어 별 분리
                   .agg(n = ('word', 'count')) \         # 빈도 구하기
                   .sort_values('n', ascending = False)  # 내림차순 정렬
  df_word
```

<br>

#### 5. 단어 빈도 막대 그래프 만들기
```Python
# 단어 빈도 상위 20개 추출
top20 = df_word.head(20)
top20
```
```Python
  # 가로 세로 크기 설정
  plt.rcParams.update({'figure.figsize': [6.5, 6]})
  
  # 막대 그래프 만들기
  sns.barplot(data = top20, y = 'word', x = 'n')
```

<br>

### 워드 클라우드 만들기
```Python
  # 데이터 프레임을 딕셔너리로 변환
  dic_word = df_word.set_index('word').to_dict()['n']
  
  # wc 만들기
  wc = WordCloud(random_state = 1234,         # 난수 고정
                 font_path = font,            # 폰트 설정
                 width = 400,                 # 가로 크기
                 height = 400,                # 세로 크기
                 background_color = 'white',  # 배경색
                 mask = img)                  # mask 설정
```
```Python
  # 워드 클라우드 만들기
  img_wordcloud = wc.generate_from_frequencies(dic_word)
  
  # 워드 클라우드 출력하기
  plt.figure(figsize = (10, 10))  # 가로, 세로 크기 설정
  plt.axis('off')                 # 테두리 선 없애기
  plt.imshow(img_wordcloud)       # 워드 클라우드 출력
```

<br>

##### 💡 텍스트 마이닝 더 알아보기
- 다양한 형태소 분석기 이용하기

  - KoNLPy 를 이용하면 한나눔, 꼬꼬마 외에도 다양한 형태소 분석기 이용 가능
 
  - 명사뿐 아니라 동사, 형용사 등 다양한 품사 추출 가능
 
  - [KoNLPy 공식 문서](konlpy.org/ko/latest)

<br>

