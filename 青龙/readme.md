## 青龙使用心得[【项目地址】](https://github.com/whyour/qinglong)

### 命令安装
```
docker run -dit \
  -v <你的本地目录>/ql/data:/ql/data \
  -p 5700:5700 \
  -e QlBaseUrl="/" \
  -e QlPort="5700" \
  -e TZ="Asia/Shanghai" \
  --dns 223.5.5.5 \
  --name qinglong \
  --hostname qinglong \
  --restart unless-stopped \
  whyour/qinglong:latest
```
--------
### 配合使用
#### 1. 京东脚本
```
https://github.com/shufflewzc/faker2
```
#### 2. B站签到（[使用方法](https://github.com/TZP001/docker-/blob/main/%E9%9D%92%E9%BE%99/BiliBiliToolPro.md)）

```
https://github.com/RayWangQvQ/BiliBiliToolPro
```
------------
### 使用注意
1. 不要使用代理，在ssr访问控制中将桥接ip设置为白名单
2. 需要添加什么后缀的文件需要修改配置文件中的```RepoFileExtensions="js mjs py pyc sh"```
