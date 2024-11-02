## <span style="color: red;">三</span>AdGuardHome实现分流加dns监控
## AdGuardHome常用配置
### 安装（开始之前先把教程看完）
#### 国内组（host模式）
```
docker run -dit \
  --name ADG_Home \
  -v <你的本地目录>/ADG_Home/work:/opt/adguardhome/work \
  -v <你的本地目录>/ADG_Home/conf:/opt/adguardhome/conf \
  -e TZ="Asia/Shanghai" \
  --dns 223.5.5.5 \
  --network host \
  --restart always \
  -d adguard/adguardhome:latest
```
#### 国外组（bridge模式）
```
docker run -dit \
  --name ADG_Firewall \
  -v <你的本地目录>/ADG_Firewall/work:/opt/adguardhome/work \
  -v <你的本地目录>/ADG_Firewall/conf:/opt/adguardhome/conf \
  -e TZ="Asia/Shanghai" \
  --dns 223.5.5.5 \
  -p 3001:3000/tcp \
  -p 5335:5335/tcp \
  -p 5335:5335/udp \
  --restart always \
  -d adguard/adguardhome:latest
```
----------
#### 监控组（bridge模式）
```
docker run -dit \
  --name ADG_Eye \
  -v <你的本地目录>/ADG_Eye/work:/opt/adguardhome/work \
  -v <你的本地目录>/ADG_Eye/conf:/opt/adguardhome/conf \
  -e TZ="Asia/Shanghai" \
  --dns 223.5.5.5 \
  -p 3002:3000/tcp \
  -p 5333:5333/tcp \
  -p 5333:5333/udp \
  --restart always \
  -d adguard/adguardhome:latest
```
----------
### 使用方法
#### DNS设置
##### 国内组
进去```<路由器ip>:3000```，把web监听端口设置为3000，DNS监听端口设置为5330（不要和其它端口冲突）<br>
###### 上游 DNS 服务器设置
```
127.0.0.1
```
###### 后备 DNS 服务器
```
223.6.6.6
```
###### Bootstrap DNS 服务器
```
114.114.114.114
```
##### 监控组
进去```<路由器ip>:3002```，把web监听端口设置为3000（设置成功点击页面会跳转到3000端口，需要手动改回3002端口），
DNS监听端口设置为5333（你映射的端口）,如果设置为53端口，会造成Adguardhome不工作，因为内部53端口已经被容器本身占用
###### 上游 DNS 服务器设置
```
https://sm2.doh.pub/dns-query
https://dns.alidns.com/dns-query
https://doh.360.cn/dns-query
https://doh.pub/dns-query
https://dns.pub/dns-query
```
###### 后备 DNS 服务器
```
223.6.6.6
```
###### Bootstrap DNS 服务器
```
114.114.114.114
```
##### 国外组
进去```<路由器ip>:3001```，把web监听端口设置为3000（设置成功点击页面会跳转到3000端口，需要手动改回3001端口），
DNS监听端口设置为5335（你映射的端口）,如果设置为53端口，会造成Adguardhome不工作，因为内部53端口已经被容器本身占用
###### 上游 DNS 服务器设置
```
https://94.140.14.141/dns-query
https://0ms.dev/dns-query
https://dns.quad9.net/dns-query
```
###### 后备 DNS 服务器
```
8.8.8.8
```
###### Bootstrap DNS 服务器
```
9.9.9.10
149.112.112.10
2620:fe::10
2620:fe::fe:10
```
#### 其它设置
* 不要勾选【浏览安全】和【家长控制】，否则AdGuard无法正常解析DNS
* 此处没有开启ipv6，有需要的自己开启，有些设置可能需要再修改
* 黑名单设置：国内组添加了一个[easylist](https://anti-ad.net/easylist.txt)，国外组默认
* 在passwall或ssr+中添加不代理域名（有挺大作用）
```
sm2.doh.pub
dns.alidns.com
doh.360.cn
doh.pub
dns.pub
```
-----------
### 路由器设置
#### lan口设置
* 将DNS服务器设置为```127.0.0.1```
#### 防火墙设置，重定向dns端口 
* 在防火墙自定义规则添加如下规则，将dns请求重定向到国内组端口
```shell
[ -n "$(command -v ip6tables)" ] && ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5330
[ -n "$(command -v ip6tables)" ] && ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5330
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5330
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5330
```
#### ```DHCP/DNS```设置
* 基本设置：DNS转发设置为```127.0.0.1#5333``` ，取消仅本地服务的√
* HOSTS和解析文件：忽略解析文件打√
* 高级设置：没有使用Ipv6就把禁止解析Ipv6 DNS记录打√，DNS查询缓存设置为0
#### 分流设置
##### passwall
```
依次点击passwall -> 基本设置 -> DNS ：
过滤模式：通过UDP请求DNS
远程DNS：127.0.0.1#5335
```
##### ssr+
* 跟passwall差不多
-----------
### 踩过的坑
#### AdGuardHome设置错误，导致进不去管理页面
* 把挂载的conf目录删除然后重启容器，然后重新设置
#### AdGuardHome黑名单或白名单更新失败
* 检查是否开启了出国，检查它们的dns是否可行
#### AdGuardHome解析域名失败
* 检查是否开启了【浏览安全】和【家长控制】
#### 只能上国外网，不能上国内网（全局才能上）
* 使用绕过中国大陆模式
#### AdGuardHome解析域名失败
* 检查是否开启了【浏览安全】和【家长控制】
