# trading-analyzer

台股交易訊號的回測與驗證服務,用 .NET 10 開發。

## 這個專案在解決什麼

自建的交易系統每天會偵測盤中訊號(「拉起來」「殺下去」「拉不動」「殺不下去」),寫進 jsonl 紀錄但不下單,理由是「先量化再自動化」。量化這一段就是這個專案:

**讀歷史 K 線與訊號紀錄,算出每種訊號後續的報酬分布、勝率與期望值,判斷哪個訊號值得自動化。**

## 邊界

**本服務只讀不寫,不接券商 API,不具備下單能力。**下單由 [stock-trading-program](../stock-trading-program) 負責,兩者透過檔案與事件溝通,互不干涉。

## 資料來源

| 來源 | 內容 |
|---|---|
| `stock-candles-*.jsonl` | 個股分K(時間、開高低收、量) |
| `signals-*.jsonl` | 盤中偵測到的訊號 |

資料檔不進版控。路徑用環境變數設定,見 `.env.example`。

## 技術

- .NET 10 / ASP.NET Core Web API
- EF Core + SQL Server(用 Docker 跑)
- xUnit

## 開發進度

- [ ] 回測引擎:讀分K,跑均線交叉策略
- [ ] 領域模型:依 DDD 設計 Portfolio、Order、Money
- [ ] 訊號驗證:統計各類訊號的後續報酬
- [ ] 拆成微服務,用訊息佇列傳事件
- [ ] 容器化,以 IaC 部署
- [ ] MCP Server:讓 AI 能查詢回測結果
