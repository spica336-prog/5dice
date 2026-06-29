# 五骰占 — 前後端分離版

## 結構
```
project/
├── api/
│   └── divine.js     ← 後端 API（所有占卜邏輯、解籤資料庫都在這裡，使用者看不到）
├── public/
│   └── index.html    ← 前端頁面（UI、動畫、呼叫 API）
└── README.md
```

## 部署到 Vercel

1. 把整個 `project` 資料夾推到一個新的 GitHub repo
2. 到 https://vercel.com 用 GitHub 帳號登入
3. 「Add New Project」→ 選擇這個 repo → Deploy（不需要額外設定，Vercel 會自動辨識 `/api` 資料夾）
4. 部署完成後會拿到一個網址，例如 `https://five-dice-xxxx.vercel.app`

## 部署後必做的 2 件事

1. **打開 `api/divine.js`**，找到 `ALLOWED_ORIGINS`，把裡面的網址改成你剛拿到的 Vercel 網址：
   ```js
   const ALLOWED_ORIGINS = [
     'https://five-dice-xxxx.vercel.app',  // 換成你真正的網址
   ];
   ```
   然後把上面那行 `// return res.status(403)...` 的註解符號 `//` 拿掉,正式啟用 Origin 檢查。
   改完後 `git push`,Vercel 會自動重新部署。

2. **（可選）綁定自訂網域**：例如 `5dice.aconyoco.com`,在 Vercel 專案設定的 Domains 裡新增即可。

## 本機測試

```bash
npm i -g vercel
cd project
vercel dev
```
打開 http://localhost:3000 測試。

## 之後想做得更完整，可以考慮

- 把「每日運勢／每日一骰」也搬到後端（目前因為使用 localStorage 做每日快取，暫時留在前端，資料表是公開的占星基本詞彙，風險較低）
- 加上更嚴謹的限流（目前用記憶體做簡易限流，重啟或多台機器會失效；流量大時建議改用 Upstash Redis）
- 把 `api/divine.js` 用 `terser` 做最後一層混淆（即使搬到後端，仍建議养成壓縮 production code 的習慣）
