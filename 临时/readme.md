### 临时文件
Jellyfin‌：适合家庭影音爱好者，可以在家中搭建一个便捷的流媒体视频播放平台。

‌Emby‌：适合需要灵活管理和访问媒体资源的用户。

‌Calibre-Web‌：适合电子书爱好者，可以方便地管理和分享电子书资源。

‌LANraragi‌：适合漫画爱好者，可以方便地管理和阅读漫画。

‌Audiobookshelf‌：适合有声读物和播客爱好者，可以方便地管理和分享有声读物。

‌qBittorrent‌：适合需要高效下载大量数据的用户。

‌Transmission‌：适合需要轻量级下载工具的用户。

heimdall：DIY导航页，支持自定义背景与图标。
#### homeassistant
```
docker run -d -p 8123:8123 --restart=always \
  --name=homeAssistant.2023.6 \
  -v /root/config:/config \
  -e TZ=Asia/Shanghai \
  --privileged=true \
  --net=host \
  homeassistant/raspberrypi3-homeassistant:stable
```
