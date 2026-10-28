# Glados自动签到

## 食用方式：

### 注册一个GLaDOS的账号([注册地址](https://glados.space/landing/0A58E-NV28S-6U3QV-33VMG))

#### 我的邀请码：([0A58E-NV28S-6U3QV-33VMG](https://0a58e-nv28s-6u3qv-33vmg.glados.space)) 

### Fork 并 Star 本仓库

1. 点击页面右上角的 **Fork**，把本仓库复制到自己的 GitHub 账号下。
2. 进入 Fork 后的仓库，点击 **Star**，避免仓库长期无活动导致 Actions 停用。
3. 后续所有 Secret 和 Actions 配置都在你 Fork 后的仓库中完成。

![Fork 和 Star 示例](imgs/github-buttons.png)

### 添加 Secret

1. 跳转至自己的仓库的 `Settings` -> `Secrets and variables` -> `Actions`。

2. 添加必须的 Repository Secret，命名为 `GLADOS_COOKIES`：

推荐使用浏览器扩展 **Cookie-Editor** 或按 `F12` 获取：

- 登录 [GLaDOS](https://glados.cloud) 后，点击浏览器扩展栏里的 **Cookie-Editor**（或按 `F12` 切换到 **Application** -> **Cookies**）。
- 确认包含完整的 Session 字段及其签名（新版为 `gld:sess` 和 `gld:sess.sig`；旧版为 `koa:sess` 和 `koa:sess.sig`）。
- **必须成对包含两个字段**，例如：
  `gld:sess=...; gld:sess.sig=...;` （或 `koa:sess=...; koa:sess.sig=...;`）
- 将该完整字符串填入 `GLADOS_COOKIES`。
- 多账号请在 `GLADOS_COOKIES` 中使用 `&` 连接多个 Cookie（例如：`cookie1&cookie2`）。

> ⚠️ **防止 Cookie 频繁/第二天失效的关键注意事项**：
> 1. **抓取后切勿点击“退出登录（Log out）”**：点击退出会导致服务端 Session 立即销毁作废，抓取后直接关闭标签页即可。
> 2. **避免多端频繁重新登录**：如果在手机端或其它浏览器再次登录，服务端会刷新 Session，之前配置给 GitHub Actions 的 Cookie 就会被顶替失效。

3. 添加可选的 Repository Secret `GLADOS_USER_AGENT`（强烈推荐，防止被风控）：
- GLaDOS 会校验发起请求的设备指纹。若 Actions 运行时的 User-Agent 与抓取 Cookie 的浏览器不一致，极易被判定为异常并吊销 Session。
- 在抓取 Cookie 的同一个浏览器中按 `F12` 打开控制台（Console），输入 `navigator.userAgent` 并回车复制完整 UA。
- 添加 Secret `GLADOS_USER_AGENT` 并填入该字符串。

4. 配置积分兑换策略（非必须）

- 添加 1 个 Repository Secret，命名为 `GLADOS_EXCHANGE_PLAN`，配置自动兑换积分策略：

| 值 | 积分要求 | 兑换天数 |
|---|---------|---------|
| `plan100` | 100 积分 | 10 天 |
| `plan200` | 200 积分 | 30 天 |
| `plan500` | 500 积分 | 100 天 (默认) |

> 不配置时默认为 `plan500`，即积分达到 500 时自动兑换 100 天


## 文件结构

```shell
│  checkin.py	# 签到脚本
│
├─.github
│  └─workflows
│          gladosCheck.yml	# Actions 配置文件
```

## 更新日志

- **2026-01**: 重构代码，添加log输出方便定位，支持新版网址，支持配置积分兑换策略。
- **2026-07**: 默认仅使用 `glados.cloud` 签到，移除手机推送配置，优化环境变量与日志输出。


## 问题排查与定位

大家可以通过查询 Actions 中的 `Running checkin` 日志快速定位问题。

正常运行时，日志会显示加载 Cookie、执行签到、查询剩余天数、查询总积分和签到总结：

![Actions 日志示例](imgs/actions-log-example.png)

如果日志中出现 Cookie 无效、未授权或请求失败，请重新使用 Cookie-Editor 获取并更新 `GLADOS_COOKIES`。

## 声明

本项目不保证稳定运行与更新, 因GitHub相关规定可能会删库, 请注意备份







