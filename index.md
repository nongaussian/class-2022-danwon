## 2022-1 빅데이터분석 (단원고)

### 9강 - 5월 12일

* MSFT 데이터 읽어들이기

```python
import pandas as pd
import os, sys
from google.colab import drive
drive.mount('/content/drive')

df = pd.read_csv('drive/MyDrive/Technology Companies/MSFT.csv')
df = df.drop(columns=["Adj Close"])

dt_all=pd.date_range(start=df.iloc[0]['Date'],end=df.iloc[-1]['Date'])
dt_breaks=[d for d in dt_all if d.strftime("%Y-%m-%d") not in df['Date'].to_list()]
```

### 8강 - 4월 15일

* 슬라이드: [W08.pdf](https://github.com/nongaussian/class-2022-danwon/files/8493598/W08.pdf)

### 7강 - 4월 14일

* 슬라이드: [W07.pdf](https://github.com/nongaussian/class-2022-danwon/files/8493587/W07.pdf)

* Stockprice 데이터 다운로드: [Top Tech Companies Stock Price.zip](https://github.com/nongaussian/class-2022-danwon/files/8460921/Top.Tech.Companies.Stock.Price.zip)

* AAPL 데이터 읽어들이기

```python
import pandas as pd
import os, sys
from google.colab import drive
drive.mount('/content/drive')

df = pd.read_csv('drive/MyDrive/Technology Companies/AAPL.csv')
```

* 휴장일 제거 코드

```python
# df의 첫날 부터 끝날 까지 모든 날짜 리스트를 생성
dt_all = pd.date_range(start=df.iloc[0]['Date'],end=df.iloc[-1]['Date'])

# df에 없는 날짜 (즉, 장이 쉬는 날) 의 리스트 만들기
dt_breaks = [d for d in dt_all if d.strftime("%Y-%m-%d") not in df['Date'].to_list()]

# 그래프 x 축에서 쉬는 날 빼기
fig.update_xaxes(rangebreaks=[dict(values=dt_breaks)])

fig.show()
```

### 6강 - 4월 7일

* 슬라이드: [W06.pdf](https://github.com/nongaussian/class-2022-danwon/files/8460917/W06.pdf)

* COVID-19 데이터 읽어들이기

```python
import pandas as pd
import os, sys
from google.colab import drive
drive.mount('/content/drive')

df = pd.read_csv('drive/MyDrive/covid_19_data.csv')

df = df.rename(columns={
    'ObservationDate': 'Date',
    'Province/State': 'City',
    'Country/Region': 'Country'
}).drop(columns=['Last Update'])

df['Active'] = df['Confirmed'] - df['Deaths'] - df['Recovered']
```

* GDP 데이터: [GDP.csv](https://github.com/nongaussian/class-2022-danwon/files/8427086/GDP.csv)


### 5강 - 3월 31일

* 슬라이드: [W05.pdf](https://github.com/nongaussian/class-2022-danwon/files/8439971/W05.pdf)

* Kaggle 데이터 다운로드 링크: [https://www.kaggle.com/datasets/sudalairajkumar/novel-corona-virus-2019-dataset](https://www.kaggle.com/datasets/sudalairajkumar/novel-corona-virus-2019-dataset?select=covid_19_data.csv)

### 4강 - 3월 25일

* 슬라이드: [W04.pdf](https://github.com/nongaussian/class-2022-danwon/files/8359802/W04.pdf)

* 데이터 로딩 코드

```python
import os
import sys
import urllib.request
import pandas as pd
import json

client_id = "나의 CLIENT ID"
client_secret = "나의 CLIENT SECRET"
url = "https://openapi.naver.com/v1/datalab/search"

gnd = ["m", "f"]
ag = ["3", "4", "5", "6", "7", "8", "9", "10", "11"]
body = '''{
  "startDate":"2022-02-15",
  "endDate":"2022-03-11",
  "timeUnit":"date",
  "keywordGroups":[
    {"groupName":"짜장면","keywords":["자장면", "짜장면"]},
    {"groupName":"짬뽕","keywords":["짬뽕"]}
  ],
  "ages":["%s"],
  "gender":"%s"
}'''

def request_naver(url, client_id, client_secret, body):
  request = urllib.request.Request(url)
  request.add_header("X-Naver-Client-Id",client_id)
  request.add_header("X-Naver-Client-Secret",client_secret)
  request.add_header("Content-Type","application/json")
  response = urllib.request.urlopen(request, data=body.encode("utf-8"))
  rescode = response.getcode()
  assert rescode==200
  return response.read().decode('utf-8')


df = pd.DataFrame(columns = ["title", "period", "gender", "age", "ratio"])

for age in ag:
  for gender in gnd:
    jsonstr = request_naver(url, client_id, client_secret, body % (age, gender))
    jsonobj = json.loads(jsonstr)

    for res in jsonobj["results"]:
      for row in res["data"]:
        df = df.append({
            "title": res["title"],
            "period": row["period"], 
            "gender": gender,
            "age": age,
            "ratio": row["ratio"]
        }, ignore_index = True)
```

* 퀴즈풀이연습: https://forms.gle/UnikwvhTYnw7DdKq9

### 3강 - 3월 17일

* 슬라이드: [W03.pdf](https://github.com/nongaussian/class-2022-danwon/files/8304209/W03.pdf)

```python
def request_naver(url, client_id, client_secret, body):
  request = urllib.request.Request(url)
  request.add_header("X-Naver-Client-Id",client_id)
  request.add_header("X-Naver-Client-Secret",client_secret)
  request.add_header("Content-Type","application/json")
  response = urllib.request.urlopen(request, data=body.encode("utf-8"))
  rescode = response.getcode()
  assert rescode==200
  return response.read().decode('utf-8')
```

### 2강 - 3월 10일

- 슬라이드: [W02-pagerank.pdf](https://github.com/nongaussian/class-2022-danwon/files/8228465/W02-pagerank.pdf)

- 웹링크 데이터 생성코드

 ```python
 def create_setdata(ss):
    record = set()
    for i in ss:
        if i not in record:
            record.add(i)
        else:
            yield record
            record = set()
            record.add(i)
    if not record:
        yield record
 
 def create_dataset(N):
    while True:
        s = np.random.default_rng().zipf(1.2, N * 100)
        dataset = list(create_setdata(s[s<N]))
        if len(dataset) >= N:
            break
    A = np.zeros((N, N))
    for i, s in enumerate(dataset[:N]):
        for j in s:
            A[i, j] = 1
    return A
 ```

### 1강 - 3월 3일

- 슬라이드: [W01-intro.pdf](https://github.com/nongaussian/class-2022-danwon/files/8160209/W01-intro.pdf)

<style>
  .footer {
    display: none;
  }
</style>
