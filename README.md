# 📬 NotifyCenter - 多渠道通知中心

**一个集中管理、开箱即用的通知转发服务**，让你通过一个统一的 API，把消息推送到 Bark、Telegram、Mattermost、企业微信、PushDeer 等多个平台。支持 JSON、表单、multipart、纯文本等多种请求体格式，原生兼容 Emby、群晖 DSM、Grafana、Uptime Kuma 等主流 Webhook 来源。适合个人自动化、Homelab、NAS 通知、媒体服务器、监控告警等场景。

<p align="center">
  <img src="https://img.shields.io/badge/version-0.70-blue.svg" alt="version">
  <img src="https://img.shields.io/badge/go-1.21+-00ADD8.svg" alt="go">
  <img src="https://img.shields.io/badge/license-MIT-green.svg" alt="license">
  <img src="https://img.shields.io/badge/docker-ready-2496ED.svg" alt="docker">
</p>

---

## ✨ v0.70 更新亮点

- 🎬 **Jellyfin 模板支持**：新增 Jellyfin 专用模板类型，自动识别播放/入库事件，与 Emby 模板同等体验
- 🔇 **通知去重/防轰炸**：相同内容在时间窗口内自动合并，防止重复推送；可配置窗口、最大合并次数，重启配置不丢失
- 🧩 **剧集入库通知合并**：Emby / Jellyfin 的「新剧集入库」支持按剧名合并，一部 40 集的剧整季入库只发一条通知，可配置合并窗口
- 📦 **配置导入/导出**：一键导出所有渠道、模板、路由、API Key、系统配置为 JSON，支持备份与跨实例迁移
- 📊 **仪表盘统计增强**：近 7 天发送趋势图、各渠道成功率统计、最近失败日志，一目了然
- 🧪 **模板在线调试**：在模板编辑页直接输入测试数据，实时预览渲染结果，并明确提示命中「事件子模板」还是「主模板」
- 🧩 **模板编辑界面优化**：Emby/Jellyfin 模板的主模板与子模板分区色块展示，主模板默认填充内置内容并锁定防误改，勾选「自定义主模板」后可解锁编辑
- 🌙 **深色模式**：所有管理页面支持深色/浅色模式切换，自动跟随系统，夜间使用更护眼
- 📘 **API 文档（Swagger UI）**：内置 OpenAPI 3.0 文档，管理后台直接浏览所有 API 接口
- ⚙️ **系统设置页**：新增统一的设置管理页面，去重配置、导入导出集中管理

## ✨ v0.69 更新亮点

- 🔁 **发送可靠性**：渠道发送失败自动重试一次，缓解瞬时网络抖动导致的丢通知
- 🔐 **部署安全**：Docker 启动自动识别弱/占位 session key 并改用持久化随机密钥
- 🛡️ **企微回调校验**：URL 验证与消息接收均校验 ReceiverID 与渠道 CorpID 一致，防止伪造消息

---

## 🎯 核心特性

| 特性 | 说明 |
|------|------|
| 🚀 **一键部署** | 单 Docker 镜像，SQLite 内嵌，无需外部数据库 |
| 📡 **7 种渠道** | Bark、Telegram、Mattermost、企业微信 x2、PushDeer、自定义 Webhook |
| 🎬 **Emby / Jellyfin** | 播放/入库事件智能识别，自动生成图片 URL，独立模板；剧集入库支持按剧名合并 |
| 📮 **多格式请求体** | 自动识别 JSON / 表单 / multipart / 纯文本 |
| 📝 **模板系统** | Pongo2/Jinja2 语法，支持变量、条件、循环；在线预览调试 |
| 🔀 **路由规则** | 一次 API 调用推送到多渠道，一 Key 多路由，每路由独立模板 |
| 🔑 **API Key 认证** | 路由级安全隔离，支持禁用、有效期、调用统计 |
| 🔇 **通知去重** | 相同内容窗口内自动合并，防轰炸；可配置窗口与最大合并次数 |
| 📦 **配置导入/导出** | 一键备份与恢复，跨实例迁移轻松搞定 |
| 📊 **可视化管理** | Web 后台 + 仪表盘 + 趋势图 + 成功率 + 实时日志 |
| 🌙 **深色模式** | 所有页面深色/浅色切换，自动跟随系统 |
| 📘 **API 文档** | 内置 Swagger UI / OpenAPI 3.0 |

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
  ttt216/notifycenter:0.70
```

### 使用 Docker Compose

```yaml
services:
  notifycenter:
    image: ttt216/notifycenter:0.70
    container_name: notifycenter
    ports:
      - "5400:5400"
    volumes:
      - ./data:/app/data
    environment:
      - TZ=Asia/Shanghai
      # - ADMIN_PASSWORD=your_secure_password
      # - ADMIN_SESSION_KEY=your_random_secret
    restart: unless-stopped
```

执行 `docker compose up -d` 即可。

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

安装 Bark App → 复制完整推送 URL → 粘贴到 NotifyCenter 的 **API URL** 字段即可。支持推送级别、角标、分组、加密、自动复制等高级选项。

#### 💬 Telegram

通过 [@BotFather](https://t.me/BotFather) 创建 Bot 获取 Token → 获取 Chat ID（个人用 [@userinfobot](https://t.me/userinfobot)，群组用 `getUpdates`）→ 在 NotifyCenter 中填写 Bot Token 和 Chat ID。支持 HTML / MarkdownV2 富文本。

#### 🗣️ Mattermost

在 Mattermost 创建传入 Webhook → 复制 URL 到 NotifyCenter。可选用户名、头像、指定频道。支持 Markdown，图片用 `![]({{BackdropImgUrl}})` 可自动预览。

#### 🏢 企业微信（应用消息）

在企业微信管理后台创建自建应用，获取 CorpID、AgentID、Secret。⚠️ 需在"企业可信IP"中配置服务器出口 IP。支持指定接收人/部门/标签。

#### 🤖 企业微信（群机器人）

在群内添加群机器人获取 Webhook → 在 NotifyCenter 填写 Webhook Key（支持完整 URL 或纯 key）。支持 text / markdown 消息类型，无需配置可信 IP。

#### 📮 PushDeer

在 PushDeer App 生成 PushKey → 在 NotifyCenter 填写。支持官方 & 自建服务器，支持 markdown / text / image 三种消息类型。

#### 🌐 自定义 Webhook

对接任意 Webhook 服务（Slack、飞书、钉钉等）。可自定义 URL、Method、Headers、Body 模板。变量：`{{title}}`、`{{content}}`、`{{img_url}}`、`{{link_url}}`。

---

### 2️⃣ 配置模板

进入 **模板（Templates）** 菜单，点击"添加模板"。

**三种模板类型：**

- **general（通用模板）**：适用于普通 API 通知，配置标题和内容
- **emby（Emby 专用）**：自动识别 Emby Webhook 事件（播放开始/暂停/入库等），支持电影/剧集分别配置
- **jellyfin（Jellyfin 专用）**：自动识别 Jellyfin Webhook 事件，与 Emby 模板同等体验

**模板语法**：使用 Pongo2/Jinja2 语法，支持变量替换、条件判断、循环等。

示例：
```
🎬 {{ UserName }} 正在观看 {{ ItemName }}
📺 设备：{{ DeviceName }}
⏱️ 进度：{{ ProgressPercent }}%
```

**🧩 主模板与事件子模板**（仅 Emby / Jellyfin 类型）：

- **主模板**：匹配不到事件子模板时兜底渲染。新建时会自动填充系统内置内容，默认锁定防止误改；确实需要修改时勾选「自定义主模板」，确认后即可编辑。
- **事件子模板**：按事件类型（电影播放、剧集播放、新电影入库等）分别配置，优先级高于主模板，推荐日常使用。
- 界面上两个区域用不同底色区分，主模板在上、子模板在下，便于快速定位。

**🧪 在线调试**：在模板编辑页点击「预览」按钮，输入测试数据即可实时查看渲染结果，无需实际发送通知；预览结果会标注本次命中的是事件子模板还是主模板。

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

```bash
curl -X POST "http://your-server:5400/api/service/notify?api_key=your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"title":"通知标题","content":"通知内容","img_url":"...","link_url":"..."}'
```

**自动识别请求格式**：支持 JSON / 表单 / multipart / 纯文本，通用模板自动归一化字段（`text/message/msg/body` → `content`，`subject/headline` → `title`）。

**直接对接 Emby / Jellyfin / Grafana / Uptime Kuma / 群晖 DSM** 等主流 Webhook，无需额外配置。

---

## 🎬 Emby / Jellyfin 专用模板详解

NotifyCenter 完整支持 Emby 和 Jellyfin 媒体服务器，两者体验一致，仅事件名与 Webhook 格式不同。

### 自动事件识别

| 媒体服务器 | 播放事件 | 入库事件 |
|-----------|---------|---------|
| **Emby** | `playback.start/stop/pause/unpause` → `playback.movie` / `playback.episode` | `library.new` → `library.new_movie` / `library.new_episode` |
| **Jellyfin** | `PlaybackStart/Stop/Pause/Unpause` → `playback.movie` / `playback.episode` | `ItemAdded` → `library.new_movie` / `library.new_episode` |

### 可用变量（Emby & Jellyfin 通用）

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
| `EpisodeImgUrl` | 单集画面（仅剧集播放时有值） |
| `ProgressMinutes` / `TotalMinutes` / `ProgressPercent` | 播放进度 |
| `EpisodeCount` / `EpisodeList` / `FirstEpisode` | 合并后的集数 / 集号列表（如 `S01E01-E40`）/ 首集标题（仅入库合并场景有值） |
| `Episodes` | 合并后的每集数组，可配合 `{% for ep in Episodes %}` 遍历（每项含 `Season` / `Episode` / `ItemName`） |

> **图片 URL 说明**：需在模板配置中填写 **Emby / Jellyfin 服务器地址**（如 `https://emby.example.com:8086`），系统自动生成图片 URL。不填写则图片变量为空字符串。

### 模板匹配规则

渲染时按以下优先级匹配，逐级回退：

1. **事件子模板**：按识别出的事件类型精确匹配（如 `playback.episode`）
2. **主模板**：未匹配到事件子模板时使用，新建模板时自动填充系统内置内容
3. **内置默认模板**：主模板为空时由系统兜底

主模板默认锁定不可编辑，如需修改请勾选编辑页的「自定义主模板」并确认。模板预览会显示本次命中层级，便于排查。

### 🧩 剧集入库通知合并

当 Emby / Jellyfin 整季批量入库时，每一集都会触发一条通知，40 集的剧一次入库就会收到 40 条消息。开启「合并同一剧集的入库通知」后：

- 系统按 **剧名 + 路由** 分组，把短时间内到达的同一部剧的多集**合并成一条**通知推送
- **合并窗口**默认 60 秒（可调 10–300 秒），从首条通知到达时开始计时，到点统一发送
- 合并后可使用 `EpisodeCount`（本次集数）、`EpisodeList`（集号列表，如 `S01E01-E40`）、`FirstEpisode`（首集标题）、`Episodes`（每集数组）等变量自定义模板
- 只入库 1 集时行为与未开启完全一致（仅多一个窗口延迟）
- 合并状态仅存在内存，进程重启会丢失尚未到期的批次（最多丢一个窗口内的通知）

> 💡 在模板编辑页 → 「📺 新剧集入库」子模板 → 勾选「合并同一剧集的入库通知」即可启用。该选项仅在勾选了该事件子模板时可用。

### 直接对接 Webhook

在 Emby 或 Jellyfin 的 Webhook 插件配置中填入：
```
http://your-server:5400/api/service/notify?api_key=your-api-key
```
NotifyCenter 自动识别来源类型，无需额外配置（包括带截图的 multipart 请求）。

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

## ⚙️ 系统设置

进入 **设置（Settings）** 菜单集中管理：

### 🔇 通知去重

防止相同内容短时间重复推送（如监控反复告警）。

- **启用去重**：总开关
- **时间窗口**：默认 60 秒，相同内容重复发送会被合并
- **最大合并次数**：默认 10 次，超过后仍然发送（防止完全静默）
- 按路由独立计数（不同路由分别去重）

### 📦 配置导入/导出

一键备份或迁移所有配置（渠道、模板、路由、API Key、系统配置）。导入支持 `skip`（跳过已存在）和 `overwrite`（覆盖）两种模式。

> 💡 **升级迁移**：导出旧版配置 → 安装新版 → 导入配置，即可完整迁移。

---

## 📘 API 文档

管理后台内置 **Swagger UI** 交互式 API 文档，访问：

```
http://your-server:5400/admin/api-docs
```

包含所有通知 API、管理 API 的完整接口定义、请求参数、响应示例。

---

## 💾 数据持久化

- 数据存储在 SQLite 数据库中，位于容器内 `/app/data/notifycenter.db`
- 通过挂载 `./data:/app/data` 可以持久化数据
- **升级版本时数据完全兼容**，只需替换镜像重启即可

### 从旧版本升级

1. 停止旧容器
2. 备份 `data/notifycenter.db` 文件（推荐）
3. 使用最新版本镜像创建新容器，挂载相同的 `data` 目录
4. 数据库结构完全兼容，自动完成必要迁移

> ✅ v0.70 为**无感升级**：不新增表、不新增列，所有新功能在已有 schema 上运行。回滚也无副作用。

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

### 忘记管理员密码？

设置 `ADMIN_PASSWORD` 环境变量仅在用户表为空时生效。若已有用户，可删除 `data/notifycenter.db` 重新初始化（⚠️ 会丢失所有配置），或在容器内用 sqlite3 直接修改。

### 如何测试渠道是否配置正确？

在**渠道管理**页面，每个渠道右侧都有**测试**按钮，点击后填写测试内容即可发送，并显示详细响应信息。

### 支持 arm64 / Apple Silicon 吗？

支持。镜像为多架构（amd64 + arm64），`docker pull` 自动匹配本机架构。

### 企业微信应用消息推送失败？

通常是未配置"企业可信IP"。在企业微信管理后台 → 应用详情 → 企业可信IP，添加服务器出口 IP。

---

## 🛠️ 技术栈

- **后端**：Go 1.21+ / Gin / GORM / SQLite
- **模板引擎**：Pongo2（Jinja2 兼容）
- **前端**：原生 HTML + Tailwind CSS + Vanilla JS
- **容器**：Alpine Linux 精简镜像

---

## 📄 更新日志

### v0.70（当前版本）
- 🎬 **Jellyfin 模板支持**：新增 Jellyfin 专用模板类型，自动识别播放/入库事件，完整变量与图片 URL 支持
- 🔇 **通知去重/防轰炸**：基于 SHA256 的内容去重，可配置时间窗口与最大合并次数；配置持久化到数据库
- 🧩 **剧集入库通知合并**：Emby / Jellyfin 的「新剧集入库」支持按剧名合并整季批量入库通知，合并窗口可调（10–300s），合并后提供 `EpisodeCount` / `EpisodeList` / `Episodes` 等变量
- 📦 **配置导入/导出**：一键导出/导入渠道、模板、路由、API Key、系统配置，支持 skip/overwrite 两种模式
- 📊 **仪表盘统计增强**：近 7 天发送趋势图、各渠道成功率统计、最近失败日志
- 🧪 **模板在线调试**：模板编辑页内置预览功能，输入测试数据实时查看渲染结果，并标注命中的模板层级
- 🧩 **模板编辑界面优化**：主模板 / 子模板分区色块展示；主模板默认填充内置内容便于直接入库，默认锁定，勾选「自定义主模板」确认后解锁
- 🔧 修复 Jellyfin 模板服务器地址标签显示为「Emby 服务器地址」的问题
- 🔧 修复 Jellyfin 模板进度为空时渲染出「进度：未知%」的问题
- 🔧 修复勾选/取消「自定义主模板」时标题与内容输入框可编辑状态不联动的问题
- 🌙 **深色模式**：所有管理页面支持深色/浅色主题切换，Swagger UI 同步适配
- 📘 **API 文档**：内置 OpenAPI 3.0 规范 + Swagger UI 可交互浏览
- ⚙️ **系统设置页**：去重配置、导入导出集中管理
- 🔧 修复 Pongo2 `{{ var or "默认值" }}` 渲染为 "True" 的兼容性问题

### v0.69
- 🔁 渠道发送失败自动重试一次，缓解瞬时网络抖动
- 🔐 Docker 启动自动识别弱 session key 并改用持久化随机密钥
- 🛡️ 企业微信回调 URL 验证与消息接收均校验 ReceiverID

### v0.68
- 🔐 API Key 哈希化、登录爆破防护、请求体大小限制
- 🐳 Docker 镜像支持 amd64 + arm64 多架构
- ⚡ 通知渠道并发发送
- 🔧 API Key 一键重置

### 更早版本

请查看 [GitHub Releases](https://github.com/ttt216/notifycenter/releases)。

---

## 📄 许可证

MIT License

---

## 🙋 反馈与建议

如果你在使用中遇到问题、有新功能建议或想贡献代码，欢迎提交 Issue 或 Pull Request。

**祝使用愉快！** 🎉
