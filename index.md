## 2022-2 빅데이터분석 (단원고)

### 5강 - 3월 31일

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
