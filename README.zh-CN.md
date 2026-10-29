<div align="center">

# DbCat

**原生、轻快的 macOS 与 Windows 数据库客户端，用 Rust 编写**

[![Version](https://img.shields.io/github/v/release/dbcat-app/dbcat?color=blue&label=version)](https://github.com/dbcat-app/dbcat/releases)
[![Platform](https://img.shields.io/badge/platform-macOS%2012%2B%20%7C%20Windows%2010%2F11%20x64-lightgrey)](https://github.com/dbcat-app/dbcat/releases)
[![Arch](https://img.shields.io/badge/arch-Apple%20Silicon%20%7C%20Intel%20%7C%20x64-success)](https://github.com/dbcat-app/dbcat/releases)
[![Notarized](https://img.shields.io/badge/Apple-Notarized-green)](https://github.com/dbcat-app/dbcat/releases)
[![Built with Rust](https://img.shields.io/badge/built%20with-Rust-orange)](https://www.rust-lang.org/)

[官方网站](https://dbcat.forzens.com) · [下载](#-下载) · [问题反馈](https://github.com/dbcat-app/dbcat/issues) · [更新日志](https://github.com/dbcat-app/dbcat/releases)

[English](README.md) | **简体中文**

<img src="docs/screenshots/grid.webp" alt="浏览表数据并打开行详情面板" width="100%">

</div>

---

## ⚡ 为什么选择 DbCat

不少数据库工具基于 Electron、WebView 或 Java 运行时，启动慢、占内存。DbCat 用 Rust 原生编写，界面由 GPU 直接绘制：

- 🦀 **原生，不套浏览器**：没有 Electron 与 WebView，打开即用。
- 🚀 **大表也不卡**：分页按需取数（每页 300 行），百万行 MySQL 表打开首页实测约 1.3 ms；所有数据库调用都在后台线程执行，慢查询期间界面照常响应。
- 🛡️ **密码只进系统钥匙串**：数据库、SSH 的密码与私钥口令只保存在 macOS 钥匙串或 Windows 凭据管理器中；不写入配置文件，不出现在日志里，不内置任何统计或追踪。
- 🚦 **生产库护栏**：只读连接、写前确认，数据修改先暂存、预览 SQL 后一次提交。

---

## 🗄️ 支持的数据库

| 类型 | 数据库 | 支持说明 |
| :--- | :--- | :--- |
| 关系型 | **MySQL** | 5.7 / 8.x；外键、触发器、存储过程；整库与多 Schema 备份恢复 |
| 关系型 | **MariaDB** | 10.x / 11.x；序列、CHECK 约束、账户锁定与角色 |
| 关系型 | **TiDB** | 兼容 MySQL 的分布式库 · 执行计划、备份恢复 |
| 关系型 | **PostgreSQL** | 多 Schema、物化视图、扩展；整库与多 Schema 备份恢复 |
| 关系型 | **CockroachDB** | 兼容 PostgreSQL 的分布式库 · 执行计划、备份恢复 |
| 关系型 | **SQL Server** | T-SQL、Schema、Windows / Kerberos / Entra ID 认证 |
| 嵌入式 | **SQLite** | 本地文件，零配置 |
| 嵌入式 | **DuckDB** | 本地 .duckdb 文件 · 就地编辑 |
| 分析型 | **ClickHouse** | MergeTree 表选项、分区与排序键 |
| 键值型 | **Redis** | 键浏览、六种数据类型、控制台、Pub/Sub、ACL；单机 / 哨兵 / 集群 |
| 键值型 | **Valkey** | 与 Redis 能力一致：键浏览、控制台、Pub/Sub、ACL、哨兵与集群 |
| 文档型 | **MongoDB** | 表格 / JSON / 树三种视图就地编辑，mongosh 语法查询，索引与校验规则 |

> 更多数据库正在陆续加入。

---

## ✨ 主要功能

- **📊 表格浏览与就地编辑**：分页加载，百万行表秒开；13 种过滤条件、服务端排序；改动先暂存，预览 SQL 后一次提交。
- **📝 查询编辑器**：按上下文补全表和列，支持查询参数、多结果集、分屏对照与执行计划；超时与行数上限保护。
- **📐 可视化结构设计**：编辑列、索引、外键与触发器，变更先生成 DDL 供确认。
- **🕸️ ER 关系图**：自动布局整库的表与外键，可搜索定位、只看相关表，双击打开表。
- **📦 导入导出与备份**：CSV / JSON / SQL 导出、CSV 导入；MySQL、MariaDB、TiDB、PostgreSQL、CockroachDB、SQL Server、SQLite 与 ClickHouse 整库与多 Schema 备份恢复，以及 MongoDB、Redis 与 Valkey 备份恢复。
- **🤖 AI 接入（MCP）**：内置 MCP 服务器，在设置里一键添加到 Claude Code、Codex、Cursor、Claude Desktop 等 AI 工具，AI 工具需要时会在后台自动启动 DbCat。默认关闭，每个连接单独授权（关闭 / 只读 / 修改需审批 / 修改免审批）；AI 提出的修改在 DbCat 里展示语句、预计影响行数与理由，由你批准后执行，密码不经过 AI 工具。
- **🔒 安全连接**：SSH 隧道（密码 / 私钥 / ssh-agent）、SSL/TLS 证书校验，生产环境可用颜色标记。
- **⌨️ 键盘优先**：`⌘P`（Windows 上为 `Ctrl+P`）快速打开表、`⌘K`（`Ctrl+K`）切换数据库，多标签与分屏，常用操作都有快捷键。
- **🌐 中英双语界面**：默认跟随系统语言，可在偏好设置中切换。

---

## 🖼️ 截图

<table>
  <tr>
    <td width="50%"><img src="docs/screenshots/sql.webp" alt="SQL 查询编辑器与结果"><br><sub><b>查询编辑器</b>：结果集与选区统计</sub></td>
    <td width="50%"><img src="docs/screenshots/er.webp" alt="ER 关系图"><br><sub><b>ER 关系图</b>：自动布局</sub></td>
  </tr>
  <tr>
    <td width="50%"><img src="docs/screenshots/redis-zset.webp" alt="Redis 有序集合浏览"><br><sub><b>Redis</b>：键浏览与值编辑</sub></td>
    <td width="50%"><img src="docs/screenshots/mongo-tree.webp" alt="MongoDB 文档树视图"><br><sub><b>MongoDB</b>：文档树视图</sub></td>
  </tr>
</table>

---

## 📥 下载

支持 macOS 12+ 与 Windows 10/11 (x64)。macOS 安装包已通过 Apple 公证，两个平台都内置自动更新。

- 🍏 **[macOS · Apple 芯片版](https://dbcat.forzens.com/download?arch=arm64&lang=zh)**
- 💻 **[macOS · Intel 版](https://dbcat.forzens.com/download?arch=x86_64&lang=zh)**
- 🪟 **[Windows 版 (x64)](https://dbcat.forzens.com/download?os=windows&lang=zh)**
- 历史版本与更新说明：[Releases](https://github.com/dbcat-app/dbcat/releases)

> **平台说明**
> - **macOS**：内置自动更新，有新版本时提示一键升级，并自动匹配你的 Mac 芯片。
> - **Windows**：未签名 Beta 安装包；若弹出 SmartScreen 提示，点「更多信息」→「仍要运行」；内置自动更新（0.7.0 / 0.7.1 用户需从官网手动安装一次新版）。

---

## 💬 反馈

- [GitHub Issues](https://github.com/dbcat-app/dbcat/issues)
- 应用内：菜单栏 `帮助 → 报告问题…`，会预填版本号与系统版本。

本仓库只用于发布版本与问题反馈，不包含源代码。
