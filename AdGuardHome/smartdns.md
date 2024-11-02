AdGuardHome和SmartDNS配合实现分流和记录dns请求
```
config smartdns
	option enabled '1'
	option server_name 'smartdns'
	option port '5336'
	option tcp_server '1'
	option ipv6_server '1'
	option dualstack_ip_selection '1'
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
	option name '阿里'
	option ip '223.6.6.6'
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

config server
	option enabled '1'
	option name '谷歌'
	option ip '8.8.8.8'
	option type 'udp'
	option server_group 'firewall'
	option blacklist_ip '0'
```
