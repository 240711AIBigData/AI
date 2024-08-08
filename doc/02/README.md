# 데이터 분석에 필요한 연장 챙기기
|-|
|-|
|![image](https://github.com/user-attachments/assets/0de790a7-9b18-4b9e-9bd7-f3f1cb9eb473)|

<br>

변하는 수, ‘변수’
---
### 변수(variable)
- 다양한 값을 지닌 하나의 속성

- 데이터 분석의 대상

<br>

|-|
|-|
|![image](https://github.com/user-attachments/assets/8bce976c-47c1-49f6-ae73-7aedd669ff29)|

<br>

### 변수 만들기
|-|
|-|
|![image](https://github.com/user-attachments/assets/c9be7373-f99f-475d-bcd2-55adc51eef99)|
|- 'a'라는 공간을 만들어 데이터 저장|

<br>

```
  a = 1  # a에 1 할당
  a      # a 출력
```
```
  b = 2
  b
```
```
  c = 3
  c
```
```
  d = 3.5
  d
```

<br>

### 변수로 연산하기
```
  a + b
```
```
  a + b + c
```
```
  4 / b
```
  ```
  5 * b
```

<br> 여러 값으로 구성된 변수 만들기
```
  var1 = [1, 2, 3]
  var1
```
```
  var2 = [4, 5, 6]
  var2
```
```
  var1 + var2
```

<br>

### 문자로 된 변수 만들기
```
  str1 = 'a'
  str1
```
```
  str2 = 'text'
  str2
```
```
  str3 = 'Hello World!'
  str3
```

<br>

### 여러 문자로 된 변수 만들기
```
  str4 = ['a', 'b', 'c']
  str4
```
```
  str5 = ['Hello!', 'World', 'is', 'good!']
  str5
```

<br>

### 문자 변수 결합하기
```
  str2 + str3
```
```
  str2 + ' ' + str3
```

<br>

### 문자로 된 변수로는 연산 불가
```
  str1 + 2
```

<br>

함수(function)
---
- 값을 넣으면 특정한 기능을 수행해 처음과 다른 값으로 만듦

|-|
|-|
|![image](https://github.com/user-attachments/assets/d824b4a6-6315-47f1-8cbe-488f641d2b9e)|

<br>

### 함수 이용하기
> 변수 만들기
```
  x = [1, 2, 3]
  x
```

> 함수 적용하기
```
  sum(x)
```
```
  max(x)
```
```
  min(x)
```

<br>

### 함수의 결과물로 새 변수 만들기
```
  x_sum = sum(x)
  x_sum
```
```
  x_max = max(x)
  x_max
```

<br>

함수 vs 메서드
---
|-|
|-|
|![image](https://github.com/user-attachments/assets/7efa0479-7fa6-4fa9-aac7-6a562401ff34)|
|- 내장함수 : 파이썬이 미리 만들어놓은 함수(바로 사용 가능) <br> - 패키지함수 : pd[판다스 라이브러리].[안의]read_csv()[해당함수 불러오기] <br> - 메서드 : df[우리가 만든 객체/변수].[안의]head()[해당함수 불러오기] <br> - 사용자(정의)함수 : def로 정의한 사용자가 만든 함수 |

<br>

### 1. 내장 함수
```
  sum(var)
  max(var
```

<br>

### 2. 패키지 함수
```
  import pandas as pd
  pd.read_csv('exam.csv')
  pdDataFrame({'x' : [1, 2, 3]})
```

<br>

### 3. 메서드
> 메서드(method) : 변수가 지니고 있는 함수
```
  df.head()
  df.info()
```

<br>

#### 변수의 자료 구조에 따라 사용 가능한 메서드가 다름
> 데이터 프레임
```
  df = pd.read_csv('exam.csv')
  df.head()
```

<br>

> 리스트
```
  var = [1, 2, 3]
  var.head()
```

<br>
