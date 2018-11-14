# suricata-rules
	此项目记录安全运营人员提取的Suricata IDS规则，重点是一些恶意行为/渗透工具/渗透流量/远控木马等,欢迎大家提交。 

#### 规则编写要求如下
每个规则对应新建目录如下

	webshell检测	#规则目录名称-按照对应检测规则描述清楚即可
	- webshell.pcap	#规则对应的pcap包，尽量以flow的形式保存
	- websehll.rules	#自己提取的规则文件，尽量测试过提交。
	- README	#可以描述一些规则相关的东西，便于他人理解，支持Markdown

#### 规则目录
	简单描述要书写的检测规则，如果有对应规则目录，建议存放至已有规则目录中。

#### 规则对应pcap包
	规则对应的pcap通过Wireshark筛选后，利用菜单文件--保存特定分组--选择pcap格式上传。
	便于识别恶意流数据，也是最小的，便于移动和备份

#### 规则.rules
规则文件命名随意，但后缀必须为rules，如：webshell_caidao.rules
文件中可以出现多个规则文件，README备注中写明
规则内容建议如下：
##### 示例
	sid随意，rev为规则版本每次修改递增，metadata添加创建日期与创建人
	alert http any any -> any any (msg:"webshell_caidao_php"; flow:established; content:"POST";
    http_method; content:".php"; http_uri; content:"base64_decode"; http_client_body;  sid:3000001; 
    rev:1; metadata:created_at 2018_11_14, by al0ne;)

# 注
本项目根目录文件说明

	suricata.rules	#所有有效规则的集合
	disable.conf	#分析过程中记录Suricata禁用规则(无效、误报等情况)
