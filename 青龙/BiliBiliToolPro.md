## BiliBiliToolPro常用配置
### [详细安装](https://github.com/RayWangQvQ/BiliBiliToolPro/blob/main/qinglong/README.md)
#### 1. 添加
青龙面板，`配置文件`页。

修改 `RepoFileExtensions="js py"` 为 `RepoFileExtensions="js py sh"`

保存配置。
#### 在青龙面板中添加拉库定时任务
两种方式，任选其一即可：
##### 方式一：订阅管理
```
名称：Bilibili
类型：公开仓库
链接：https://github.com/RayWangQvQ/BiliBiliToolPro.git
定时类型：crontab
定时规则：2 2 28 * *
白名单：bili_task_.+\.sh
文件后缀：sh
```
没提到的不要动。

保存后，点击运行按钮，运行拉库。
##### 方式二：定时任务拉库
青龙面板，`定时任务`页，右上角`添加任务`，填入以下信息：
```
名称：拉取Bili库
命令：ql repo https://github.com/RayWangQvQ/BiliBiliToolPro.git "bili_task_"
定时规则：2 2 28 * *
```
点击确定。

保存成功后，找到该定时任务，点击运行按钮，运行拉库。

-------------------
### [详细配置](https://github.com/RayWangQvQ/BiliBiliToolPro/blob/2b47c813d6def872307ad4369a5d37acf32601ec/docs/configuration.md#3641-webhookurl)
#### 1.4. 方式四：托管在青龙面板上，使用面板的环境变量页或配置文件页进行配置

青龙面板配置，其本质还是通过环境变量进行配置，有如下两种方式。

- 环境变量页[推荐]

例如：

名称：`Ray_BiliBiliCookies__1`

值：`abcde`

- 配置文件页

例如，配置Cookie和推送：

```
export Ray_BiliBiliCookies__1="_uuid=abc..."
export Ray_Serilog__WriteTo__9__Args__token="abcde"
```
配置文件页添加、修改配置，需要重启青龙容器使之生效，环境变量页则可以立即生效，所以推荐使用环境变量页配置。

--------------
##### 4. 多账号支持

部署成功后，直接去运行扫码登录任务，扫码成功后，应用会自动更新或添加cookie。

青龙平台会添加环境变量里，Key 为 `Ray_BiliBiliCookies__0`、`Ray_BiliBiliCookies__1`、`Ray_BiliBiliCookies__2`...

其他平台默认会添加到名为cookies.json的账号配置文件中：
```
{
  "BiliBiliCookies": [
    "cookie1",
    "cookie2",
    "...",
  ],
}
```
------------
#### 3.6.1. 是否开启每个账号单独推送消息

|   TITLE   | CONTENT   |
| ---------- | -------------- |
| 配置Key | `Notification:IsSingleAccountSingleNotify` |
| 意义 | 开启后，每个账号会单独推送消息。否则多账号合并只推送一条消息 |
| 值域   | [true,false] |
| 默认值   | true |
| 环境变量   | `Ray_Notification__IsSingleAccountSingleNotify` |
| GitHub Secrets  | |

-----------------------
#### 3.6.4. 钉钉机器人

在群内添加机器人，获取到机器人的WebHook地址，添加到配置中。

机器人的安全策略，当前不支持加签，请使用关键字策略，推荐关键字：`Ray` 或 `BiliBili`

##### 3.6.4.1. webHookUrl

|   TITLE   | CONTENT   |
| ---------- | -------------- |
| 配置Key | `Serilog:WriteTo:5:Args:webHookUrl` |
| 值域   | 一串字符串 |
| 默认值   | 空 |
| 环境变量   | `Ray_Serilog__WriteTo__5__Args__webHookUrl` |
| GitHub Secrets  | `PUSHDINGURL`|

