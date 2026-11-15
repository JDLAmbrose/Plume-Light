<p align="center">
  <img src="assets/plume_light_logo_horizontal.svg" alt="Plume Light" width="380">
</p>

# 泰易送 · 翼隨行 — Plume Light

Plume Light 是 Cathay Hackathon 的行李服務原型，展示旅客行李申報、費用試算、取送服務、家庭協助、行程進度及面向Cathay的營運看板。預設演示資料以 CX257 香港至倫敦行程為例；訂位、會員、付款、物流及營運資料均為演示資料，不會建立真實訂單或安排實際運送。

## 提交連結

- 程式碼庫：[GitHub — Plume Light](https://github.com/JDLAmbrose/Plume-Light.git)
- 本地原型入口：啟動後開啟 <http://127.0.0.1:8000/>。

## 快速開始

需要 Node.js（建議 24 LTS；啟動腳本亦接受 20.19+ 或 22.12+）及 Python 3.9+。在倉庫根目錄執行：

```sh
npm start
```

啟動腳本會按鎖文件檢查並安裝前端依賴、構建主站，再啟動本地 Python 服務並嘗試開啟瀏覽器。首次安裝依賴需要網絡。也可在 macOS 執行 `sh start.command`，或在 Windows 雙擊 `start.bat`。

- 不自動開啟瀏覽器：`npm start -- --no-open`
- 更改連接埠（macOS / Linux）：`PLUME_PORT=8001 npm start`
- 更改連接埠（PowerShell）：`$env:PLUME_PORT = '8001'; npm start`

請透過本地 HTTP 網址使用網站，不要直接以 `file://` 開啟 HTML。旅客演示狀態保存在目前瀏覽器的本機儲存空間；不同瀏覽器或網址來源不會自動共用資料。

### 前端開發

```sh
npm ci
npm run dev
```

Vite 會顯示本地網址，預設為 <http://localhost:5173/>。若要使用 AI API 或演示查件 API，另開終端啟動 Python 服務：

```sh
python3 tools/serve.py --port 8000
```

Vite 預設將 `/api` 請求轉發至 `http://127.0.0.1:8000`；可用 `PLUME_API_ORIGIN` 指定其他 API 位址。

## 網站功能

| 頁面 | 本地路徑 | 現有功能 |
| --- | --- | --- |
| 行程首頁 | `/` | 進入預約流程或查看已建立的演示行程，並瀏覽服務介紹。 |
| 預約流程 | `/pages/prepare.html`、`/pages/information.html`、`/pages/baggage.html`、`/pages/pricing.html`、`/pages/checkout.html` | 查看演示乘機人及票據行李額、填寫取送資料、逐件申報行李、計算演示報價，並模擬付款或里數兌換。 |
| AI 出行助手 | `/pages/assistant.html` | 使用離線規則演示，或設定 OpenAI 後以文字或圖片詢問行李、服務及目的地政策相關問題。 |
| 行程概覽 | `/pages/journey.html` | 查看演示訂單與逐件交接進度；可在演示控制項中模擬事件。查件資料亦由本地演示 API 產生，不是承運商的實際追蹤號碼。 |
| 家庭互聯 | `/pages/family.html` | 以演示資料預覽家庭邀請、成員關係及指定行程的協助權限。 |
| 家庭權益 | `/pages/benefits.html` | 查看演示家庭里程碑及會籍權益。 |
| 營運看板入口 | `/pages/operations.html` | 進入貨艙壓力看板或用戶／會員看板。 |
| 貨艙壓力看板 | `/pages/hold-pressure/index.html` | 以演示航班資料查看貨艙負載、壓力及調度規劃資訊。 |
| 用戶／會員看板 | `/pages/hold-pressure/index.html?board=members` | 以模擬資料查看會員、家庭關係與服務相關指標。 |

預約、付款、家庭身份與權限確認、物流交接和營運數據均屬原型流程。看板是規劃參考，沒有員工登入或正式調度執行；家庭邀請亦不會發送真實郵件或在不同裝置間同步。

## AI 出行助手

AI Agent 的設定、資料處理方式、工具及限制詳見 [AI 助手接入指南](doc/AI_AGENT.md)。目前支援：

- **OpenAI 模式：**透過同源 Python API 使用 OpenAI Chat Completions，預設模型為 `gpt-4.1-mini`。
- **圖片問答：**可提交 JPEG、PNG 或 WebP 圖片；原圖上限為 8 MiB。瀏覽器會縮放及重新編碼後再傳送。圖片只會在使用者提交問題時送出，不會存入伺服器檔案或瀏覽器持久化儲存。
- **只讀工具：**助手可讀取行程、行李、報價、服務指南及海關政策摘要；不會代替使用者修改行李、付款、下單或確認物流。
- **政策限制：**海關資料來自倉庫內的精選政策摘要，不是即時網路查詢，也不是完整法規或正式攜帶許可判定。航空公司及安檢規則需分開確認。
- **離線規則演示：**未設定 API 時仍可使用頁面提供的規則示例；這不是 OpenAI 即時回答。

啟用 OpenAI 時，請在服務端設定 `OPENAI_API_KEY`，或以 `OPENAI_API_KEY_FILE` 指向只包含 API key 的私有檔案，再啟動 `npm start`。例如 macOS / Linux：

```sh
OPENAI_API_KEY_FILE=/path/to/openai-key.txt npm start -- --no-open
```

請勿將 API key 寫入原始碼、提交至版本庫或放入前端設定。API 使用量及額度依 OpenAI 帳戶而定。

## 資料與語言

- 旅客演示狀態儲存在瀏覽器的 `plume-light-multipage-v2`；語言偏好儲存在 `plume-light-language`。
- 貨艙及會員看板使用各自的演示資料；旅客在主站新增的資料不會自動更新會員看板。
- 主站提供繁體中文、英文、法文、西班牙文及阿拉伯文介面。

## 測試與構建

在倉庫根目錄執行：

```sh
npm test
npm run lint
npm run build
python3 -m unittest discover -s tools/tests -p 'test_*.py'
```

`npm run build` 會檢查 TypeScript、構建主站，並將現有看板靜態檔案複製至 `dist/`。看板源碼位於 `kanban-app/`，可在該目錄安裝依賴並執行 `npm run dev` 或 `npm run build`。

## 專案文件

- [AI 助手接入指南](doc/AI_AGENT.md)
- [產品需求與演示設計](doc/Plume_Light_PRD.md)
- [貨艙壓力看板需求](kanban/Kanban_Requirements.md)
- [會員看板需求](kanban/Member_Kanban_Requirements.md)
- [授權條款](LICENSE)
