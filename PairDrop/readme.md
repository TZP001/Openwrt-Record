## PairDrop使用心得
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
    lscr.io/linuxserver/pairdrop
```
