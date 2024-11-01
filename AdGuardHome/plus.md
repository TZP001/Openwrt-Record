同时兼顾dns监控
```
docker run -dit \
  --name ADG \
  -v <你的本地目录>/ADG/work:/opt/adguardhome/work \
  -v <你的本地目录>/ADG/conf:/opt/adguardhome/conf \
  -e TZ="Asia/Shanghai" \
  --dns 223.5.5.5 \
  -p 3001:3000/tcp \
  -p 5353:5353/tcp \
  -p 5353:5353/udp \
  --restart always \
  -d adguard/adguardhome:latest
```
