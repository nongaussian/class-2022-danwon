## 2022-2 빅데이터분석 (단원고)

### 3강 - 3월 17일

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
