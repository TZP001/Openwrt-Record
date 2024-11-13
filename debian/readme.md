## debian使用心得[【项目地址】](https://github.com/dockur/windows)
#### 安装
 ```
docker run -it \
      --name=debian \
      --rm \
      -p 8099:3389 \
      -v <你的本地目录>/debian:/debian
      -e TZ="Asia/Shanghai" \
      --dns 223.5.5.5 \
      debian:rc-buggy-20241111
```
------------------
### 图形界面lxde和xrdp安装
```
  apt-get update
	apt-get install lxde-core
	startlxde
	apt install xrdp 
	service xrdp start
	adduser xrdp ssl-cert #添加一个账户“xrdp”
	passwd xrdp #设置账户xrdp的密码
```
### 注意事项
* 挂载目录不要选择系统原有的目录，会导致该目录无法正确生成该有的文件
  ```
  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
  ```
* 端口映射是为了安装xrdp远程控制映射的
* 版本说明：
  
