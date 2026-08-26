# 📬 NotifyCenter - 多渠道通知中心

**一个集中管理、开箱即用的通知转发服务**，让你通过一个统一的 API，把消息推送到 Bark、Telegram、Mattermost、企业微信、PushDeer 等多个平台。支持 JSON、表单、multipart、纯文本等多种请求体格式，原生兼容 Emby、群晖 DSM、Grafana、Uptime Kuma 等主流 Webhook 来源。适合个人自动化、Homelab、NAS 通知、媒体服务器、监控告警等场景。

<p align="center">
  <img src="https://img.shields.io/badge/version-0.68-blue.svg" alt="version">
  <img src="https://img.shields.io/badge/go-1.21+-00ADD8.svg" alt="go">
  <img src="https://img.shields.io/badge/license-MIT-green.svg" alt="license">
  <img src="https://img.shields.io/badge/docker-ready-2496ED.svg" alt="docker">
</p>

---

## ✨ v0.68 更新亮点

- 🔐 **安全性**：API Key 哈希化、登录爆破防护、请求体大小限制、反代 IP 识别
- 🐳 **部署**：Docker 镜像支持 amd64 + arm64 多架构
- ⚡ **性能**：通知渠道改为并发发送
- 🔧 **易用性**：API Key 一键重置、补全 LICENSE
- 🤖 **CI/CD**：push tag 自动构建发布 + 同步 Docker Hub 描述

## ✨ v0.67 更新亮点


- 🐛 Emby 模板系统事件下 `{{ UserName }}` 等占位符字面量暴露的问题修复

## ✨ v0.66 更新亮点（图标适配）

- 📱 iOS Safari "添加到主屏幕" 适配：新增 4 个 apple-touch-icon PNG（180/152/120/76）
- 🎨 iPhone/iPad 主屏快捷方式可显示紫蓝渐变铃铛图标
- 🖼️ 图标按现有 favicon.svg 风格光栅化生成，保持视觉一致
- 🐛 修复 iOS 添加书签到桌面无图标的兼容性问题

## ✨ v0.65 更新亮点（界面体验优化）

- 📌 顶部导航栏和页面标题滚动时固定（类似 Excel 冻结窗格），长列表操作更方便
- 🎯 导航菜单自动高亮当前所在页面，桌面/移动端一致
- 👤 标题栏始终显示登录用户名，窄屏不再隐藏
- 📱 API Key 列表手机端保留调用 URL 及复制按钮，操作按钮文案优化（"数据"）
- 🐛 修复日志页因 JS 缓存导致加载失败的问题

## ✨ v0.64 更新亮点（安全加固）

- 🔒 修复多个安全问题：删除无鉴权的遗留管理 API 和调试端点
- 🛡️ 后台列表对外部输入统一 HTML 转义，防止存储型 XSS
- 🔑 默认管理员密码改为随机生成（可用 `ADMIN_PASSWORD` 指定），Session 密钥自动生成并持久化
- 🌐 CORS 默认收紧为同源，新增 `ALLOWED_ORIGINS`、`COOKIE_SECURE` 环境变量
- 🧹 Mattermost 渠道配置统一为 webhook_url，精简冗余代码

## ✨ v0.63 更新亮点

- 📱 后台管理页面全面支持移动端浏览：手机/平板访问自动适配，汉堡菜单展开为 3 列网格快捷入口
- 📐 弹窗/表格/工具栏自适应小屏，操作提示升级为右上角 Toast

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
  ttt216/notifycenter:0.67
```

### 使用 Docker Compose

```yaml
version: '3.8'

services:
  notifycenter:
    image: ttt216/notifycenter:0.67
    container_name: notifycenter
    ports:
      - "5400:5400"
    volumes:
      - ./data:/app/data
    environment:
      - TZ=Asia/Shanghai
      # 首次初始化时设置管理员账号密码（仅在用户表为空时生效）
      # - ADMIN_USERNAME=admin
      # - ADMIN_PASSWORD=your_secure_password
      # 会话密钥（建议设置为随机字符串；不设置则自动生成并持久化到 data/.session_key）
      # - ADMIN_SESSION_KEY=your_random_secret
      # HTTPS 部署时建议开启
      # - COOKIE_SECURE=true
    restart: unless-stopped
```

保存为 `docker-compose.yml`，执行 `docker compose up -d` 即可。

### 访问后台

打开浏览器访问：**http://localhost:5400/admin/login**

- 默认账号：`admin`（可通过 `ADMIN_USERNAME` 环境变量修改）
- 默认密码：
  - 若设置了 `ADMIN_PASSWORD` 环境变量，使用该密码
  - 否则首次启动会**自动生成随机密码**并打印在容器日志中（只显示一次），请查看 `docker logs notifycenter` 获取

```bash
docker logs notifycenter 2>&1 | grep "管理员用户已创建"
```

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
| `PORT` | `5400` | 监听端口 |
| `GIN_MODE` | `release` | Gin 运行模式 |
| `ADMIN_USERNAME` | `admin` | 首次初始化时的管理员用户名（仅在用户表为空时生效） |
| `ADMIN_PASSWORD` | 随机生成 | 首次初始化时的管理员密码；未设置则自动生成随机密码并打印到启动日志 |
| `ADMIN_SESSION_KEY` | 自动生成 | 管理后台会话 Cookie 签名密钥；未设置时自动生成并持久化到 `data/.session_key` |
| `ALLOWED_ORIGINS` | 空（仅同源） | 允许跨域访问的来源，多个用逗号分隔（如 `https://a.com,https://b.com`） |
| `COOKIE_SECURE` | `false` | 设为 `true` 时仅在 HTTPS 下发送会话 Cookie（生产环境推荐） |
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

**方式一：通过环境变量重置（推荐）**

设置 `ADMIN_PASSWORD` 环境变量仅在用户表为空时生效。若已有用户，可直接修改数据库：

```bash
docker exec -it notifycenter sh
# 进入容器后使用 sqlite3 修改密码（需要技术基础）
```

**方式二：重新初始化（会丢失所有配置）**

```bash
docker stop notifycenter
rm data/notifycenter.db
docker start notifycenter
```

重启后会生成随机密码，查看 `docker logs notifycenter` 获取。

> ⚠️ 该操作会丢失所有配置数据，请谨慎操作。

### 如何测试渠道是否配置正确？

在 **渠道管理** 页面，每个渠道右侧都有 **测试** 按钮，点击后可以填写测试标题和内容进行发送，会显示详细的响应信息。

### 支持 arm64 / Apple Silicon 吗？

已支持多架构：amd64 + arm64。一次 `docker pull ttt216/notifycenter` 自动匹配本机架构。

---

## 🛠️ 技术栈

- **后端**：Go 1.21+ / Gin / GORM / SQLite
- **模板引擎**：Pongo2（Jinja2 兼容）
- **前端**：原生 HTML + Tailwind CSS + Vanilla JS
- **容器**：Alpine Linux 精简镜像

---

## 📄 更新日志

### v0.68（当前版本）
- 🔐 安全性：API Key 哈希化、登录爆破防护、请求体大小限制、反代 IP 识别
- 🐳 部署：Docker 镜像支持 amd64 + arm64 多架构
- ⚡ 性能：通知渠道改为并发发送
- 🔧 易用性：API Key 一键重置、补全 LICENSE
- 🤖 CI/CD：push tag 自动构建发布 + 同步 Docker Hub 描述

### v0.66
- 📱 iOS Safari 添加到主屏幕适配：新增 4 个 apple-touch-icon PNG
- 🎨 iPhone/iPad 主屏快捷方式显示紫蓝渐变铃铛图标
- 🖼️ 图标按 favicon.svg 风格光栅化生成，视觉保持一致

### v0.65
- 📌 顶部导航栏与页面标题滚动时固定（sticky），长列表操作无需回滚到顶部
- 🎯 导航菜单自动高亮当前页面（桌面/移动端一致）
- 👤 标题栏始终显示登录用户名，窄屏不再隐藏
- 🧩 新增 `nav-active.js`，统一样式与逻辑
- 📱 API Key 列表手机端保留调用 URL 与复制按钮，操作列"调用"文案改为"数据"
- 🐛 修复日志页因 JS 缓存导致加载失败的问题（版本号统一提升）
- 🧹 Mattermost 编辑/新增只保留 webhook_url，去除高级设置

### v0.64
- 🔒 安全加固：删除无鉴权的遗留管理 API（`/api/channels` 等 CRUD）与调试端点
- 🛡️ 修复存储型 XSS：所有后台列表对外部输入做 HTML 转义
- 🔑 默认管理员密码改为随机生成（或通过 `ADMIN_PASSWORD` 指定），不再硬编码 `123456`
- 🧂 Session 密钥自动生成并持久化，消除硬编码默认值
- 🌐 CORS 默认同源，新增 `ALLOWED_ORIGINS` / `COOKIE_SECURE` 环境变量
- 🧹 Mattermost 渠道配置统一为 webhook_url，精简冗余代码
- 📱 API Key 列表页手机端保留 URL 复制按钮，操作按钮文案优化

### v0.63
- 📱 后台管理页面全面支持移动端浏览器访问
- 🍔 顶部导航在窄屏折叠为汉堡菜单，展开为 3 列网格快捷入口
- 📐 所有弹窗、表单、登录卡片自适应屏幕宽度
- 📊 数据表格在小屏自动隐藏次要列，并支持横向滚动
- 🔔 操作反馈由浏览器原生 alert 升级为右上角 Toast 提示
- 📝 工具栏、分页器、筛选栏在窄屏自动堆叠

### v0.62
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

### v0.58 及更早版本

早期版本更新日志请查看 [GitHub Releases](https://github.com/ttt216/notifycenter/releases)。

---

## 📄 许可证

MIT License

---

## 🙋 反馈与建议

如果你在使用中遇到问题、有新功能建议或想贡献代码，欢迎提交 Issue 或 Pull Request。

**祝使用愉快！** 🎉
