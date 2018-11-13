# suricata-rules
这是自己平时抓包写的suricata规则，会慢慢更新

### webshell_caidao_php
    匹配菜刀最常见的特征base64_decode
    POST /upload/1.php HTTP/1.1\r\nUser-Agent: Java/1.8.0_151\r\nHost: xxxxxxx\r\nAccept: text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2\r
    \nConnection: keep-alive\r\nContent-type: application/x-www-form-urlencoded\r\nContent-Length: 725\r\n\r\naa=@eval.   (base64_decode($_POST[action]));&action=QGluaV9zZXQoImR   pc3BsYXlfZXJyb3JzIiwiMCIpO0BzZXRfdGltZV9saW1pdCgwKTtAc2V0X21hZ2ljX3F1b3Rlc19ydW50aW1lKDApO2VjaG8oIi0%2BfCIpOzskRD1iYXNlNjRfZGVjb2RlKCRfUE9TVFsiejEiXSk7JEY9QG9wZW5kaXIoJEQ     pO2lmKCRGPT1OVUxMKXtlY2hvKCJFUlJPUjovLyBQYXRoIE5vdCBGb3VuZCBPciBObyBQZXJtaXNzaW9uISIpO31lbHNleyRNPU5VTEw7JEw9TlVMTDt3aGlsZSgkTj1AcmVhZGRpcigkRikpeyRQPSRELiIvIi4kTjskVD1AZGF0ZSgiWS1tLWQgSDppOnMiLEBmaWxlbXRpbWUoJFApKTtAJEU9c3Vic3RyKGJhc2VfY29udmVydChAZmlsZXBlcm1zKCRQKSwxMCw4KSwtNCk7JFI9Ilx0Ii4kVC4iXHQiLkBmaWxlc2l6ZSgkUCkuIlx0Ii4kRS4iCiI7aWYoQGlzX2RpcigkUCkpJE0uPSROLiIvIi4kUjtlbHNlICRMLj0kTi4kUjt9ZWNobyAkTS4kTDtAY2xvc2VkaXIoJEYpO307ZWNobygifDwtIik7ZGllKCk7&z1=RDpcd2FtcDY0XHd3d1x1cGxvYWQ%3D
    
### Hacker backdoor or shell  Microsoft Corporation
    TCP流量中匹配windows cmd启动后回显信息“Microsoft Corporation”，常见场景nc反弹，添加depth修饰符只在前200字节匹配避免误报。
    Microsoft Windows [版本 6.1.7601]
    版权所有 (c) 2009 Microsoft Corporation。保留所有权利。
    
### Hacker backdoor or shell Microsoft Windows
    同上 匹配“Microsoft Windows [”关键字 
    
### Windows Powershell Request UserAgent
    匹配powershell web cmdlet user-agent特征

### Linux wget/curl download .sh script
    一些恶意软件使用curl或者wget下载sh脚本执行，常见于挖矿木马。

### 可疑的caidao响应-列目录
    通过正则表达式匹配http响应里列目录操作,排除掉html标签避免误报
    ->|1.jsp.2018-10-14 12:21:12.2129.R W
    |<-

### CobaltStrike download.windowsupdate.com C2 Profile
    正常windows更新域名，uri是十六进制形式，日期变动，格式固定。cobaltstrike c2通过更改host伪造windows更新产生的流量，将传输的
    数据base64后放到url中，例如：
    GET /c/msdownload/update/others/2016/12/29136388_oY_bIWScNUZ3X5ebOUqkFA.cab HTTP/1.1
    Accept: */*
    Host: download.windowsupdate.com
    User-Agent: Windows-Update-Agent/10.0.10011.16384 Client-Protocol/1.40
    windows更新正常格式“/c/msdownload/update/others/2012/02/8位数字_40位十六进制.cab”

### CobaltStrike login server
    cobaltstrike客户端与团队服务端通讯走ssl，默认证书配置在teamserver文件中
    keytool -keystore ./cobaltstrike.store -storepass 123456 -keypass 123456 -genkey -keyalg RSA -alias cobaltstrike -dname "CN=google.com OU=xsser, O=xsser, L=Somewhere, S=Cyberspace, C=Earth"

### 可疑的dns隧道请求
    开始的01 00 匹配dns.flags字段（0100表示dns请求8180代表dns响应），后面用正则匹配的十六进制代表dns请求类型
    \x00\x10\x00\x01是TXT记录，\x00\x0f\x00\x01是MX记录，\x00\x05\x00\x01是CNAME记录，dsize代表udp数据包长度，
    dns隧道将数据封装到二级域名方式传输长度要大于普通域名，但也容易触发误报。

### http GET data
    HTTP协议中GET请求也可以像POST那样在请求体中传输数据，大多数IDS以及协议还原设备对于GET请求这块是只记录URL，
    如果传输数据在流量还原中只会当成在正常的访问请求，SSR中http混淆就是使用这种方法。
    47 45 54 匹配HTTP GET关键字，0d0a0d0a是HTTP协议分隔符，wireshark追踪流后发现0d0a0d0a就是请求体结尾，后面跟着响应数据，
    此条规则匹配0d0a0d0a后面还有数据的情况，并且排除掉了一些正常业务（排除掉{和<字符串）。
