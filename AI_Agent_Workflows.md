# 從 Spotify 和 Uber 看 AI Agent 開發工作流程

Spotify 的 Honk 案例聚焦在如何把程式修改交給背景 Agent，並用建置、測試和程式碼審查驗證結果。Uber 的案例聚焦在如何將團隊的工作流程寫成可重用的 Skills，再用實際執行資料和評估持續改善。兩者也帶出討論的核心問題：團隊有哪些工程規範，才能把個人使用 AI 的經驗累積成共用流程？

本文整理自原始對話，文中的案例資料於 2026-11-25 依官方文章與訪談查證。[原始對話](./chatgpt.md) · [官方來源與查證附錄](./AI_Agent_Workflows_Sources.md)

## 原始問題

**問題一｜Spotify 公開過哪些 AI 工作流程？**

> spotify 之前有發表過什麼ai工作流程嗎 印象中好像是有

**問題二｜有工程規範的團隊是否更容易形成共用流程？**

> https://www.uber.com/us/en/blog/optimizing-ubers-agent-skills/ uber也有. 要發展出這種工作流程的是不是都是有在規範coding style的開發team比較會發生 相較於一般沒有規範 style 大家各做各的很難 演化出這種行為

## Spotify｜Honk 如何在背景完成程式修改

Fleet Management 是 Spotify 大規模維護程式碼的做法。工程師用 Fleetshift 確定遷移目標、排程與追蹤進度，由 Honk 在背景修改程式。以資料集遷移為例，團隊用 Backstage 的資料血緣和 Codesearch 找出受影響的儲存庫。工程師也可以從 Slack 發起臨時任務，讓背景 Agent 完成修改並回傳 PR。 [Honk Part 1](https://engineering.atspotify.com/2025/11/spotifys-background-coding-agent-part-1) · [Honk Part 4](https://engineering.atspotify.com/2026/4/background-coding-agents-dataset-migrations-honk-part-4) · [Spotify 工程分享](https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint)

**早期 Judge 與後續 CI 驗證。** Spotify 在 2025 年的 Honk Part 3 記載，Honk 可依驗證結果修正後重跑。當時用 Claude Code 的 `Stop` hook 觸發開 PR 前的驗證，先做格式、建置與測試等檢查，再由 LLM Judge 比對原始指令與程式差異。若驗證未通過，系統會回報錯誤並阻止建立 PR。 [Honk Part 3](https://engineering.atspotify.com/2025/12/feedback-loops-background-coding-agents-part-3)

在 2026-06-29 發布的訪談中，Niklas Gustavsson 說，隨著模型與 Agent 執行環境改進，Honk 已移除早期的 LLM Judge，仍可呼叫 Linux 與 macOS 上的 CI 建置來驗證變更。Spotify 也提到會在安全的情況下自動合併，把人工審查留給需要判斷的變更。 [訪談 09:45](https://www.youtube.com/watch?v=9DHZLw5653E&t=585s) · [Spotify 工程分享](https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint)

Spotify 於 2026-06-03 公布，超過 99% 的工程師每週使用 AI 開發工具，94% 回報生產力提升，PR 頻率增加 76%。文中舉出一次後端 Java 遷移耗時 3 天。超過 250 萬筆則是 Fleet Management 自導入 Agent 前開始累計的自動化維護 PR 合併數。 [Spotify 工程分享](https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint)

## Uber｜如何把團隊流程變成可維護的 Skills

Uber 於 2026-11-23 公布，共用 Skills 市集從 2025 年 10 月的 1 個 Skill，在約一年內成長到超過 5,000 個。其跨平台使用資料**單月記錄超過 100 萬次呼叫**。 [Uber 官方文章](https://www.uber.com/us/en/blog/optimizing-ubers-agent-skills/)

Uber 用 Git 儲存庫管理 Skills 的版本、維護者和審查。團隊先寫 `spec.md`，再由 Skill Workshop 產生 `SKILL.md` 和測試提示。Linter 檢查步驟與錯誤復原說明，Evals 評估 Agent 的產出與執行過程。Agent 遇到問題時可提交結構化的 `papercut` 回報，讓維護者能追查、修正並驗證。 [Uber 官方文章](https://www.uber.com/us/en/blog/optimizing-ubers-agent-skills/)

## 團隊工程規範如何影響 AI 工作流程

Spotify 回報，在其程式碼較一致的環境中，Agent 表現較好。在資料集遷移案例中，資料管線框架的差異也增加了撰寫共用指令的難度。這些是 Spotify 在自身環境的觀察。 [Spotify 工程分享](https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint) · [Honk Part 4](https://engineering.atspotify.com/2026/4/background-coding-agents-dataset-migrations-honk-part-4)

Spotify 在 Fleet Management 開始自動合併變更時就補強了測試，Honk 後來可以沿用這套驗證。Gustavsson 也指出，程式碼、工具與框架越一致，Agent 越容易從同類功能找到可參考的實作。 [訪談 12:01](https://www.youtube.com/watch?v=9DHZLw5653E&t=721s) · [訪談 19:20](https://www.youtube.com/watch?v=9DHZLw5653E&t=1160s)

原對話據此討論團隊如何建立可重用的 AI 工作流程。Coding Style 先處理命名、格式和註解。共用 Skill 還需要知道專案如何分層、該使用哪套元件、如何建置與測試，以及什麼結果才符合交付條件。

| 面向 | 有共用工程規範時 | 主要依個人做法時 |
| --- | --- | --- |
| 程式碼一致性 | 可依專案規範驗證 | Agent 產出的做法可能更加分散 |
| 工程知識與交接 | 可透過文件、範例和測試累積 | 較依賴原開發者或個人提示詞 |
| Skills 與自動化 | 相同流程較容易重用、擴展及評估 | 經常需要為不同專案重新補充規則與驗收條件 |
| 失敗經驗與維護 | 可將錯誤回饋至共用 Skill 或測試 | 經常由當事人修正，相同錯誤可能重複發生 |
| 生產力 | 個人提速，成果也有機會累積到團隊 | 個人同樣可能提速，團隊較難承接 |

這是原對話對團隊可能發展方向的推演。不同專案可以採用不同的技術選型，只要各自的規則明確。規範仍在建立中的團隊也可以先利用 AI 盤點現有做法，逐步補上規範和測試。

### .NET 範例｜把新增 API 寫成共用 Skill

假設三位工程師分別用 EF Core、Dapper 和 ADO.NET，分層方式也各有不同，例如三層式、Clean Architecture 或直接在 Controller 實作。他們的錯誤處理、命名與測試習慣也不一致。此時共用的 `create-web-api` Skill 會缺少足夠資訊來判斷應採用的資料存取方式、錯誤處理、API 回傳格式和驗收方式。

若某個專案已經明確採用下列規範，`create-web-api` 就可以直接依這些條件執行和驗證。

| 項目 | 範例規範 |
| --- | --- |
| 執行環境與架構 | .NET 10，Controller → Service → Repository |
| 資料存取與 API 回傳 | Dapper，統一使用 `ApiResponse` |
| 例外處理與日誌 | Global Exception Middleware，Serilog |
| 命名與註解 | Microsoft C# Coding Conventions，繁體中文 XML Summary |
| 驗證 | Build + Unit Test |

共用的 AI 開發資源可以依用途放在同一個儲存庫。

```text
AGENTS.md                     團隊通用開發規範
.editorconfig                 程式碼格式規範
skills/
├── create-web-api/          新增 RESTful API
├── create-service/          新增 Service
├── create-sql-migration/    資料庫異動
├── code-review/             程式碼審查
├── generate-unit-test/      產生單元測試
├── fix-build-error/          修復編譯錯誤
└── deploy-iis/              IIS 部署
```

對應到 Codex App 或 Codex CLI，可以由 `AGENTS.md` 和架構文件提供 Context，以 Skills 記錄可重用步驟，再用 `dotnet build`、`dotnet test` 和 GitHub Actions 建立回饋。Codex 背景任務可承接非同步工作，自訂工具以及 Hooks 可作為 Agent 執行環境的一部分。需要分工時可使用 Subagents，交付前則用 Git Diff 或 Pull Request 做人工審查。

### 原對話的六階段成熟度框架

下表是原回答用來討論工程標準化的自訂框架。

| 階段 | 團隊具備的能力 |
| --- | --- |
| Level 0｜個人化開發 | 工程師依各自的做法開發 |
| Level 1｜Coding Standard | 共用命名、格式、註解等規範 |
| Level 2｜Architecture Standard | 定義架構、設計模式和專案結構 |
| Level 3｜Workflow Standard | 建立共用的開發、測試、審查和部署流程 |
| Level 4｜Agentic Workflow | 用 Context、Harness 和 Skills 支援 Agent 執行任務 |
| Level 5｜Self-improving Workflow | 以 Evals 和執行結果持續改善 Skills |

依這個框架，Spotify 與 Uber 公開案例中的部分流程呈現 Level 4 和 Level 5 的特徵。團隊採購 AI 工具後，仍需要把架構、業務邏輯、操作步驟和驗收條件寫成同事與 Agent 都能使用的資產。當模型或工具更換時，這些規範、Skills 和測試案例仍然可以沿用。
