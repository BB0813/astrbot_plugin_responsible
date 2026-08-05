# 项目概述：bot_responsible – AstrBot 人际关系管理插件

## 1. 项目简介

这是一个为 [AstrBot](https://github.com/AstrBotDevs/AstrBot) 框架开发的人际关系管理插件，提供黑名单管理、好友/群组管理、请求自动处理与通知等功能，帮助 Bot 管理员高效维护社交关系安全。

## 2. 技术栈

- **编程语言**：Python 3.8+
- **运行框架**：AstrBot（基于 OneBot v11/v12 协议）
- **数据存储**：JSON 文件（本地持久化）
- **依赖**：AstrBot 内置 API、OneBot 标准事件（request.friend, request.group 等）
- **实验性功能**：部分操作依赖 SnowLuma 的 `send_packet`（非标准 OneBot 能力）

## 3. 项目结构

```
bot_responsible/
├── .gitignore
├── LICENSE                # MIT 许可证
├── README.md              # 完整使用文档
├── main.py                # 插件主入口，注册事件监听与命令处理
├── metadata.yaml          # 插件元数据（名称、版本、作者等）
├── pkg.py                 # 工具函数或数据持久化逻辑
└── .sakura/               # 插件内部数据与文档目录
    ├── docs/              # 插件文档（可能存放说明或帮助）
    ├── memory/            # 运行时记忆或状态缓存
    └── rules/             # 规则定义（如黑名单匹配规则）
```

- **main.py**：核心模块，实现所有命令（`/好友`、`/拉黑`、`/同意` 等）和事件监听（好友申请、群邀请、Bot被踢等）。
- **pkg.py**：辅助模块，封装数据读写（blacklist.json、pending.json）、操作封装等。
- **metadata.yaml**：AstrBot 插件规范所需，定义插件基本信息。
- **.sakura/**：自定义存储目录，用于存放插件文档、运行时记忆和规则文件，符合 AstrBot 插件数据隔离规范。

## 4. 开发约定

- **事件驱动**：插件以事件监听为核心，响应 `request.friend`、`request.group`、`group.increase`、`group.decrease` 等 OneBot 标准事件。
- **命令风格**：所有管理员命令以 `/` 开头，支持别名（如 `/好友` 与 `/fl`），并遵循统一的参数格式（空格分隔，支持批量操作）。
- **权限控制**：命令仅限管理员执行，自动处理逻辑（如拒绝黑名单请求）不占用权限。
- **数据持久化**：采用 JSON 文件存储，路径可配置（默认 `data` 目录），关键文件包括 `blacklist.json`（黑名单）和 `pending.json`（待处理请求）。
- **通知机制**：支持群通知和私聊通知两种模式，通过 `/通知群` 命令切换；未设置时默认私聊通知管理员。
- **引用处理**：支持通过引用消息快速处理请求（同意/拒绝/拉黑），也支持通过编号操作，提高交互效率。
- **实验性功能标记**：对于非标准 OneBot 能力（如主动加好友、加群），在文档中明确标注依赖条件，避免兼容性问题。
- **模块化**：主逻辑、工具函数、元数据分离，便于扩展和维护。