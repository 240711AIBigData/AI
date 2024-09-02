# 지도 시각화

|-|
|-|
|![image](https://github.com/user-attachments/assets/fda5bf7d-7aac-4ccb-a372-185dc112c81a)|

<br>

시군구별 인구 단계 구분도 만들기
---
### 단계 구분도(choropleth map)
- 지역별 통계치를 색깔 차이로 표현한 지도

- 인구, 소득 같은 통계치가 지역별로 어떻게 다른지 쉽게 이해할 수 있음

<br>

### 시군구별 인구 단계 구분도 만들기
#### 1. 시군구 경계 지도 데이터 준비하기
- SIG.geojson

  - 행정 구역 코드, 지역 이름, 시군구 경계 위도와 경도 좌표를 담고 있음

- 딕셔너리 자료 구조로 되어 있음

- SIG_CD : 지역을 나타내는 행정 구역 코드

- geometry : 시군구의 경계를 나타낸 위도, 경도 좌표

```Python
  import json
  geo = json.load(open('SIG.geojson', encoding = 'UTF-8'))
```
```Python
  # 행정 구역 코드 출력 
  geo['features'][0]['properties']
```
```Python
# 위도, 경도 좌표 출력
geo['features'][0]['geometry']
```

<br>

#### 2. 시군구별 인구 데이터 준비하기
- Population_SIG.csv

  - 지도에 표현할 시군구별 인구 통계 데이터
 
- 2021년의 시군구별 행정 구역 코드, 지역 이름, 인구를 담고 있음

```Python
  import pandas as pd
  df_pop = pd.read_csv('Population_SIG.csv')
  df_pop.head()
```
```Python
  df_pop.info()
```

<br>

> code 를 문자 타입으로 바꾸기

- 행정 구역 코드를 나타낸 df_pop 의 code 는 int64 타입으로 되어있음

- 문자 타입으로 되어 있어야 지도를 만드는데 활용 가능

```Python
  df_pop['code'] = df_pop['code'].astype(str)
```

<br>

#### 3. 단계 구분도 만들기
> 패키지 설치

- folium 패키지를 이용하면 단계 구분도 생성 가능

- 아나콘다 프롬프트에서 folium 패키지 설치하기

```Python
  pip install folium
```

<br>

##### (1) 배경 지도 만들기
- foliu,.Map() 을 이용해 배경 지도 만들기

  - location : 지도의 중심 위도, 경도 좌표 입력
 
  - zoom_start : 지도를 확대할 정도 입력

```Python
  import folium
  folium.Map(location = [35.95, 127.7],  # 지도 중심 좌표
             zoom_start = 8)             # 확대 단계
```

<br>

> 배경 지도 만들어 저장하기
```Python
  map_sig = folium.Map(location = [35.95, 127.7],  # 지도 중심 좌표
                       zoom_start = 8,             # 확대 단계
                       tiles = 'cartodbpositron')  # 지도 종류
  map_sig
```

<br>

##### (2) 단계 구분도 만들기
- folium.Choropleth() 를 이용해 단계 구분도 만들기

  - geo_data : 지도 데이터
 
  - data : 색깔로 표현할 통계 데이터
 
  - columns : 통계 데이터의 행정 구역 코드 변수, 색깔로 표현할 변수
 
  - key_on : 지도 데이터의 행정 구역 코드

```Python
  # 지도 데이터
  # 통계 데이터
  # df_pop 행정 구역 코드, 인구
  # geo 행정 구역 코드
  folium.Choropleth(geo_data = geo,
                    data = df_pop,
                    columns = ('code', 'pop'),
                    key_on = 'feature.properties.SIG_CD') \
        .add_to(map_sig)
  
  map_sig
```

<br>

##### (3) 계급 구간 정하기
- 앞에서 출력한 지도는 지역이 색깔별로 표현되어 있지 않음

- 지역을 단계별로 나눌 때 기준으로 삼은 '계급 구간'이 적당하지 않기 때문

<br>

> 분위수를 이용해 지역을 적당히 나누는 계급 구간 정하기

- quantile() 을 이용해 5가지 계급 구간의 하한값, 상한값 구하기

- folium.Choropleth() 의 bins 에 입력

  - 지역을 인구에 따라 다섯 단계 색깔로 표현

```Python
  bins = list(df_pop['pop'].quantile([0, 0.2, 0.4, 0.6, 0.8, 1]))
  bins
```

<br>

##### (4) 디자인 수정하기
```Python
  ## 배경 지도 만들기
  
  # 지도 중심 좌표
  # 확대 단계
  # 지도 종류
  map_sig = folium.Map(location = [35.95, 127.7],
                       zoom_start = 8,
                       tiles = 'cartodbpositron')
```
```Python
  ## 단계 구분도 만들기
  # 지도 데이터
  # 통계 데이터
  # df_pop 행정 구역 코드, 인구
  # geo 행정 구역 코드
  # 컬러맵
  # 투명도
  # 경계선 투명도
  # 계급 구간 기준값
  # 배경 지도에 추가
  folium.Choropleth(geo_data = geo,
                    data = df_pop,
                    columns = ('code', 'pop'),
                    key_on = 'feature.properties.SIG_CD',
                    fill_color = 'YlGnBu',
                    fill_opacity = 1,
                    line_opacity = 0.5,
                    bins = bins) \
        .add_to(map_sig)
  map_sig
```

<br>

---

<br>

서울시 동별 외국인 인구 단계 구분도 만들기
---
### 서울시 동별 외국인 인구 단계 구분도 만들기
#### 1. 서울시 동 경계 지도 데이터 준비하기
- EMD_Seoul.geojson

  - 서울시의 동별 행정 구역 코드, 동 이름, 동 경계 위도, 경도 좌표 담고 있음
 
- ADM_DR_CD : 동을 나타내는 행정 구역 코드

- geometry : 동별 경계를 나타낸 위도, 경도 좌표

```Python
  import json
  geo_seoul = json.load(open('EMD_Seoul.geojson', encoding = 'UTF-8'))
```
```Python
  # 행정 구역 코드 출력
  geo_seoul['features'][0]['properties']
```
```Python
  # 위도, 경도 좌표 출력
  geo_seoul['features'][0]['geometry']
```

<br>

#### 2. 서울시 동별 외국인 인구 데이터 준비하기
- Foreigner_EMD_Seoul.csv

  - 2021년 서울시 동별 행정 구역 코드, 동 이름, 외국인 인구 담고 있음

```Python
  foreigner = pd.read_csv('Foreigner_EMD_Seoul.csv')
  foreigner.head()
```
```Python
  foreigner.info()
```

<br>

> code 를 문자 타입으로 바꾸기
```Python
  foreigner['code'] = foreigner['code'].astype(str)
```

<br>

#### 3. 단계 구분도 만들기
##### 계급 구간 정하기
- 지역을 8단계로 나누도록 8개 계급 구간의 하한값, 상한값 만들기

```Python
  bins = list(foreigner['pop'].quantile([0, 0.2, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1]))
  bins
```
```Python
  ## 배경 지도 만들기
  
  # 서울 좌표
  # 확대 단계
  # 지도 종류
  map_seoul = folium.Map(location = [37.56, 127],
                         zoom_start = 12,
                         tiles = 'cartodbpositron')
```
```Python
  ## 단계구분도 만들기
  # 지도 데이터
  # 통계 데이터
  # foreigner 행정 구역 코드, 인구
  # geo_seoul 행정 구역 코드
  # 컬러맵
  # 결측치 색깔
  # 투명도
  # 경계선 투명도
  # 계급 구간 기준값
  # 배경 지도에 추가
  folium.Choropleth(geo_data = geo_seoul,
                    data = foreigner,
                    columns = ('code', 'pop'),
                    key_on = 'feature.properties.ADM_DR_CD',
                    fill_color = 'Blues',
                    nan_fill_color = 'White',
                    fill_opacity = 1,
                    line_opacity = 0.5,
                    bins = bins) \
        .add_to(map_seoul)
  map_seoul
```

<br>

#### 4. 구 경계선 추가하기
- SIG_Seoul.geojson

  - 서울시의 구 경계 좌표를 담고 있음
 
- 서울시 구 경계선을 이용해 단계 구분도 만들기

- .add_to(map_seoul_gu) 를 이용해 앞에서 만든 지도에 추가
 
```Python
  geo_seoul_sig = json.load(open('SIG_Seoul.geojson', encoding = 'UTF-8'))
```
```Python
  ## 서울 구 라인 추가
  
  # 지도 데이터
  # 투명도
  # 선 두께
  # 지도에 추가
  folium.Choropleth(geo_data = geo_seoul_sig,
                    fill_opacity = 0,
                    line_weight = 4) \
        .add_to(map_seoul)
  
  map_seoul
```

<br>

##### 💡 folium 활용하기
> HTML 파일로 저장하기
```Python
  map_seoul.save('map_seoul.html')
```

<br>

> 웹 브라우저에서 html 파일 열기
```Python
  import webbrowser
  webbrowser.open_new('map_seoul.html')
```

<br>

