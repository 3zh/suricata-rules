### Linux wget/curl download .sh script
    一些恶意软件使用curl或者wget下载sh脚本执行，常见于挖矿木马。

### Windows Powershell Request UserAgent
    匹配powershell web cmdlet user-agent特征

### Hacker backdoor or shell  Microsoft Corporation
    TCP流量中匹配windows cmd启动后回显信息“Microsoft Corporation”，常见场景nc反弹，添加depth修饰符只在前200字节匹配避免误报。
    Microsoft Windows [版本 6.1.7601]
    版权所有 (c) 2009 Microsoft Corporation。保留所有权利。
    
### Hacker backdoor or shell Microsoft Windows
    同上 匹配“Microsoft Windows [”关键字 

### 可疑的netstat命令流量
    HTTP中匹配cmd执行netstat响应结果，常见于命令执行。
