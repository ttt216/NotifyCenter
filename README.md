<a href="https://afdian.com/item/9ac2dafac6e011f19a1552540025c377"><img width="2400" height="1350" alt="cover" src="https://github.com/user-attachments/assets/b0dc5275-a5bf-45f3-b8d7-1d5fc6916b13" /></a>

# 📬 NotifyCenter - 多渠道通知中心

**一个集中管理、开箱即用的通知转发服务**，让你通过一个统一的 API，把消息推送到 Bark、Telegram、Mattermost、企业微信、PushDeer 等多个平台。支持 JSON、表单、multipart、纯文本等多种请求体格式，原生兼容 Emby、群晖 DSM、Grafana、Uptime Kuma 等主流 Webhook 来源。适合个人自动化、Homelab、NAS 通知、媒体服务器、监控告警等场景。

<p align="center">  
  <img src="https://img.shields.io/badge/version-1.01-blue.svg" alt="version">  
  <img src="https://img.shields.io/badge/go-1.25+-00ADD8.svg" alt="go">  
  <img src="https://img.shields.io/badge/免费版%20%2B%20增强版%20Pro-blueviolet.svg" alt="edition">  
  <img src="https://img.shields.io/badge/docker-amd64%20%7C%20arm64-2496ED.svg" alt="docker">  
</p>

---

## 🔧 v1.01 更新亮点

- 📖 **使用指南重写**：目录支持二级跳转，12 项增强功能逐项说明
- 🕸️ **关系图谱改版**：三列布局 + 「新建流程」向导
- 📘 **API 文档补齐**：新增 `priority` 字段说明

---

## ✨ v1.0 更新亮点

- 🔑 **授权体系**：免费版 + 增强版单镜像，离线授权码解锁
- 📤 **送达保障** *（Pro）*：失败自动重试，重启不丢消息
- 🌙 **静默时段** *（Pro）*：夜间非紧急消息延后补发
- 📣 **消息升级与确认** *（Pro）*：超时未确认自动升级渠道
- 📥 **入站归档与重放** *（Pro）*：第三方回调可存档、可重放
- 🕘 **模板版本历史** *（Pro）*：diff 对比与一键回滚
- 🧩 **扩展渠道包** *（Pro）*：新增 ntfy / Gotify / Discord
- 📊 **监控指标** *（Pro）*：Prometheus `/metrics`

---

## 🎯 核心特性

| 特性                     | 说明                                                                                    |
| ---------------------- | ------------------------------------------------------------------------------------- |
| 🚀 **一键部署**            | 单 Docker 镜像，无需外部数据库，一条命令就能跑起来                                                         |
| 📡 **10 种渠道**          | Bark、Telegram、Mattermost、企业微信 x2、PushDeer、自定义 Webhook；ntfy / Gotify / Discord *（Pro）* |
| 🎬 **Emby / Jellyfin** | 播放/入库事件智能识别，自动生成图片 URL，独立模板；剧集入库支持按剧名合并 *（Pro）*                                       |
| 📮 **多格式请求体**          | 自动识别 JSON / 表单 / multipart / 纯文本                                                      |
| 📝 **模板系统**            | Pongo2/Jinja2 语法，支持变量、条件、循环；在线预览调试；版本历史 + diff + 回滚 *（Pro）*                           |
| 🔀 **路由规则**            | 一次 API 调用推送到多渠道，一 Key 多路由，每路由独立模板                                                     |
| 🔑 **API Key 认证**      | 路由级安全隔离，支持禁用、有效期、调用统计                                                                 |
| 🔓 **12 项增强功能**       | 送达保障、静默时段、消息升级、入站重放、模板版本、故障转移、监控指标等 —— 详见下文「**免费版 vs 增强版**」                          |
| 🔇 **通知去重**            | 相同内容窗口内自动合并，防轰炸；可配置窗口与最大合并次数                                                          |
| 📦 **配置导入/导出**         | 一键备份与恢复，跨实例迁移轻松搞定                                                                     |
| 📊 **可视化管理**           | Web 后台 + 仪表盘 + 趋势图 + 成功率 + 实时日志                                                       |
| 🌙 **深色模式**            | 所有页面深色/浅色切换，自动跟随系统                                                                    |
| 📘 **API 文档**          | 内置 Swagger UI / OpenAPI 3.0                                                           |

---

## 📦 支持的通知渠道

| 渠道              | 类型标识            | 简要说明                                             |
| --------------- | --------------- | ------------------------------------------------ |
| **Bark**        | `bark`          | iOS 免费推送 App，支持推送级别、角标、加密、自动复制、点击跳转、图标等          |
| **Telegram**    | `telegram`      | 官方 Bot API，支持个人 & 群组，HTML/Markdown 富文本           |
| **Mattermost**  | `mattermost`    | 开源团队协作平台，通过 Incoming Webhook 推送，支持 Markdown      |
| **企业微信（应用消息）**  | `wecom`         | 通过企业自建应用推送到 App，支持指定接收人/部门/标签，需配置可信 IP           |
| **企业微信（群机器人）**  | `wecom_webhook` | 通过群机器人 Webhook 推送到企业微信群，无需可信 IP，支持 text/markdown |
| **PushDeer**    | `pushdeer`      | 开源推送服务，支持官方 & 自建服务器，支持 markdown/text/image       |
| **自定义 Webhook** | `webhook`       | 对接任意支持 Webhook 的第三方服务（如 Slack、飞书、钉钉）             |
| **ntfy**        | `ntfy`          | 开源推送服务，支持自建服务器与优先级/标签 *（Pro）*                    |
| **Gotify**      | `gotify`        | 自建推送服务，支持优先级 *（Pro）*                             |
| **Discord**     | `discord`       | 通过 Discord Webhook 推送到频道 *（Pro）*                 |

> 💡 **渠道收费说明**：`ntfy` / `Gotify` / `Discord` 为 **v1.0 新增**的增强版（Pro）渠道；**原有 7 种渠道（Bark、Telegram、Mattermost、企业微信 x2、PushDeer、自定义 Webhook）继续免费**，不受授权影响。

---

## 🚀 快速开始

### 使用 Docker 一键运行

```bash
docker run -d \
  --name notifycenter \
  -p 5400:5400 \
  -v $(pwd)/data:/data \
  -e TZ=Asia/Shanghai \
  -e DATA_PATH=/data \
  --restart unless-stopped \
  ttt216/notifycenter:1.01
```

### 使用 Docker Compose

```yaml
services:
  notifycenter:
    image: ttt216/notifycenter:1.01
    container_name: notifycenter
    ports:
      - "5400:5400"
    volumes:
      - ./data:/data
    environment:
      - TZ=Asia/Shanghai
      - DATA_PATH=/data
      # - ADMIN_PASSWORD=your_secure_password
      # - ADMIN_SESSION_KEY=your_random_secret
      # 授权码与「本实例对外地址」都在「系统设置」页面配置，无需改这里
    restart: unless-stopped
```

执行 `docker compose up -d` 即可。

### 访问后台

打开浏览器访问：**<http://localhost:5400/admin/login>**

- 默认账号：`admin`（可通过 `ADMIN_USERNAME` 环境变量修改）
- 默认密码：
  - 若设置了 `ADMIN_PASSWORD` 环境变量，使用该密码
  - 否则首次启动使用**默认密码 `123456`**，并在启动日志中打印，请查看 `docker logs notifycenter` 获取

```bash
docker logs notifycenter 2>&1 | grep "管理员用户已创建"
```

> ⚠️ **首次登录会强制要求修改密码**，请设置一个安全的新密码（字母 + 数字组合，不少于 6 位）。

---

## 📖 怎么用？

装好之后**不用回来翻文档**——后台顶部菜单里有「**📖 指南**」，从建渠道到排错全在里面：

| 你在找什么 | 去哪看 |
| --- | --- |
| 5 分钟跑通（建渠道 → 模板 → 路由 → Key） | 📖 指南 → **0 跑通** |
| 某个字段怎么填、每种渠道要准备什么 | 📖 指南 → **2 分步操作** |
| Emby / Grafana / 群晖 / qBittorrent 怎么接 | 📖 指南 → **3 对接实战** |
| API 参数、`priority`、返回字段 | 📖 指南 → **4 调用 API**，或 📘 API（Swagger） |
| 发不出去 / 没收到 | 📖 指南 → **6 排错** |

> 指南在免费版里同样开放，不需要授权。

---

## 🆓 免费版 vs 🔓 增强版

两个版本**共用同一个 Docker 镜像**，装的时候不用选。区别只在于是否填了有效授权码。

**免费版没有时间限制**，长期自用完全够用。未授权时增强功能**不会报错、也不会丢消息**，只是不生效
（比如「剧集入库合并」会退化成逐条发送）。

| 能力 | 免费版 | 增强版 Pro |
| --- | :---: | --- |
| **通知渠道** | 7 种：Bark / Telegram / Mattermost / 企业微信 ×2 / PushDeer / 自定义 Webhook | **+3 种**：ntfy / Gotify / Discord |
| **Emby / Jellyfin** | 事件自动识别、主模板 + 事件子模板、在线预览 | **+ 剧集入库合并**（整季 40 条压成 1 条） |
| **模板系统** | Pongo2 语法：变量、条件、循环 | **+ 版本历史**：diff 对比、一键回滚 |
| **路由** | 一个模板绑多个渠道，一次调用全推 | **+ 备用渠道故障转移**（主渠道全挂自动切） |
| **通知去重** | ✅ 相同内容窗口内合并 | ✅ |
| **送达保障** | —（失败只记日志） | ✅ 持久化队列 + 退避重试 + 渠道熔断 + 死信人工重放 |
| **静默时段** | — | ✅ 夜间非紧急消息延迟到早间补发，可配穿透优先级 |
| **消息升级与确认** | — | ✅ 超时未确认自动重推 / 升级渠道，附确认链接 |
| **入站归档与重放** | — | ✅ 第三方回调原样存档、可重放 |
| **关系图谱** | — | ✅ 可视化 Key → 路由 → 渠道的引用关系 |
| **监控指标** | — | ✅ Prometheus `/metrics`，可接 Grafana |
| **通知重新发送** | — | ✅ 调用记录里一键重发 |
| **Jellyfin 噪音过滤** | — | ✅ 自动过滤播放进度类噪音 |
| **多格式请求体** | ✅ JSON / 表单 / multipart / 纯文本 | ✅ |
| **API Key** | ✅ 绑路由、可禁用、可设有效期 | ✅ |
| **配置导入导出** | ✅ | ✅ |
| **可视化后台** | ✅ 仪表盘、趋势图、调用记录、深色模式 | ✅ |
| **Swagger + 使用指南** | ✅ | ✅ |
| **授权期限** | 永久免费 | 买断永久 |

> ⚠️ **渠道说明**：`ntfy` / `Gotify` / `Discord` 是 v1.0 **新增**的 Pro 渠道，**原有 7 种渠道继续免费**，不要误读为「渠道整体收费」。

---

## 📦 数据与备份

所有数据（渠道、模板、路由、API Key、日志、授权码）都在容器内的 `/data` 目录里，**备份就是复制这个目录**，换机器迁移也是复制它。

> ⚠️ `data/` 含授权码与 API Key 的加密材料，备份文件请当作敏感数据保管。

小版本升级（1.0.x → 1.0.y）替换镜像重启即可，数据目录不用动。跨大版本升级的步骤见 [GitHub Releases](https://github.com/ttt216/NotifyCenter/releases)。

---

## 🎯 典型应用场景

| 场景                          | 说明                       |
| --------------------------- | ------------------------ |
| 📺 **Emby / Jellyfin 播放通知** | 家人开始观看/新入库剧集时推送到手机       |
| 🗄️ **群晖 DSM 系统通知**         | 群晖系统事件、备份任务、存储空间告警统一转发   |
| 🖥️ **监控告警**                | 服务器 CPU/内存告警推送到多个渠道      |
| 🤖 **自动化脚本通知**              | Python/Shell 脚本执行结果推送    |
| 📦 **下载完成提醒**               | qBittorrent/Aria2 下载完成通知 |
| 🔒 **登录/安全告警**              | 检测到异常登录立即推送              |
| 📊 **日报/周报推送**              | 定时任务生成的统计报告              |

---

## 📄 许可证与授权

**分发方式**：v1.0 起**不提供源码**，仅以 Docker 镜像形式分发。授权条款见 [LICENSE](https://github.com/ttt216/NotifyCenter/blob/main/LICENSE)。

**功能对比**：见上文「**免费版 vs 增强版**」。

授权码是**离线签名令牌**，**无需联网即可校验，不上报任何数据**。获取后在「系统设置 → 授权」中粘贴即可，页面会显示本机机器码，用于把授权码绑定到你的实例。换设备、换机器可凭购买记录**免费换绑，每份授权最多 3 次**。

---

## 💬 交流群

遇到问题想交流、想第一时间知道新功能？欢迎加入 QQ 群。

<p align="center">  
  <img src="https://github.com/user-attachments/assets/b184828d-969b-4224-82bd-e578476507be" width="200" alt="NotifyCenter 交流群二维码">  
</p>

<p align="center">
  <strong>群名称</strong>：NotifyCenter 交流群　｜　<strong>群号</strong>：<code>1109380940</code>
</p>

> 💡 也可以直接在 QQ 里搜索群号加入。后台「系统设置 → 💬 加入交流群」里同样能看到这张二维码。

---

## 🙋 反馈与建议

如果你在使用中遇到问题或有新功能建议，欢迎提交 Issue。

**祝使用愉快！** 🎉
