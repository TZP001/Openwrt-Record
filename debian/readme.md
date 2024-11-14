## debian使用心得[【项目地址】](https://github.com/dockur/windows)[【镜像仓库】](https://hub.docker.com/_/debian/tags)
#### 安装
 ```
docker run -it \
      --name=debian \
      --rm \
      -p 8099:3389 \
      -v <你的本地目录>/debian:/debian \
      -e TZ="Asia/Shanghai" \
	-e USER="VNCC"
      --dns 223.5.5.5 \
      debian:rc-buggy-20241111
```
------------------
#### 使用
* 安装必要组件（ps、netstat、ping）
```
apt-get update
apt install procps net-tools inetutils-ping
```
* 图形界面安装（lxde 或 xfce4）
	 * lxde
	```
	apt-get install lxde-core
	startlxde
	```
	 * xfce4（推荐）
	```
	apt install xfce4 xfce4-goodies xorg dbus-x11 x11-xserver-utils
	```
* 远程访问
	* xrdp
	```
	apt install xrdp 
	service xrdp start
	adduser xrdp ssl-cert #添加一个账户“xrdp”
	passwd xrdp #设置账户xrdp的密码
	```
	* vnc(推荐)
	```
	apt install tightvncserver novnc 
	tightvncserver  #启动tightvnc，需要设置用户USER和密码
	websockify -D --web=/usr/share/novnc/ 5908 localhost:5901 #5901是vnc启动后的端口，5908是novnc监听端口
 	```
 	* vnc访问方式
 	```
	http://<路由器IP地址>:5908/vnc.html?password=123456&autoconnect=true&reconnect=true&resize=remote
  	```
---------------
### 注意事项
* 挂载目录不要选择系统原有的目录，会导致该目录无法正确生成该有的文件
  ```
  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
  ```
* 端口映射是为了安装xrdp远程控制映射的
* 版本说明：[1](https://www.debian.org/doc/manuals/debian-faq/ftparchives.zh-cn.html) [2](https://wiki.debian.org/zh_CN/Backports) [3](https://blog.csdn.net/problc/article/details/141429782)

| **代号** | **版本**  	| | **代号** 	| **版本**	| | **镜像代号** 	| **说明**	|
|:---:	   |:---:	|-|:---:	|:---:		|-|:---:		|:---:		|
|buzz      |Debian 1.1	| |lenny	|Debian 5.0	| |	slim		|	只包含必要的软件包	|
|rex	   |Debian 1.2	| |squeeze	|Debian 6	| |	backports	|一种软件源[?](https://wiki.debian.org/zh_CN/Backports)	|
|bo	   |Debian 1.3	| |wheezy	|Debian 7	| |			|		|
|hamm	   |Debian 2.0	| |jessie	|Debian 8	| |			|		|
|slink	   |Debian 2.1	| |stretch	|Debian 9	| |			|		|
|potato	   |Debian 2.2	| |buster	|Debian 10	| |			|		|
|woody	   |Debian 3.0	| |bullseye	|Debian 11	| |			|		|
|sarge	   |Debian 3.1	| |bookworm	|Debian 12	| |			|		|
|etch	   |Debian 4.0	| |trixie	|Debian 13	| |			|		|
|sid 	   |unstable 	| |		|		| |			|		|
  
