# BeeChat - 纯静态在线聊天室

基于 MQTT 公共代理的纯 HTML5 在线聊天室，上传到任意虚拟主机即可使用，零服务端依赖。

## 功能

- 实时多人聊天（MQTT WebSocket 同步，所有在线用户即时收到消息）
- 图片上传自动压缩（Canvas 压缩为 WebP/JPEG，最大 800px，小体积高保真）
- 图片灯箱点击放大
- 用户昵称系统（首次访问弹窗设置，localStorage 持久化）
- 管理后台（密码登录，SHA-256 验证，单条删除 / 全量清空）
- 每条消息显示发送者昵称
- 在线用户列表 + IP 地址（仅管理后台可见）
- 消息 IP 追溯（仅管理后台可见）
- 6 个 MQTT 代理自动切换（中国节点优先，WSS + WS 双协议）
- CDN 加载失败自动切换备用 CDN
- 发言频率限制

## 部署

将 `index.html` 上传到任意静态文件托管服务即可：

- GitHub Pages
- Vercel / Netlify
- 虚拟主机（FTP 上传）
- 任何 HTTP 服务器

```bash
# 本地测试
python -m http.server 3000
# 打开 http://localhost:3000
```

## 修改管理密码

1. 浏览器打开 `index.html`，按 F12 打开控制台
2. 运行以下命令获取新密码哈希：

```js
console.log(await crypto.subtle.digest('SHA-256', new TextEncoder().encode('你的新密码')).then(h => Array.from(new Uint8Array(h)).map(b => b.toString(16).padStart(2,'0')).join('')))
```

3. 将输出的哈希值替换 `index.html` 中 `ADMIN_PASSWORD_HASH` 的值

默认密码：`123456`

## 技术架构

| 组件 | 方案 |
|------|------|
| 实时通信 | MQTT over WebSocket（公共免费代理） |
| 消息同步 | publish/subscribe 模式 |
| 图片处理 | Canvas API 客户端压缩 → base64 嵌入 |
| 管理员认证 | SHA-256 客户端哈希比对 |
| IP 获取 | ipify / httpbin / ip.sb 多重备用 |
| MQTT 库 | mqtt.js v5.10.4（CDN） |
| 在线状态 | MQTT Last Will + 定期心跳 |

## 安全

- 管理密码 SHA-256 哈希，不传输明文
- 消息 XSS 防护（HTML 实体转义）
- 发言限速
- 图片 URL 仅限 MQTT 消息内传输，不外链

## 尾语
- 给分Ster⭐吧

## 许可

MIT
