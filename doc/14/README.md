'한국복지패널 데이터' 분석 준비
---
### 한국복지패널 데이터
- 한국보건사회연구원 발간 조사 자료

- 전국 7,000여 가구 선정, 2006년부터 매년 추적 조사한 자료

- 경제활동, 생활실태, 복지욕구 등 천여 개 변수로 구성됨

- 다양한 분야의 연구자, 정책 전문가들이 활용

- 엄밀한 절차로 수집되고 다양한 변수가 있으므로 데이터 분석 연습하기 좋은 재료

<br>

### 데이터 분석 준비하기
#### 1. 데이터 준비하기
- [Koweps_hpwc14_2019_beta2.sav](https://www.koweps.re.kr:442/data/data/list.do) 파일을 워킹 디렉터리에 삽입

- 2020년 발간 복지패널 데이터

  - 6,331 가구, 14,418 명의 정보를 담고 있음

<br>

#### 2. 패키지 설치 및 로드하기
- sav : 통계 분석 소프트웨어 SPSS 전용 파일

- pyreadstat 패키지 설치하면 pandas 패키지를 이용해 불러올 수 있음

<br>

> (1) 패키지 설치
```
  pip install pyreadstat
```

<br>

> (2) 패키지 로드
```
  import pandas as pd
  import numpy as np
  import seaborn as sns
```

<br>

#### 3. 데이터 불러오기
```
  # 데이터 불러오기
  raw_welfare = pd.read_spss('Koweps_hpwc14_2019_beta2.sav')
  
  # 복사본 만들기 
  welfare = raw_welfare.copy()
```

<br>

#### 4. 데이터 검토하기
```
  welfare             # 앞부분, 뒷부분 출력
  welfare.shape       # 행, 열 개수 출력
  welfare.info()      # 변수 속성 출력
  welfare.describe()  # 요약 통계량
```
- 규모가 큰 데이터는 변수명을 쉬운 단어로 바꾸고 분석할 변수를 하나씩 살펴봐야 함

<br>

#### 5. 변수명 바꾸기
```
  welfare = welfare.rename(columns = {'h14_g3'     : 'sex',            #  성별
                                      'h14_g4'     : 'birth',          #  태어난 연도
                                      'h14_g10'    : 'marriage_type',  #  혼인 상태
                                      'h14_g11'    : 'religion',       #  종교 
                                      'p1402_8aq1' : 'income',         #  월급 
                                      'h14_eco9'   : 'code_job',       #  직업 코드
                                      'h14_reg7'   : 'code_region'})   #  지역 코드
```
- 규모가 큰 조사 자료는 데이터의 특징을 설명해 놓은 코드북(codebook)을 함께 제공함

- 코드북에 코드로 된 변수명과 값의 의미가 설명되어 있음

- 데이터의 특징을 알 수 있고 분석 방향 아이디어를 얻을 수 있음

- [Koweps_Codebook_2019.xlsx](https://github.com/240711AIBigData/AI/blob/38f9e85056f7b81358bcb5eb7b296ae35f87fe62/doc/Data/Koweps_Codebook_2019.xlsx) 참고

<br>

### 데이터 분석 절차 살펴보기

|-|
|-|
|![image](https://github.com/user-attachments/assets/7cedcaf0-cd5c-45d4-b0a7-37b69b63f5af)|

<br>

#### 1단계 - 변수 검토 및 전처리
- 분석에 활용할 변수 전처리

  - 변수의 특징 파악, 이상치와 결측치 정제
 
  - 변수의 값을 다루기 편하게 바꾸기

- 분석에 활용할 변수 각각 전처리

  - ex) 성별에 따른 월급 차이 : 성별, 월급 각각

<br>

#### 2단계 - 변수 간 관계 분석
- 변수 간 관계 분석

  - 데이터 요약 표, 그래프 만들기
 
  - 분석 결과 해석

<br>

---

<br>

성별에 따른 월급 차이
---
### 성별 변수 검토 및 전처리하기
#### 1. 변수 검토하기
```
  welfare['sex'].dtypes  # 변수 타입 출력
```
```
  welfare['sex'].value_counts()  # 빈도 구하기
```

<br>

#### 2. 전처리하기
```
  # 이상치 확인
  welfare['sex'].value_counts()
```
```
  # 이상치 결측 처리
  welfare['sex'] = np.where(welfare['sex'] == 9, np.nan, welfare['sex'])
  
  # 결측치 확인
  welfare['sex'].isna().sum()
```
```
  # 성별 항목 이름 부여
  welfare['sex'] = np.where(welfare['sex'] == 1, 'male', 'female')
  
  # 빈도 구하기
  welfare['sex'].value_counts()
```
```
  # 빈도 막대 그래프 만들기
  sns.countplot(data = welfare, x = 'sex')
```

<br>

### 월급 변수 검토 및 전처리하기
#### 1. 변수 검토하기
```
  welfare['income'].dtypes  # 변수 타입 출력
```
```
  welfare['income'].describe()  # 요약 통계량 구하기
```
```
  sns.histplot(data = welfare, x = 'income')  # 히스토그램 만들기
```

<br>

#### 2. 전처리하기
```
  welfare['income'].describe()  # 이상치 확인
```
```
  welfare['income'].isna().sum()  # 결측치 확인
```
```
  # 이상치 결측 처리
  welfare['income'] = np.where(welfare['income'] == 9999, np.nan, welfare['income'])
  
  # 결측치 확인
  welfare['income'].isna().sum()
```

<br>

### 성별에 따른 월급 찿이 분석하기
#### 1. 성별 월급 평균표 만들기
```
  ## 성별 월급 평균표 만들기
  sex_income = welfare.dropna(subset = 'income') \            # income 결측치 제거
                      .groupby('sex', as_index = False) \     # sex 별 분리
                      .agg(mean_income = ('income', 'mean'))  # income 평균 구하기
  sex_income
```

<br>

#### 2. 그래프 만들기
```
  # 막대 그래프 만들기
  sns.barplot(data = sex_income, x = 'sex', y = 'mean_income')
```

<br>

---

<br>

나이와 월급의 관계
---
### 나이 변수 검토 및 전처리하기
#### 1. 변수 검토하기
```
  welfare['birth'].dtypes  # 변수 타입 출력
```
```
  welfare['birth'].describe()  # 요약 통계량 구하기
```
```
  sns.histplot(data = welfare, x = 'birth')  # 히스토그램 만들`
```

<br>

#### 2. 전처리하기
```
  welfare['birth'].describe()  # 이상치 확인
```
```
  welfare['birth'].isna().sum()  # 결측치 확인
```
```
  # 이상치 결측 처리
  welfare['birth'] = np.where(welfare['birth'] == 9999, np.nan, welfare['birth'])
  
  # 결측치 확인
  welfare['birth'].isna().sum()
```

#### 3. 파생변수 만들기 - 나이
```
  welfare = welfare.assign(age = 2019 - welfare['birth'] + 1)  # 나이 변수 만들기
  welfare['age'].describe()                                    # 요약 통계량 구하기
```
```
  sns.histplot(data = welfare, x = 'age')  # 히스토그램 만들기
```

<br>

### 나이와 월급의 관계 분석하기
#### 1. 나이에 따른 월급 평균표 만들기
```
  # 나이별 월급 평균표 만들기
  age_income = welfare.dropna(subset = 'income') \            # income 결측치 제거
                      .groupby('age') \                       # age 별 분리
                      .agg(mean_income = ('income', 'mean'))  # income 평균 구하기
  
  age_income.head()
```

<br>

#### 2. 그래프 만들기
```
  # 선 그래프 만들기
  sns.lineplot(data = age_income, x = 'age', y = 'mean_income')
```

<br>

---

<br>

연력대에 따른 월급 차이
---
### 연령대 변수 검토 및 전처리하기
#### 파생변수 만들기 - 연령대
```
  # 나이 변수 살펴보기
  welfare['age'].head()
```
```
  # 연령대 변수 만들기
  welfare = welfare.assign(ageg = np.where(welfare['age'] <  30, 'young',
                                  np.where(welfare['age'] <= 59, 'middle', 'old')))
  
  # 빈도 구하기
  welfare['ageg'].value_counts()
```
```
  # 빈도 막대 그래프 만들기
  sns.countplot(data = welfare, x = 'ageg')
```

<br>

### 연령대에 따른 월급 차이 분석하기
#### 1. 연령대별 월급 평균표 만들기
```
# 연령대별 월급 평균표 만들기
ageg_income = welfare.dropna(subset = 'income') \            # income 결측치 제거
                     .groupby('ageg', as_index = False) \    # ageg 별 분리
                     .agg(mean_income = ('income', 'mean'))  # income 평균 구하기
ageg_income
```

<br>

#### 2. 그래프 만들기
```
  # 막대 그래프 만들기
  sns.barplot(data = ageg_income, x = 'ageg', y = 'mean_income')
```
```
  # 막대 정렬하기
  sns.barplot(data = ageg_income, x = 'ageg', y = 'mean_income',
              order = ['young', 'middle', 'old'])
```

<br>

---

<br>

연령대 및 성별 월급 차이
---
### 연령대 및 성별 월급 차이 분석하기
#### 1. 연령대 및 성별 월급 평균표 만들기
```
# 연령대 및 성별 평균표 만들기
sex_income = welfare.dropna(subset = 'income') \                  # income 결측치 제거
                    .groupby(['ageg', 'sex'], as_index = False) \ # ageg 및 sex 별 분리
                    .agg(mean_income = ('income', 'mean'))        # income 평균 구하기
sex_income
```

<br>

#### 2. 그래프 만들기
```
  # 막대 그래프 만들기
  sns.barplot(data = sex_income, x = 'ageg', y = 'mean_income', hue = 'sex',
              order = ['young', 'middle', 'old'])
```

<br>

### 나이 및 성별 월급 차이 분석하기
```
  # 나이 및 성별 월급 평균표 만들기
  sex_age = welfare.dropna(subset = 'income') \                  # income 결측치 제거
                   .groupby(['age', 'sex'], as_index = False) \  # age 및 sex 별 분리
                   .agg(mean_income = ('income', 'mean'))        # income 평균 구하기
  sex_age.head()
```
```
  # 선 그래프 만들기
  sns.lineplot(data = sex_age, x = 'age', y = 'mean_income', hue = 'sex')
```

<br>

직업별 월급 차이
--
### 직업 변수 검토 및 전처리하기
#### 1. 변수 검토하기
```
  welfare['code_job'].dtypes  # 변수 타입 출력
```
```
  welfare['code_job'].value_counts()  # 빈도 구하기
```

<br>

#### 2. 전처리하기
```
  list_job = pd.read_excel('Koweps_Codebook_2019.xlsx', sheet_name = '직종코드')
  list_job.head()
```
```
  list_job.shape  # 행, 열 개수 출력
```
```
    # welfare에 list_job 결합하기
    welfare = welfare.merge(list_job, how = 'left', on = 'code_job')
    
    # code_job 결측치 제거하고 code_job, job 출력
    welfare.dropna(subset = ['code_job'])[['code_job', 'job']].head()
```

<br>

### 직업별 월급 차이 분석하기
#### 1. 직업별 월급 평균표 만들기
```
  # 직업별 월급 평균표 만들기
  job_income = welfare.dropna(subset = ['job', 'income']) \    # job, income 결측치 제거
                      .groupby('job', as_index = False) \      # job 별 분리
                      .agg(mean_income = ('income', 'mean'))   # income 평균 구하기
  job_income.head()
```

<br>

#### 2. 그래프 만들기
> (1) 월급이 많은 직업
```
  # 상위 10위 추출
  top10 = job_income.sort_values('mean_income', ascending = False).head(10)
  top10
```
```
  # 맑은 고딕 폰트 설정
  import matplotlib.pyplot as plt
  plt.rcParams.update({'font.family' : 'Malgun Gothic'})
  
  # 막대 그래프 만들기
  sns.barplot(data = top10, y = 'job', x = 'mean_income')
```

<br>

> (2) 월급이 적은 직업
```
  # 하위 10위 추출
  bottom10 = job_income.sort_values('mean_income').head(10)
  bottom10
```
```
  # 막대 그래프 만들기
  sns.barplot(data = bottom10, y = 'job', x = 'mean_income') \
     .set(xlim = [0, 800])
```

<br>

---

<br>

성별 직업 빈도
---
### 성별 직업 빈도 분석하기
#### 1. 성별 직업 빈도표 만들기
```
  # 남성 직업 빈도 상위 10개 추출
  job_male = welfare.dropna(subset = ['job']) \            # job 결측치 제거
                    .query('sex == "male"') \              # male 추출
                    .groupby('job', as_index = False) \    # job 별 분리
                    .agg(n = ('job', 'count')) \           # job 빈도 구하기
                    .sort_values('n', ascending = False) \ # 내림차순 정렬
                    .head(10)                              # 상위 10행 추출
  job_male
```
```
  # 여성 직업 빈도 상위 10개 추출
  job_female = welfare.dropna(subset = ['job']) \             # job 결측치 제거
                      .query('sex == "female"') \             # female 추출
                      .groupby('job', as_index = False) \     # job 별 분리
                      .agg(n = ('job', 'count')) \            # job 빈도 구하기
                      .sort_values('n', ascending = False) \  # 내림차순 정렬
                      .head(10)                               # 상위 10행 추출
  job_female
```

<br>

#### 2. 그래프 만들기
```
  # 남성 직업 빈도 막대 그래프 만들기
  sns.barplot(data = job_male, y = 'job', x = 'n').set(xlim = [0, 500])
```
```
# 여성 직업 빈도 막대 그래프 만들기
sns.barplot(data = job_female, y = 'job', x = 'n').set(xlim = [0, 500])
```

<br>

---

<br>

종교 유무에 따른 이혼율
---
### 종교 변수 검토 및 전처리하기
#### 1. 변수 검토하기
```
  welfare['religion'].dtypes  # 변수 타입 출력
```
```
  welfare['religion'].value_counts()  # 빈도 구하기
```

<br>

#### 2. 전처리하기
```
  # 종교 유무 이름 부여
  welfare['religion'] = np.where(welfare['religion'] == 1, 'yes', 'no')
  
  # 빈도 구하기
  welfare['religion'].value_counts()
```
```
  # 막대 그래프 만들기
  sns.countplot(data = welfare, x = 'religion')
```

<br>

### 혼인 상태 변수 검토 및 전처리하기
#### 1. 변수 검토하기
```
  welfare['marriage_type'].dtypes  # 변수 타입 출력
```
```
  welfare['marriage_type'].value_counts()  # 빈도 구하기
```

<br>

#### 2. 파생변수 만들기 - 이혼 여부
```
  # 이혼 여부 변수 만들기
  welfare['marriage'] = np.where(welfare['marriage_type'] == 1, 'marriage',
                        np.where(welfare['marriage_type'] == 3, 'divorce', 'etc'))
```
```
  # 이혼 여부별 빈도
  n_divorce = welfare.groupby('marriage', as_index = False) \  # marriage 별 분리
                     .agg(n = ('marriage', 'count'))           # marriage 별 빈도 구하기
  n_divorce
```
```
  # 막대 그래프 만들기
  sns.barplot(data = n_divorce, x = 'marriage', y = 'n')
```

<br>

### 종교 유무에 따른 이혼율 분석하기
#### 1. 종교 유무에 따른 이혼율표 만들기
```
  rel_div = welfare.query('marriage != "etc"') \            # etc 제외
                   .groupby('religion', as_index = False) \ # religion 별 분리
                   ['marriage'] \                           # marriage 추출
                   .value_counts(normalize = True)          # 비율 구하기
  rel_div
```

<br>

#### 2. 그래프 만들기
```
  rel_div = \
      rel_div.query('marriage == "divorce"') \                    # devorce 추출
             .assign(proportion = rel_div['proportion'] * 100) \  # 백분율로 바꾸기
             .round(1)                                            # 반올림
  
  rel_div
```
```
  # 막대 그래프 만들기
  sns.barplot(data = rel_div, x = 'religion', y = 'proportion')
```

<br>

### 연령대 및 종교 유무에 따른 이혼율 분석하기
#### 1. 연령대별 이혼율표 만들기
```
  age_div = welfare.query('marriage != "etc"') \          # etc 제외
                   .groupby('ageg', as_index = False) \   # ageg 별 분리
                   ['marriage'] \                         # marriage 추출
                   .value_counts(normalize = True)        # 비율 구하기
  age_div
```
```
  # 연령대 및 이혼 여부별 빈도
  welfare.query('marriage != "etc"') \          # etc 제외
         .groupby('ageg', as_index = False) \   # ageg 별 분리
         ['marriage'] \                         # marriage 추출
         .value_counts()                        # 빈도 구하기
```

<br>

#### 2. 연령대별 이혼율 그래프 만들기
```
  age_div = \ 
      age_div.query('ageg != "young" & marriage == "divorce"') \  # 초년층 제외, 이혼 추출
             .assign(proportion = age_div['proportion'] * 100) \  # 백분율로 바꾸기
             .round(1)                                            # 반올림
  
  age_div
```
```
  # 막대 그래프 만들기
  sns.barplot(data = age_div, x = 'ageg', y = 'proportion')
```

<br>

#### 3. 연령대 및 종교 유무에 따른 이혼율표 만들기
```
  age_rel_div = \
      welfare.query('marriage != "etc" & ageg != "young"') \    # etc 제외, 초년층 제외
             .groupby(['ageg', 'religion'], as_index = False) \ # ageg, religion 별 분리
             ['marriage'] \                                     # marriage 추출
             .value_counts(normalize = True)                    # 비율 구하기
  
  age_rel_div
```

<br>

#### 4. 연령대 및 종교 유무에 따른 이혼율 그래프 만들기
```
  age_rel_div = \
      age_rel_div.query('marriage == "divorce"') \                       # divorce 추출
                 .assign(proportion = age_rel_div['proportion'] * 100) \ # 백분율로 바꾸기
                 .round(1)                                               # 반올림
  
  age_rel_div
```
```
  # 막대 그래프 만들기
  sns.barplot(data = age_rel_div, x = 'ageg', y = 'proportion', hue = 'religion')
```

<br>

---

<br>

지역별 연령대 비율
---
### 지역 변수 검토 및 전처리하기
#### 1. 변수 검토하기
```
  welfare['code_region'].dtypes  # 변수 타입 출력
```
```
  welfare['code_region'].value_counts()  # 빈도 구하기
```

<br>

#### 2. 전처리하기
```
  # 지역 코드 목록 만들기
  list_region = pd.DataFrame({'code_region' : [1, 2, 3, 4, 5, 6, 7],
                              'region'      : ['서울',
                                               '수도권(인천/경기)',
                                               '부산/경남/울산',
                                               '대구/경북',
                                               '대전/충남',
                                               '강원/충북',
                                               '광주/전남/전북/제주도']})
  list_region
```
```
  # 지역명 변수 추가
  welfare = welfare.merge(list_region, how = 'left', on = 'code_region')
  welfare[['code_region', 'region']].head()
```

<br>

### 지역별 연령대 비율 분석하기
#### 1. 지역별 연령대 비율표 만들기
```
  region_ageg = welfare.groupby('region', as_index = False) \  # region 별 분리
                       ['ageg'] \                              # ageg 추출
                       .value_counts(normalize = True)         # 비율 구하기
  region_ageg
```

<br>

#### 2. 그래프 만들기
```
  region_ageg = \
      region_ageg.assign(proportion = region_ageg['proportion'] * 100) \  # 백분율로 바꾸기
                 .round(1)                                                # 반올림
```
```
  # 막대 그래프 만들기
  sns.barplot(data = region_ageg, y = 'region', x = 'proportion', hue = 'ageg')
```

<br>

#### 3. 누적 비율 막대 그래프 만들기
> (1) 피벗하기
```
  # 피벗
  pivot_df = \
      region_ageg[['region', 'ageg', 'proportion']].pivot(index   = 'region',
                                                          columns = 'ageg',
                                                          values  = 'proportion')
  pivot_df
```

<br>

> (2) 그래프 만들기
```
  # 가로 막대 그래프 만들기
  pivot_df.plot.barh(stacked = True)
```

<br>

> (3) 막대 정렬하기
```
  # 노년층 비율 기준 정렬, 변수 순서 바꾸기
  reorder_df = pivot_df.sort_values('old')[['young', 'middle', 'old']]
  reorder_df
```
```
  # 누적 가로 막대 그래프 만들기
  reorder_df.plot.barh(stacked = True)
```

<br>
