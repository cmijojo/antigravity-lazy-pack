---
name: antigravity-computer-use
description: 在 AntiGravity 連接 Open Computer Use MCP。說「開啟電腦控制」「設定電腦操作」時載入。
---

# 連接 Open Computer Use（AntiGravity 版）

## 步驟

### 1. 安裝
```bash
npm install -g open-computer-use
```

### 2. 授權與權限檢查
因為 macOS 上的隱私與安全限制，必須授權 Accessibility (輔助功能) 與 Screen Recording (螢幕錄影) 權限：
```bash
open-computer-use doctor
```
依照畫面提示，在系統設定中開啟權限。

### 3. 註冊 MCP
設定以 AntiGravity 的 MCP 設定檔為準：
```json
"computer-use": {
  "type": "local",
  "command": ["open-computer-use", "mcp"],
  "enabled": true
}
```

或者使用完整路徑（macOS 預設）：
```json
"computer-use": {
  "command": "/opt/homebrew/bin/open-computer-use",
  "args": ["mcp"]
}
```

### 4. 驗證
重新啟動 AntiGravity，讓 AI 嘗試獲取螢幕狀態或目前開啟的應用程式：
```bash
open-computer-use list-apps
```

⚠️ 安全提醒：啟用電腦控制後，AI 將具有模擬鍵盤滑鼠與讀取螢幕畫面的權限，請確保在安全受信任的專案中開啟。
