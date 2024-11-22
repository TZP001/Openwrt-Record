## PairDrop使用心得[【项目地址】](https://github.com/schlagmichdoch/PairDrop)
#### 安装
 ```
docker run -dit \
    --name=pairdrop \
    --restart=unless-stopped \
    -p 3300:3000 \
    -e PUID=1000 \
    -e PGID=1000 \
    -e WS_SERVER=false \
    -e WS_FALLBACK=false \
    -e RTC_CONFIG=false \
    -e RATE_LIMIT=false \
    -e DEBUG_MODE=false \
    -e TZ="Asia/Shanghai" \
    --dns 223.5.5.5 \
    linuxserver/pairdrop:latest
```
#### 说明
* 可用于传输文件及文本，通过网页即可传输，非常方便
* 苹果文件大小限制在200M内

