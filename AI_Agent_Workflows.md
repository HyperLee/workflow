# 從 Spotify 和 Uber 看 AI Agent 開發工作流程

Spotify 的 Honk 案例聚焦在如何把程式修改交給背景 Agent，並用建置、測試和程式碼審查驗證結果。Uber 的案例聚焦在如何將團隊的工作流程寫成可重用的 Skills，再用實際執行資料和評估持續改善。兩者也帶出討論的核心問題：團隊有哪些工程規範，才能把個人使用 AI 的經驗累積成共用流程？

本文整理自 [chatgpt.md](./chatgpt.md)。發布日期、數據及產品描述沿用原對話的引用，尚未逐項查證。

## 原始問題

**問題一｜Spotify 公開過哪些 AI 工作流程？**

> spotify 之前有發表過什麼ai工作流程嗎 印象中好像是有

**問題二｜有工程規範的團隊是否更容易形成共用流程？**

> https://www.uber.com/us/en/blog/optimizing-ubers-agent-skills/ uber也有. 要發展出這種工作流程的是不是都是有在規範coding style的開發team比較會發生 相較於一般沒有規範 style 大家各做各的很難 演化出這種行為

## Spotify｜Honk 如何在背景完成程式修改

原對話以 Honk 為主軸，整理了 Spotify 的幾項工程工具及用途。

| 工具 | 原對話整理的用途 |
| --- | --- |
| Backstage | 提供專案、系統架構及依賴關係等背景資訊 |
| Fleet Management | 管理大規模軟體維護和自動化變更 |
| Fleetshift | 分派任務、排程並追蹤進度 |
| Honk | 在背景執行程式開發與維護任務 |
| Chirp | 協調多個 Agent 與團隊協作 |
| Xirp | 提供可搭配不同 AI 廠商的 Agent 開發環境 |

原對話將公開案例簡化為以下工作流程。

1. **提出任務。** 工程師發起功能開發、修正缺陷或系統遷移等任務，Slack 可作為入口之一。
2. **提供背景與分派工作。** 透過 Backstage 取得專案資訊，再由 Fleetshift 分派及追蹤任務。
3. **在背景執行。** Honk 使用 Claude Agent SDK 與 Spotify 自製的 Harness，在 Kubernetes 環境中分析程式碼、理解需求、修改程式並執行工具。工程師可以同時交辦多個任務。
4. **用驗證結果回饋 Agent。** 執行 Build、Lint 和 Test。若驗證失敗，Agent 根據結果修正並重跑。
5. **送交審查與交付。** 建立 Pull Request，依任務進入人工 Code Review，符合條件時可自動合併，之後接續部署流程。

原對話引述 Spotify 在 2026 年 6 月 3 日分享的數據，並另外提到 Fleet Management 的累積規模。

| 指標 | 原對話引述 |
| --- | --- |
| 每週使用 AI 開發工具的工程師 | 99%+ |
| 認為 AI 提高生產力的工程師 | 94% |
| Pull Request 頻率增幅 | 76% |
| 一次後端 Java 遷移耗時 | 3 天 |
| Fleet Management 累計合併的自動化維護 PR | 超過 250 萬筆，包含傳統自動化 |

### Spotify 文章與延伸資料

| 日期 | 主題 |
| --- | --- |
| 2025-11-06 | [Honk Part 1｜背景 AI Coding Agent](https://www.engineering.atspotify.com/2025/11/spotifys-background-coding-agent-part-1) |
| 2025-11-24 | Honk Part 2｜Context Engineering（上下文工程） |
| 2025-12-09 | Honk Part 3｜Feedback Loop（回饋迴圈） |
| 2026-04-22 | Honk Part 4｜大型資料集遷移 |
| 2026-06-03 | [Coding Is No Longer the Constraint](https://engineering.atspotify.com/2026/06/code-with-claude-coding-is-no-longer-the-constraint) |
| 2026-08-10 | [Xirp｜擴展 AI Coding Agent 的開發環境](https://portal.spotify.com/blog/introducing-xirp) |

Part 2 和 Part 3 分別回答「Agent 開工前需要哪些專案資訊」和「團隊如何驗證 Agent 的修改」，是原對話建議優先讀的兩篇。[Spotify Engineering 開發者體驗系列](https://engineering.atspotify.com/tag/developer-experience) · [Spotify × Anthropic 技術研討會](https://www.youtube.com/watch?v=fHDGQHr_agg)

## Uber｜如何把團隊流程變成可維護的 Skills

原對話引用 Uber 於 2026 年 11 月 23 日發表的文章。Uber 自 2025 年 10 月從 1 個 Skill 起步，累積至 5,000+ 個內部 Skills，每月跨平台呼叫超過 100 萬次。[How Uber Evals 1 Million Agent Skill Invocations a Month](https://www.uber.com/us/en/blog/optimizing-ubers-agent-skills/)

在原對話的整理中，Uber 將團隊的工作流程、操作程序與資深員工的經驗寫成 Skills，透過 Git 儲存庫、審查、自動化評估（Evals）和使用數據維護。每個 Skill 的步驟都要說明目標、驗證方式和失敗處理，並指明維護者及如何評估成果是否改善。

Agent 執行時遇到的問題也能回流到評估流程。維護者將失敗案例納入 Evals，據此調整 Skill，再檢查調整後的效果。修正因此可以被團隊重複使用，後續執行同一流程的人也能受益。

## 團隊工程規範如何影響 AI 工作流程

依照原對話的分析，有共同工程規範的團隊通常較容易發展出可重用的 AI 工作流程。Coding Style 先處理命名、格式和註解。共用 Skill 還需要知道專案如何分層、該使用哪套元件、如何建置與測試，以及什麼結果才符合交付條件。

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
