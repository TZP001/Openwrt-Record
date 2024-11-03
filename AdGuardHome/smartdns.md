## AdGuardHome和SmartDNS配合实现分流和记录dns请求
### 安装（开始之前先把教程看完）
#### AdGuardHome（host模式）
##### 安装
```
docker run -dit \
  --name ADG_Eye \
  -v <你的本地目录>/ADG_Eye/work:/opt/adguardhome/work \
  -v <你的本地目录>/ADG_Eye/conf:/opt/adguardhome/conf \
  -e TZ="Asia/Shanghai" \
  --dns 223.5.5.5 \
  --network host \
  --restart always \
  -d adguard/adguardhome:latest
```
##### DNS设置
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
##### 其它设置
* 不要勾选【浏览安全】和【家长控制】，否则AdGuard无法正常解析DNS
* 此处没有开启ipv6，有需要的自己开启，有些设置可能需要再修改
* 黑名单设置：添加了一个[easylist](https://anti-ad.net/easylist.txt)，国外组默认
* 在passwall或ssr+中添加不代理域名（有挺大作用）
```
sm2.doh.pub
dns.alidns.com
doh.360.cn
doh.pub
dns.pub
```
-----------
#### smartDNS设置
##### 实际上是将ADG国内组和国外组配置填入smartdns
* 一定要填入一组ip，否则无法解析，和ADG的```Bootstrap DNS```一样的道理
```

config smartdns
	option enabled '1'
	option server_name 'smartdns'
	option port '5336'
	option tcp_server '1'
	option serve_expired '1'
	option resolve_local_hostnames '1'
	option force_https_soa '0'
	option rr_ttl_min '600'
	option seconddns_enabled '1'
	option seconddns_tcp_server '1'
	option seconddns_server_group 'firewall'
	option seconddns_no_speed_check '0'
	option seconddns_no_rule_addr '0'
	option seconddns_no_rule_nameserver '0'
	option seconddns_no_rule_ipset '0'
	option seconddns_no_rule_soa '0'
	option seconddns_no_dualstack_selection '0'
	option seconddns_no_cache '0'
	option coredump '0'
	option auto_set_dnsmasq '0'
	option force_aaaa_soa '1'
	option seconddns_port '5335'
	option seconddns_force_aaaa_soa '1'
	option prefetch_domain '1'
	option cache_size '9999'
	option ipv6_server '0'
	option dualstack_ip_selection '0'
	option old_port '5336'
	option old_enabled '1'
	option old_auto_set_dnsmasq '0'

config server
	option enabled '1'
	option name '阿里'
	option ip 'https://sm2.doh.pub/dns-query'
	option type 'udp'
	option blacklist_ip '0'

config server
	option enabled '1'
	option name '阿里'
	option ip 'https://dns.alidns.com/dns-query'
	option type 'udp'
	option blacklist_ip '0'

config server
	option enabled '1'
	option name '360'
	option ip 'https://doh.360.cn/dns-query'
	option type 'udp'
	option blacklist_ip '0'

config server
	option enabled '1'
	option name '腾讯'
	option ip 'https://doh.pub/dns-query'
	option type 'udp'
	option blacklist_ip '0'

config server
	option enabled '1'
	option name '腾讯'
	option ip 'https://dns.pub/dns-query'
	option type 'udp'
	option blacklist_ip '0'

config server
	option enabled '1'
	option name '国外'
	option ip 'https://94.140.14.141/dns-query'
	option type 'udp'
	option server_group 'firewall'
	option blacklist_ip '0'

config server
	option enabled '1'
	option name '国外'
	option ip 'https://0ms.dev/dns-query'
	option type 'udp'
	option server_group 'firewall'
	option blacklist_ip '0'

config server
	option enabled '1'
	option name '国外'
	option ip 'https://dns.quad9.net/dns-query'
	option type 'udp'
	option server_group 'firewall'
	option blacklist_ip '0'
```
-----------
### 路由器设置
#### lan口设置
* 将DNS服务器设置为```223.5.5.5``` 和 ```8.8.8.8``` ，这里填什么都不会影响，但填127.0.0.1会对smartdns首次启动造成影响
#### 防火墙设置，重定向dns端口 
* 在防火墙自定义规则添加如下规则，将dns请求重定向到国内组端口
```shell
[ -n "$(command -v ip6tables)" ] && ip6tables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5330
[ -n "$(command -v ip6tables)" ] && ip6tables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5330
iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-ports 5330
iptables -t nat -A PREROUTING -p tcp --dport 53 -j REDIRECT --to-ports 5330
```
#### ```DHCP/DNS```设置
* 基本设置：DNS转发设置为```127.0.0.1#5336``` ，取消仅本地服务的√
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


