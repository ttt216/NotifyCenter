# 📬 NotifyCenter - 多渠道通知中心

**一个集中管理、开箱即用的通知转发服务**，让你通过一个统一的 API，把消息推送到 Bark、Telegram、Mattermost、企业微信、PushDeer 等多个平台。支持 JSON、表单、multipart、纯文本等多种请求体格式，原生兼容 Emby、群晖 DSM、Grafana、Uptime Kuma 等主流 Webhook 来源。适合个人自动化、Homelab、NAS 通知、媒体服务器、监控告警等场景。

<p align="center">
  <img src="https://img.shields.io/badge/version-0.62-blue.svg" alt="version">
  <img src="https://img.shields.io/badge/go-1.21+-00ADD8.svg" alt="go">
  <img src="https://img.shields.io/badge/license-MIT-green.svg" alt="license">
  <img src="https://img.shields.io/badge/docker-ready-2496ED.svg" alt="docker">
</p>

---

## ✨ v0.62 更新亮点

- 📮 **全面支持多种请求体格式**：根据 Content-Type 自动识别并解析 `application/json`、`application/x-www-form-urlencoded`、`multipart/form-data`、`text/plain`，覆盖 Emby、Grafana 等各种 Webhook 来源
- 🖼️ **正式入口支持 multipart/form-data**：带截图/附件的通知也能正常解析（如 Emby 媒体通知）
- 📄 无 Content-Type 或未知类型时智能兜底：先尝试 JSON，再尝试表单，最后作为纯文本
- 🔔 标题缺失时统一兜底为 `Notification`，不再出现 `{{ title }}` 原样字符串

## ✨ v0.61 更新亮点

- 🧩 **通用模板字段自动归一化**：`text` / `message` / `msg` / `body` / `description` 会自动映射到 `content`，`subject` / `headline` 映射到 `title`，不同来源的 payload 都能命中同一套模板
- 🔍 **未匹配变量保留原样**：模板里 `{{ foo }}` 找不到数据时会保留成字符串本身，方便一眼看出哪些字段没传（Emby 模板同样适用）
- 📝 **渲染兜底更友好**：渲染失败时优先取 payload 的 content/text/message；都没有则输出标准 JSON 字符串
- ✅ **修复新建 API Key 失败**：升级到 v0.60 时因老数据库遗留字段导致新建 API Key 报错的问题
- 🔔 **删除 / 清空操作有真实弹窗**：使用页面内自定义确认框，各类嵌入式浏览器也能正常拦截取消

## ✨ v0.60 更新亮点

- 🔀 **一个 API Key 支持绑定多个路由**：每个路由独立使用自己的模板发送到自己的渠道集合，让 Markdown 渠道、纯文本渠道、企业微信 markdown 等各得其所
- 📋 推送日志按路由分组展示：每个路由的渲染标题/内容、以及各渠道的成功失败一目了然
- 🛡️ 删除保护与提示：
  - 删除模板前检查引用，被路由使用时拒绝删除并列出占用路由
  - 删除渠道前提示会影响哪些路由
  - 删除路由前提示归属的 API Key
  - 删除 API Key 前提示会释放的路由列表
- 🔄 抢占式路由转移：APIKey 保存时若勾选了已被别的 APIKey 占用的路由，会弹窗让你确认"转移"
- 🧭 路由列表新增"归属 APIKey"列，一眼看清路由被谁占用
- 🗃️ 平滑数据迁移：老的 `apikeys.route_id` 会自动反填到 `routes.api_key_id`，无需手动处理

## ✨ v0.58 更新亮点

- 🧹 清理冗余代码，精简项目结构
- 🐛 修复通用模板的变量显示错误

## ✨ v0.57 更新亮点

- 🕒 日志保留天数控制（推送日志 / 后端日志各自独立，支持 7天/30天/永久）
- 🗑️ 一键清空推送日志 / 后端日志
- 🧹 每日凌晨自动清理超期日志
- 🐛 修复 Bark 渠道图标不生效
- 🐛 修复保留天数切换取消后仍触发清理
- 🎨 搜索框宽度优化，工具栏布局更紧凑

## ✨ v0.56 更新亮点

- 🎨 新增网站图标（favicon）：铃铛 + 信号波设计，紫蓝色渐变背景
- 📱 支持浏览器标签栏、书签、移动端"添加到主屏幕"显示项目 Logo

## ✨ v0.55 更新亮点

- 🐛 修复弹窗内文本框选中文字后松开鼠标误关闭弹窗的问题

## ✨ v0.54 更新亮点

- 🌐 **企业微信 API 代理**：支持通过代理服务器调用企业微信 API，解决动态 IP 用户无法配置"可信 IP"的问题
- 📩 **企业微信接收消息回调**：内置回调地址，支持签名校验、AES 解密及消息转发
- 🎯 **渠道配置简化**：Bark、Mattermost、企业微信群机器人 支持直接粘贴完整 URL
- 🛡️ **API 保底解析**：接收非 JSON 请求体时自动作为纯文本消息处理
- 🐛 **多项 Bug 修复**：空列表显示、删除渠道后自动刷新、回调 URL 显示等

---

## 🎯 核心特性

| 特性 | 说明 |
|------|------|
| 🚀 **一键部署** | 单个 Docker 镜像，SQLite 内嵌，无需外部数据库 |
| 📡 **7 种渠道** | Bark、Telegram、Mattermost、企业微信 x2、PushDeer、自定义 Webhook |
| 🎬 **Emby 支持** | 播放/入库事件智能识别，自动生成海报剧照 URL |
| 📮 **多格式请求体** | 自动识别 JSON / 表单（`x-www-form-urlencoded`）/ multipart / 纯文本 |
| 📝 **模板系统** | Pongo2/Jinja2 语法，支持变量、条件、循环；未匹配变量保留原名，调试一目了然 |
| 🔀 **路由规则** | 一次 API 调用同时推送到多个渠道，一个 API Key 可绑定多条路由，每路由独立模板与渠道集合 |
| 🔑 **API Key 认证** | 路由级 API Key 安全隔离，支持一键禁用、有效期、调用统计 |
| 📊 **可视化管理** | 内置 Web 后台，实时查看通知日志、系统日志 |
| 🌐 **时区友好** | 默认 `Asia/Shanghai`，可通过环境变量调整 |
| 🎨 **详细日志** | 每条通知记录原始数据、模板渲染结果、各渠道发送状态 |

---

## 📦 支持的通知渠道

| 渠道 | 类型标识 | 简要说明 |
|------|---------|---------|
| **Bark** | `bark` | iOS 免费推送 App，支持推送级别、角标、加密、自动复制、点击跳转、图标等 |
| **Telegram** | `telegram` | 官方 Bot API，支持个人 & 群组，HTML/Markdown 富文本 |
| **Mattermost** | `mattermost` | 开源团队协作平台，通过 Incoming Webhook 推送，支持 Markdown |
| **企业微信（应用消息）** | `wecom` | 通过企业自建应用推送到 App，支持指定接收人/部门/标签，需配置可信 IP |
| **企业微信（群机器人）** | `wecom_webhook` | 通过群机器人 Webhook 推送到企业微信群，无需可信 IP，支持 text/markdown |
| **PushDeer** | `pushdeer` | 开源推送服务，支持官方 & 自建服务器，支持 markdown/text/image |
| **自定义 Webhook** | `webhook` | 对接任意支持 Webhook 的第三方服务（如 Slack、飞书、钉钉） |

---

## 🚀 快速开始

### 使用 Docker 一键运行

```bash
docker run -d \
  --name notifycenter \
  -p 5400:5400 \
  -v $(pwd)/data:/app/data \
  -e TZ=Asia/Shanghai \
  --restart unless-stopped \
  ttt216/notifycenter:0.62
```

### 使用 Docker Compose

```yaml
version: '3.8'

services:
  notifycenter:
    image: ttt216/notifycenter:0.62
    container_name: notifycenter
    ports:
      - "5400:5400"
    volumes:
      - ./data:/app/data
    environment:
      - TZ=Asia/Shanghai
    restart: unless-stopped
```

保存为 `docker-compose.yml`，执行 `docker compose up -d` 即可。

### 访问后台

打开浏览器访问：**http://localhost:5400/admin/login**

- 默认账号：`admin`
- 默认密码：`123456`

> ⚠️ **首次登录会强制要求修改密码**，请设置一个安全的新密码（字母 + 数字组合，不少于 6 位）。

---

## 📖 使用指南

### 1️⃣ 添加通知渠道

进入 **渠道（Channels）** 菜单，点击"添加渠道"，根据不同类型填写配置。

#### 📱 Bark（iOS 推送）

1. 在手机上安装 [Bark](https://apps.apple.com/app/bark-customed-notifications/id1403753865)
2. 打开 App 首页，**复制服务器上方显示的完整推送 URL**（形如 `https://api.day.app/xxxxxxxxxxxx`）
3. 在 NotifyCenter 中添加 Bark 渠道，把这个 URL 粘贴到 **API URL** 字段即可，系统会自动解析出服务器地址和设备 Key
4. 可选：
   - **推送级别**：`active`（响铃）/ `timeSensitive`（勿扰模式也响）/ `passive`（不亮屏）
   - **高级设置**：如果使用自建 Bark 服务器，可展开"🔧 高级设置"分开填写服务器地址和设备 Key
   - **角标数字**、**分组**、**加密**、**自动复制**、**归档** 等
5. 消息中的 `imgURL` 会作为图标，`linkURL` 作为点击跳转链接

> 💡 说明：Bark App 显示的完整 URL 里包含一个专用 Key（不同于普通的 Device Token），只有使用这个完整 URL 才能成功推送。这也是 NotifyCenter 简化为"直接粘贴 URL"的原因。

#### 💬 Telegram

1. **创建 Bot**：与 [@BotFather](https://t.me/BotFather) 对话 → 发送 `/newbot` → 按提示操作 → 获取 Bot Token
2. **获取 Chat ID**：
   - **个人接收**：与 [@userinfobot](https://t.me/userinfobot) 对话 → 得到你的 Chat ID（正整数）
   - **群组接收**：把 Bot 拉进群 → 发一条消息 → 访问 `https://api.telegram.org/bot<TOKEN>/getUpdates` 查看 `chat.id`（负整数）
3. 在 NotifyCenter 中添加 Telegram 渠道，填写 Bot Token 和 Chat ID
4. 可选：选择 **Parse Mode** 为 `HTML` 或 `MarkdownV2` 以支持富文本

#### 🗣️ Mattermost

1. 在 Mattermost → **集成 → 传入 Webhook** → 创建 Webhook
2. 复制 Webhook URL（形如 `https://mm.example.com/hooks/xxxxx`）
3. 在 NotifyCenter 中添加 Mattermost 渠道，粘贴完整 URL
4. 可选：**用户名**（Bot 显示名）、**图标 URL**（Bot 头像）、**频道**（覆盖默认发送频道）
5. **💡 图片显示技巧**：在消息内容里用 Markdown 语法 `![]({{BackdropImgUrl}})`，Mattermost 会自动展开预览

#### 🏢 企业微信（应用消息）

1. 在 [企业微信管理后台](https://work.weixin.qq.com) 创建自建应用
2. 记录三个关键值：
   - **CorpID**（"我的企业"页面）
   - **AgentID** 和 **Secret**（应用详情页）
3. 在 NotifyCenter 中添加"企业微信（应用消息）"渠道
4. ⚠️ **重要**：需要在应用详情页的"企业可信IP"中配置服务器出口 IP，否则会返回 `60020: not allow to access from your ip` 错误
5. 可通过 `to_user` / `to_party` / `to_tag` 指定接收人/部门/标签，留空默认 `@all`

#### 🤖 企业微信（群机器人）

1. 在企业微信群中添加"群机器人"，获取 Webhook 地址
2. 在 NotifyCenter 中添加"企业微信（群机器人）"渠道
3. **Webhook Key** 支持两种格式：
   - 完整 URL：`https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=xxxxxxxx`
   - 只填 key 值：`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`
4. 选择消息类型：`text`（纯文本）或 `markdown`（富文本）
5. ✅ 相比应用消息，群机器人**无需配置可信 IP**，配置更简单
6. ⚠️ 注意：只能发到企业微信群，不能发送到个人微信

#### 📮 PushDeer

1. 在 PushDeer App 中生成 **PushKey**（App 内 → "Key 管理" → "新建 Key"）
2. 在 NotifyCenter 中添加 PushDeer 渠道
3. **服务器地址**：默认 `https://api2.pushdeer.com`，也支持自建 PushDeer 服务器
4. **消息类型**：
   - `markdown`（默认）：支持 Markdown 富文本
   - `text`：纯文本
   - `image`：以图片 URL 作为消息内容

#### 🌐 自定义 Webhook

用于对接项目未内置的第三方服务（如 Slack、飞书、钉钉）。

配置项：
- **URL**：目标接口地址
- **Method**：`POST`（默认）/ `GET` / `PUT`
- **Headers**：自定义请求头（JSON）
- **Body Template**：请求体模板，支持变量替换

支持的模板变量：`{{title}}`、`{{content}}`、`{{img_url}}`、`{{link_url}}`

**示例：对接 Slack Webhook**

```json
{
  "text": "*{{title}}*\n{{content}}"
}
```

---

### 2️⃣ 配置模板

进入 **模板（Templates）** 菜单，点击"添加模板"。

**两种模板类型：**

- **general（通用模板）**：适用于普通 API 通知，配置标题和内容
- **emby（Emby 专用）**：自动识别 Emby Webhook 事件（播放开始/暂停/入库等），支持电影/剧集分别配置

**模板语法**：使用 Pongo2/Jinja2 语法，支持变量替换、条件判断、循环等。

示例：
```
🎬 {{ UserName }} 正在观看 {{ ItemName }}
📺 设备：{{ DeviceName }}
⏱️ 进度：{{ ProgressPercent }}%
```

---

### 3️⃣ 创建路由（Route）

进入 **路由（Routes）** 菜单，点击"添加路由"。

- 选择关联的**模板**
- 选择要推送到的**渠道**（可多选，一次通知同时发到多个平台）
- 保存后即可使用

---

### 4️⃣ 生成 API Key

进入 **API Key** 菜单，点击"添加 API Key"。

- 每个 API Key 绑定一个路由
- 保存后会生成唯一的 API Key，用于调用 API

---

### 5️⃣ 发送通知

**通用 API 调用**：

```bash
curl -X POST "http://your-server:5400/api/service/notify?api_key=your-api-key" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "通知标题",
    "content": "通知内容",
    "img_url": "https://example.com/image.png",
    "link_url": "https://example.com"
  }'
```

**也支持 Bark 风格的 GET 请求**：

```
GET /api/service/notify/{title}/{content}?api_key=xxx&push_img_url=...&push_link_url=...
```

#### 支持的请求体格式

NotifyCenter 会根据 `Content-Type` 自动识别并解析：

| Content-Type | 适用来源 | 处理方式 |
|-------------|---------|---------|
| `application/json` | Emby、Grafana、Uptime Kuma、自建系统等 | 直接 JSON 解析 |
| `application/x-www-form-urlencoded` | 旧版系统、部分表单型 Webhook | 表单解析，自动解开 `payload=...` 嵌套 JSON 与 URL 编码 |
| `multipart/form-data` | Emby 带截图通知、带附件的告警 | 提取 `data` 字段中的 JSON |
| `text/plain` 或空 | 简单脚本（`curl -d "xxx"`） | 先试 JSON，失败则作为纯文本放入 `content` |
| 未知类型 | 任意 | 依次尝试 JSON、表单、纯文本兜底 |

通用模板会自动做字段归一化：`text` / `message` / `msg` / `body` / `description` → `content`，`subject` / `headline` → `title`，所以不同来源的 payload 都能直接命中同一套模板。

#### 群晖 DSM Webhook 对接

在群晖 **控制面板 → 通知设置** 中：

- 选择「**Webhook**」类型（注意：不要选 "Synology Chat"，后者会发送私有的 `payload=...` 表单格式）
- **HTTP 方法**：POST
- **Content-Type**：`application/json`
- **消息模板**：填写标准 JSON，例如：
  ```json
  {"text":"你要发送的消息内容"}
  ```

消息到达后，通用模板用 `{{ content }}` 即可直接取到正文（`text` 会自动归一化为 `content`）。

**Emby Webhook 直接对接**：

在 Emby 的 Webhook 配置中填入：
```
http://your-server:5400/api/service/notify?api_key=your-api-key
```

NotifyCenter 会自动识别 Emby 的 payload（包括带截图的 multipart 请求），无需额外处理。

---

## 🎬 Emby 专用模板详解

NotifyCenter 完整支持 Emby 媒体服务器的 Webhook 通知，专为 Emby 优化。

### 自动事件识别

- `playback.start` / `playback.stop` / `playback.pause` / `playback.unpause` → 自动映射为 `playback.movie` 或 `playback.episode`
- `library.new` → 自动映射为 `library.new_movie` 或 `library.new_episode`

### 可用变量

| 变量 | 说明 |
|------|------|
| `Event` | 事件类型 |
| `UserName` | 用户名 |
| `ItemName` | 媒体名称 |
| `ItemType` | 媒体类型（Movie / Episode） |
| `ItemYear` | 发行年份 |
| `ItemOverview` | 简介（自动截断至 200 字符） |
| `SeriesName` | 剧集名称（仅剧集） |
| `Season` / `Episode` | 季数 / 集数 |
| `DeviceName` / `Client` / `ClientIp` | 播放设备信息 |
| `ServerName` / `ServerVersion` | 服务器信息 |
| `DateLocal` | 本地时间（自动转换时区） |
| `CoverImgUrl` / `PrimaryImgUrl` | 海报封面（竖版；剧集为整剧海报） |
| `BackdropImgUrl` | 剧照/背景图（横版） |
| `LogoImgUrl` | 影片 Logo 图 |
| `EpisodeImgUrl` | **单集画面**（仅剧集播放时有值） |
| `ProgressMinutes` / `TotalMinutes` / `ProgressPercent` | 播放进度 |

### 图片变量使用说明

> **需要配置 Emby 服务器地址**：在模板管理 → 编辑 Emby 模板 → 填写 **Emby 服务器地址**（如 `https://emby.example.com:8086`），系统会自动生成图片 URL。不填写则四个图片变量均为空字符串。

### 示例：剧集播放模板

```
🎬 {{ UserName }} 正在观看《{{ SeriesName }}》
📺 第 {{ Season }} 季第 {{ Episode }} 集 - {{ ItemName }}
⏱️ 进度：{{ ProgressPercent }}%

![]({{ EpisodeImgUrl }})
```

---

## ⚙️ 环境变量

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `TZ` | `Asia/Shanghai` | 时区配置 |
| `DATA_PATH` | 容器内 `/app/data` | 数据存储路径 |
| `ADMIN_SESSION_KEY` | 内置默认值 | 管理后台会话安全密钥（建议自行设置） |
| `APP_VERSION` | 从 VERSION 文件读取 | 应用版本号（一般无需设置） |

---

## 💾 数据持久化

- 数据存储在 SQLite 数据库中，位于容器内 `/app/data/notifycenter.db`
- 通过挂载 `./data:/app/data` 可以持久化数据
- **升级版本时数据完全兼容**，只需替换镜像重启即可

### 从旧版本升级

1. 停止旧容器
2. 备份 `data/notifycenter.db` 文件（推荐）
3. 使用 v0.52 镜像创建新容器，挂载相同的 `data` 目录
4. 数据库结构完全兼容，无需迁移

---

## 🎯 典型应用场景

| 场景 | 说明 |
|------|------|
| 📺 **Emby / Jellyfin 播放通知** | 家人开始观看/新入库剧集时推送到手机 |
| 🗄️ **群晖 DSM 系统通知** | 群晖系统事件、备份任务、存储空间告警统一转发 |
| 🖥️ **监控告警** | 服务器 CPU/内存告警推送到多个渠道 |
| 🤖 **自动化脚本通知** | Python/Shell 脚本执行结果推送 |
| 📦 **下载完成提醒** | qBittorrent/Aria2 下载完成通知 |
| 🔒 **登录/安全告警** | 检测到异常登录立即推送 |
| 📊 **日报/周报推送** | 定时任务生成的统计报告 |

---

## ❓ 常见问题

### 如何获取服务器公网 IP？

```bash
curl ifconfig.me
```

用于企业微信应用消息的"企业可信IP"配置。

### 忘记管理员密码怎么办？

删除数据库文件重新初始化：

```bash
docker stop notifycenter
rm data/notifycenter.db
docker start notifycenter
```

> ⚠️ 该操作会丢失所有配置数据，请谨慎操作。

### 如何测试渠道是否配置正确？

在 **渠道管理** 页面，每个渠道右侧都有 **测试** 按钮，点击后可以填写测试标题和内容进行发送，会显示详细的响应信息。

### 支持 arm64 / Apple Silicon 吗？

当前发布版本为 amd64 架构。如需 arm64，可以拉取源码本地构建。

---

## 🛠️ 技术栈

- **后端**：Go 1.21+ / Gin / GORM / SQLite
- **模板引擎**：Pongo2（Jinja2 兼容）
- **前端**：原生 HTML + Tailwind CSS + Vanilla JS
- **容器**：Alpine Linux 精简镜像

---

## 📄 更新日志

### v0.62（当前版本）
- 📮 按 Content-Type 智能解析请求体：JSON / form-urlencoded / multipart / 纯文本
- 🖼️ 通知入口支持 multipart/form-data（Emby 带截图场景）
- 📄 未知 Content-Type 时依次尝试 JSON、表单、纯文本兜底
- 🔔 标题缺失或未匹配时统一兜底为 `Notification`，对所有模板类型生效

### v0.61
- 🧩 通用模板字段自动归一化（text/message/msg/body/description → content；subject/headline → title）
- 🔍 未匹配变量保留原样字符串（Emby 模板同样适用）
- 📝 渲染兜底优化：优先输出真实文本，无法获取时输出标准 JSON
- ✅ 修复升级到 v0.60 后新建 API Key 失败的问题
- 🔔 删除 / 清空操作改用页面内自定义确认弹窗

### v0.60
- 🔀 一个 API Key 支持绑定多个路由：每路由独立模板 + 独立渠道集合
- 📋 推送日志按路由分组展示，每路由的渲染结果与渠道结果一目了然
- 🛡️ 删除模板/渠道/路由/APIKey 前引用检查与提示
- 🔄 抢占式路由转移：勾选归属他人的路由需二次确认
- 🧭 路由列表新增「归属 APIKey」列
- 🗃️ 自动迁移老的 `apikeys.route_id` 到 `routes.api_key_id`

### v0.58
- 🧹 清理冗余代码，精简项目结构
- 🐛 修复通用模板的变量显示错误

### v0.57
- 🕒 日志保留天数控制（推送日志 / 后端日志各自独立，支持 7天/30天/永久）
- 🗑️ 一键清空推送日志 / 后端日志
- 🧹 每日凌晨自动清理超期日志
- 🐛 修复 Bark 渠道图标不生效
- 🐛 修复保留天数切换取消后仍触发清理
- 🎨 搜索框宽度优化，工具栏布局更紧凑

### v0.56
- 🎨 新增网站图标（favicon）：铃铛 + 信号波设计，浏览器标签栏可显示 Logo

### v0.55
- 🐛 修复弹窗内文本框选中文字后松开鼠标误关闭弹窗的问题

### v0.54
- 🌐 企业微信 API 代理支持
- 📩 企业微信接收消息回调（签名校验、AES 解密、消息转发）
- 🎯 Bark / Mattermost / 企业微信群机器人 支持直接粘贴完整 URL
- 🛡️ API 支持接收非 JSON 请求体
- 🐛 修复多项列表和 UI 相关的 Bug

### v0.53
- 🐛 修复删除渠道后列表未自动刷新的问题
- 🎯 企业微信应用消息渠道编辑页新增回调 URL 一键复制按钮

### v0.52
- 🎨 **界面术语统一**：所有 UI 文案中的"通道"改为"路由"，避免与"渠道（Channel）"混淆
- 🔐 **密码修改体验优化**：修改成功后显示提示页 + 倒计时，登录后自动跳回原页面
- 📖 **使用文档大幅完善**：Bark、Telegram、Mattermost、Webhook 配置步骤全面补齐，附典型示例
- 🚀 **性能优化**：版本号读取从每次请求改为启动加载，减少文件 IO
- 🧹 **代码质量提升**：清理冗余代码和临时文件，项目结构更清晰

### v0.51
- 🎬 **Emby 图片链接自动生成**：新增 `CoverImgUrl` / `PrimaryImgUrl` / `BackdropImgUrl` / `LogoImgUrl` / `EpisodeImgUrl` 五个模板变量
- 📸 **单集画面独立变量**：新增 `EpisodeImgUrl`，剧集播放通知可显示单集自身画面
- 🎯 **智能回退**：剧集单集自动使用父级（Series）的 Backdrop / Logo 图片
- ⚙️ **模板配置扩展**：Emby 模板可配置服务器地址，图片 URL 自动拼接

### v0.5
- 🆕 新增**企业微信（应用消息）**渠道
- 🆕 新增**企业微信（群机器人）**渠道
- 🆕 新增 **PushDeer** 渠道
- 🔧 修复渠道配置存储的多个问题
- 📊 Demo 初始化数据完善

### v0.4
- 🚀 项目从 Python (FastAPI) 全面重写为 Go (Gin)
- ⚡ 启动速度、性能、部署体积显著优化
- 🎨 保持所有功能、页面、API 完全兼容

---

## 📄 许可证

MIT License

---

## 🙋 反馈与建议

如果你在使用中遇到问题、有新功能建议或想贡献代码，欢迎提交 Issue 或 Pull Request。

**祝使用愉快！** 🎉
