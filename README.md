# NodeSeek Daily Check-in

GitHub Actions 自动化：每天北京时间 2:00 自动登录 [NodeSeek](https://www.nodeseek.com/) 签到（随机模式）。

## 工作原理

1. 用 Puppeteer + stealth 插件模拟真实浏览器，过 Cloudflare Turnstile 验证
2. 注入你提供的登录态 cookie（`NS_COOKIE`），访问签到接口 `https://www.nodeseek.com/api/attendance?random=true`
3. 通过 `actions/cache` 持久化浏览器 profile（`cf_clearance`），避免每次重新过验证
4. 失败时输出 profile 调试信息

## 配置

### 必需 Secret

| 名称 | 说明 |
|------|------|
| `NS_COOKIE` | NodeSeek 浏览器导出的完整 cookie 字符串（含 `cf_clearance`、`session`、`pjwt` 等） |

### 可选 Secret

| 名称 | 说明 |
|------|------|
| `PUSHPLUS_TOKEN` | [pushplus](https://www.pushplus.plus/) 推送 token，签到结果会微信通知；不配则只输出到 Actions 日志 |

## 触发时间换算

`cron: '0 18 * * *'` = UTC 18:00 = 北京时间 02:00
