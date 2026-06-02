# CC Switch 与 WSL opencode 配置指南

## 什么是本地路由？

本地路由（Local Routing）是指将本机应用程序发出的 API 请求先经过一个本地代理服务，再由这个代理转发到目标 API 服务器。CC Switch 的本地路由就是在 Windows 上启动一个代理服务（默认 `http://127.0.0.1:15721`），让 Claude、Codex、Gemini 等桌面客户端的请求先经过它，实现统一管理、日志记录、故障转移等功能。

---

## WSL 中的 opencode 能否使用 CC Switch？

### 结论：不能（目前）

CC Switch 的本地路由只支持**预设的特定应用**（Claude、Codex、Gemini），不支持自定义 Provider 或通用 OpenAI 兼容代理。opencode 使用的模型（deepseek、qianwen、openrouter 等）不在支持列表中，CC Switch 无法识别和转发这些请求。

### 例外情况

如果以后 CC Switch 增加了自定义 Provider / 通用代理功能，可以通过以下方式连接：

### WSL 访问 Windows CC Switch 的方法

#### 情况 A：WSL2（推荐）

WSL2 中，`localhost` 理论上会自动转发到 Windows 的 `localhost`。如果不生效，解决方法：

1. **通过 WSL 网关 IP 访问**（实测可行）

   ```bash
   # 在 WSL 中查看 Windows 主机 IP
   ip route show default | awk '{print $3}'
   ```

   示例输出：`172.20.64.1`

2. **netsh 端口转发**（在 Windows 管理员 PowerShell 中执行）

   ```powershell
   netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=15721 connectaddress=127.0.0.1 connectport=15721
   ```

   删除转发：

   ```powershell
   netsh interface portproxy delete v4tov4 listenport=15721
   ```

3. **防火墙放行**

   ```powershell
   New-NetFirewallRule -DisplayName "CC Switch" -Direction Inbound -Protocol TCP -LocalPort 15721 -Action Allow
   ```

   删除规则：

   ```powershell
   Remove-NetFirewallRule -DisplayName "CC Switch"
   ```

#### 情况 B：WSL1

WSL1 中 `localhost` 可以直接访问 Windows 服务，不需要额外配置。

---

## opencode 配置修改（如果以后要用 CC Switch）

将 opencode 配置中所有 provider 的 `baseURL` 改为指向 CC Switch：

```json
"options": {
  "apiKey": "your-api-key",
  "baseURL": "http://WINDOWS_HOST_IP:15721/v1"
}
```

- WSL2 推荐用网关 IP（如 `http://172.20.64.1:15721/v1`）
- 如果 localhost 转发正常，也可以用 `http://localhost:15721/v1`
- 所有 provider 的 `npm` 需要统一为 `@ai-sdk/openai-compatible`

---

## 当前 opencode 配置

目前使用直连方式，各 provider 直接调用官方 API：

| Provider   | 端点                                        |
| ---------- | ------------------------------------------- |
| deepseek   | `https://api.deepseek.com/v1`               |
| openrouter | `https://openrouter.ai/api/v1`              |
| qianwen    | `https://dashscope.aliyuncs.com/compatible-mode/v1` |

配置文件路径：`C:\Users\Lenovo\.config\opencode\opencode.json`

---

## 常用命令

| 用途             | 命令                                                              |
| ---------------- | ----------------------------------------------------------------- |
| 测试 CC Switch   | `curl -v http://localhost:15721/v1/models`                        |
| 查看 WSL 版本    | `wsl -l -v` (Windows PowerShell)                                  |
| 查看 WSL 网关 IP | `ip route show default` (WSL)                                     |
| 查看 WSL 自身 IP | `ip addr show` (WSL)                                              |
| 查看防火墙规则   | `Get-NetFirewallRule -DisplayName "*CC*"` (管理员 PowerShell)     |
| 查看端口转发     | `netsh interface portproxy show all` (管理员 PowerShell)          |
