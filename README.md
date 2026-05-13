# grok_ttk_run

一个基于 Tkinter + DrissionPage 的 Grok 注册工具（本地 GUI 版）。

## 当前版本说明

- 已移除 NSFW 自动开启模块
- 已移除教程弹窗与教程按钮
- 保留 Cloudflare / DuckMail / YYDS 邮箱接入能力
- 支持并发注册（线程数可配置）
- 成功账号实时写入 `accounts_YYYYMMDD_HHMMSS.txt`

## 运行环境

- Windows
- Python 3.12 / 3.13（推荐）

## 安装依赖

```bash
pip install -r requirements.txt
```

如果项目中没有 `requirements.txt`，至少需要：

```bash
pip install DrissionPage curl_cffi
```

## 启动方式

```bash
python grok_register_ttk.py
```

## 主要配置项（GUI）

- 邮箱服务商（duckmail / yyds / cloudflare）
- 注册数量
- 并发线程
- 代理（可选）
- Cloudflare API Base / Key / 鉴权模式 / 路径
- grok2api 自动入池（本地 / 远端）

## 输出文件

- `accounts_*.txt`：成功账号导出（email / password / sso）
- `mail_credentials.txt`：邮箱地址与邮箱 token 记录（用于排查验证码链路）

## 注意事项

- 高并发会增加浏览器启动冲突概率，建议先小并发压测后再拉高
- 如果验证码拉取异常，优先检查邮箱 API 路径与鉴权模式
- 代理不稳定会直接影响注册和收信链路

## 安全建议

- 发布给他人前请清理 `config.json`、`accounts_*.txt`、`mail_credentials.txt` 等敏感文件
- 推荐使用 `config.example.json` 作为分发模板

