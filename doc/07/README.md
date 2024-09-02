# 메서드 체이닝(method chainig)
- .을 이용해 메서드를 게속 이어서 작성하는 방법

> ex
```Python
  mpg['grade'].value_counts().sort_index()
```

- 변수에 여러 메서드를 순서대로 적용
- 출력 결과를 변수에 할당하고 다시 불러오는 작업 반복하지 않아도 됨

<br>

> 출력 결과를 변수에 할당하는 방법
```Python
  df = mpg['grade']
  df = df.value_counts()
  df = df.sort_index()
```

<br>

> 메서드 체이닝
```Python
  df = mpg['grade'].value_counts().sort_index()
```

<br>
