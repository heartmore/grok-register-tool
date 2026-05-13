# grok-register-tool

## QQ 交流群

如需交流使用经验、参数配置、排障思路，可加入：

- **QQ 群：1060714372**

---

一个面向 Windows 的 Grok 注册自动化工具，采用 `Tkinter + DrissionPage + curl_cffi` 实现可视化批量流程。

项目目标是把“打开注册页 -> 创建临时邮箱 -> 轮询验证码 -> 填写注册资料 -> 获取 sso”这一整套链路做成可配置、可并发、可落盘、可复现的本地 GUI 工具，方便长期稳定运行与排障。

## 当前版本特性（真实状态）

- GUI 可视化配置与运行日志（实时滚动）
- 支持邮箱提供商：
  - DuckMail
  - YYDS
  - Cloudflare Temp Mail（自定义 API Base / 鉴权模式 / 路径）
- 支持并发注册（线程数可调，带启动节流）
- 注册成功后自动提取并保存：
  - 邮箱地址
  - 注册密码
  - `sso` token
- 支持 grok2api 自动入池：
  - 本地 token 文件入池
  - 远端接口入池（可选）
- 保留详细调试日志，便于定位验证码、邮箱 API、浏览器连接等问题

> 说明：本仓库版本已移除 NSFW 自动开启模块与教程弹窗/教程按钮。

## 运行环境

- 系统：Windows
- Python：3.12 / 3.13（推荐）
- 浏览器：本地 Chromium 内核环境（由 DrissionPage 驱动）

## 安装依赖

如有 `requirements.txt`：

```bash
pip install -r requirements.txt
```

若没有，可先安装最小依赖：

```bash
pip install DrissionPage curl_cffi
```

## 启动方式

```bash
python grok_register_ttk.py
```

## 主要配置项（GUI）

- 邮箱服务商：`duckmail / yyds / cloudflare`
- 注册数量：本轮总任务数
- 并发线程：建议先小并发压测，再逐步提升
- 代理（可选）：如 `http://127.0.0.1:7890`
- Cloudflare 配置：
  - API Base
  - API Key（如需要）
  - 鉴权模式（none / bearer / x-api-key / query-key）
  - 四段路径（domains/accounts/token/messages）
- grok2api 自动入池：
  - 本地入池开关 + token 文件路径
  - 远端入池开关 + base + app key

## 输出文件

- `accounts_YYYYMMDD_HHMMSS.txt`
  - 成功账号导出（`email----password----sso`）
- `mail_credentials.txt`
  - 邮箱地址与邮箱侧 token/JWT 记录（用于验证码链路排查）

## 常见排障建议

- 验证码收不到：优先核对邮箱 API 路径、鉴权模式、域名配置
- HTTP 401：通常为 key/JWT/鉴权头不匹配
- 浏览器偶发连接失败：降低并发、提高线程启动间隔、重启后重试
- 使用代理时超时：先直连验证，再决定是否保留代理

## 安全与分发建议

发布给他人前请清理以下敏感文件：

- `config.json`
- `accounts_*.txt`
- `mail_credentials.txt`

推荐分发时仅保留示例配置（如 `config.example.json`）。

