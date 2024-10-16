## 记录openwrt使用过程中一些常用插件遇到的问题
### passwall或ssr+问题
#### 1. passwall或ssr+ 节点可用，但是不能连接Google
* 使用了luci-app-nginx，将nginx插件卸载即可
#### 2. 只能上国外网，不能上国内网（全局才能上）
* 使用绕过中国大陆模式
#### 3. docker容器不能上外网
* ssr+接口控制处添加docker接口，没有则按下方配置在```网络→接口```重新添加docker0接口（重启生效）
```
config interface 'docker'
        option ifname 'docker0'
        option proto 'none'
        option auto '0'

config device 'docker0'
        option type 'bridge'
        option name 'docker0'
        list ifname 'docker0'
```
------------
