<p align="center">
  <img src="assets/plume_light_logo_horizontal.svg" alt="Plume Light" width="380">
</p>

[繁體中文](#traditional-chinese) · [English](#english-version)

<a id="english-version"></a>
# Plume Light — English Version

Plume Light is a baggage-service prototype created for the Cathay Hackathon. It demonstrates baggage declaration, estimated pricing, pickup and delivery services, family assistance, journey progress, and operational dashboards for Cathay. The default scenario uses flight CX257 from Hong Kong to London. Booking, membership, payment, logistics, and operations data are simulated; the prototype does not create real bookings or arrange real shipments.

## Submission Links

- Codebase: [GitHub — Plume Light](https://github.com/JDLAmbrose/Plume-Light.git)
- Local prototype: After starting the app, open <http://127.0.0.1:8000/>.

## Quick Start

Requirements: Node.js (Node 24 LTS recommended; the startup script also accepts 20.19+ or 22.12+) and Python 3.9+. From the repository root, run:

```sh
npm start
```

The startup script checks the lockfile, installs front-end dependencies when needed, builds the main site, starts the local Python server, and attempts to open a browser. Network access is required for the initial dependency installation. On macOS, you can also run `sh start.command`; on Windows, double-click `start.bat`.

- Do not open a browser automatically: `npm start -- --no-open`
- Change the port (macOS / Linux): `PLUME_PORT=8001 npm start`
- Change the port (PowerShell): `$env:PLUME_PORT = '8001'; npm start`

Open the site over local HTTP; do not open its HTML files directly with `file://`. Demo traveller state is stored in the current browser's local storage and is not automatically shared across browsers or origins.

## Development

### Front-end Development

```sh
npm ci
npm run dev
```

Vite prints the local URL; the default is <http://localhost:5173/>. To use the AI or demo tracking APIs, start the Python service in a separate terminal:

```sh
python3 tools/serve.py --port 8000
```

By default, Vite proxies `/api` requests to `http://127.0.0.1:8000`. Set `PLUME_API_ORIGIN` to use a different API origin.

### Website Features

| Feature | Description |
| --- | --- |
| Trip home | Booking entry, trip summary, and service overview. |
| Booking | Enter pickup and delivery details, declare baggage, estimate prices, and simulate payment. |
| AI travel assistant | Ask about baggage, services, and destination policies using text or images. |
| Journey tracking | View demo orders and baggage handover progress. |
| Family services | Manage demo family invitations, trip permissions, and membership benefits. |
| Operations dashboards | Review cargo-load planning and user or membership metrics. |

These are prototype features only. No real bookings, payments, shipments, or operational dispatch are performed.

## AI Travel Assistant

For configuration, data handling, tools, and limitations, see the [AI Assistant Integration Guide](doc/AI_AGENT.md). The current implementation supports:

- **OpenAI mode:** Uses OpenAI Chat Completions through the same-origin Python API. The default model is `gpt-4.1-mini`.
- **Image questions:** Accepts JPEG, PNG, and WebP images up to 8 MiB each. The browser resizes and re-encodes images before sending them. Images are sent only when the user submits a question and are not saved to server files or persistent browser storage.
- **Read-only tools:** The assistant can read trip, baggage, pricing, service-guide, and customs-policy information. It does not modify baggage, make payments, place orders, or confirm logistics on the user's behalf.
- **Policy limitations:** Customs answers use curated policy summaries stored in the repository; they are not live web searches, complete legal texts, or formal permissions to carry an item. Airline and security rules must be checked separately.
- **Offline rules demo:** Example rule-based responses remain available without an API key; they are not live OpenAI responses.

To enable OpenAI, set `OPENAI_API_KEY` on the server or set `OPENAI_API_KEY_FILE` to a private file containing only the API key, then start the app. For example, on macOS / Linux:

```sh
OPENAI_API_KEY_FILE=/path/to/openai-key.txt npm start -- --no-open
```

Do not put the API key in source code, commit it to the repository, or expose it in front-end settings. API usage and quotas are subject to the OpenAI account.

## Data and Languages

- Traveller demo state is stored in the browser under `plume-light-multipage-v2`; the language preference is stored under `plume-light-language`.
- The cargo and membership dashboards use their own demo data. New traveller data in the main site does not automatically update the membership dashboard.
- The main site supports Traditional Chinese, English, French, Spanish, and Arabic.

## Tests and Build

Run these commands from the repository root:

```sh
npm test
npm run lint
npm run build
python3 -m unittest discover -s tools/tests -p 'test_*.py'
```

`npm run build` checks TypeScript, builds the main site, and copies the existing static dashboard files into `dist/`. Dashboard source code is in `kanban-app/`; install its dependencies there to run `npm run dev` or `npm run build`.

## Project Documentation

- [AI Assistant Integration Guide](doc/AI_AGENT.md)
- [Product Requirements and Demo Design](doc/Plume_Light_PRD.md)
- [Cargo Pressure Dashboard Requirements](kanban/Kanban_Requirements.md)
- [Membership Dashboard Requirements](kanban/Member_Kanban_Requirements.md)
- [License](LICENSE)

<a id="traditional-chinese"></a>
# 泰易送 · 翼隨行 — Plume Light

Plume Light 是 Cathay Hackathon 的行李服務原型，展示旅客行李申報、費用試算、取送服務、家庭協助、行程進度及面向 Cathay 的營運看板。預設演示資料以 CX257 香港至倫敦行程為例；訂位、會員、付款、物流及營運資料均為演示資料，不會建立真實訂單或安排實際運送。

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

## 技術簡介
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

### 網站功能

| 功能 | 簡介 |
| --- | --- |
| 行程首頁 | 預約入口、行程摘要及服務介紹。 |
| 預約流程 | 填寫取送資料、申報行李、試算報價及模擬付款。 |
| AI 出行助手 | 以文字或圖片詢問行李、服務及目的地政策。 |
| 行程追蹤 | 查看演示訂單及行李交接進度。 |
| 家庭服務 | 管理演示家庭邀請、行程權限及會員權益。 |
| 營運看板 | 查看貨艙負載規劃及用戶／會員指標。 |

以上均為原型演示功能，不會建立真實訂單或執行付款、物流及營運調度。

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
