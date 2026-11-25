# Spotify 與 Uber 官方來源查證附錄

查證日期為 2026-11-25。[返回主文](./AI_Agent_Workflows.md)

## 官方文章發表日期

| 發表日期 | 文章 | 內容 |
| --- | --- | --- |
| 2025-11-06 | [Spotify Honk Part 1](https://engineering.atspotify.com/2025/11/spotifys-background-coding-agent-part-1) | 背景 Coding Agent 與大規模維護 |
| 2025-11-24 | [Spotify Honk Part 2](https://engineering.atspotify.com/2025/11/context-engineering-background-coding-agents-part-2) | 遷移任務的上下文工程和工具設計 |
| 2025-12-09 | [Spotify Honk Part 3](https://engineering.atspotify.com/2025/12/feedback-loops-background-coding-agents-part-3) | 驗證工具與 LLM Judge |
| 2026-04-22 | [Spotify Honk Part 4](https://engineering.atspotify.com/2026/4/background-coding-agents-dataset-migrations-honk-part-4) | 下游資料集使用者的遷移案例 |
| 2026-06-03 | [Spotify 的 AI 開發經驗與工程標準化](https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint) | 工具採用與 PR 數據、Fleetshift、Honk 和 Chirp |
| 2026-08-10 | [Spotify 的 Xirp 介紹](https://portal.spotify.com/blog/introducing-xirp) | 可搭配不同模型與 Agent 的開發環境 |
| 2026-11-23 | [Uber 的 Agent Skills 評估](https://www.uber.com/us/en/blog/optimizing-ubers-agent-skills/) | Skills 市集規模、評估與維護 |

Spotify 與 Anthropic 的技術對談也可從 Spotify Engineering 的文章觀看。[Spotify × Anthropic 技術對談](https://engineering.atspotify.com/2026/4/anthropic-agentic-development)

## Spotify 數據口徑

以下是 Spotify 於 2026-06-03 披露的公司內部數據和案例。 [Spotify 工程分享](https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint)

| 數字 | 官方資料中的統計對象 |
| --- | --- |
| 超過 99% | Spotify 工程師中每週使用 AI 開發工具的比例 |
| 94% | 回報 AI 使自己更有生產力的工程師比例，屬自我回報 |
| 增加 76% | Spotify 報告的 Pull Request 頻率增幅，文中未列出比較期間 |
| 3 天 | 文中提及的一次跨後端服務 Java 遷移的耗時 |
| 超過 250 萬筆 | Fleet Management 截至發文已合併的自動化維護 PR，涵蓋 Honk 導入前的自動化 |

## Spotify 技術描述

- **Fleetshift 與 Honk。** Fleetshift 協助工程師選定目標、排程並追蹤大規模變更。Honk 負責程式碼修改。2026 年 6 月的分享指出，Honk 使用 Claude Agent SDK 和 Spotify 自製的 Harness，在 Kubernetes Pod 中並行執行，並可用 CI 工具跨作業系統建置。該文也將 Chirp 描述為 Honk v2 的 Agent 編排功能之一，並介紹共用工作階段和團隊專案。 [Spotify 工程分享](https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint)
- **任務入口與上下文。** Honk Part 1 描述從 Slack 或 GitHub Enterprise 發起臨時任務。互動 Agent 蒐集資訊、整理指令，再交給背景 Agent。資料集遷移另有一條流程，團隊用 Backstage 查下游依賴，以 Codesearch 找到目標儲存庫，再用 Fleetshift 編排。Honk Part 2 說明了當時如何事先整理指令並提供受限的驗證與 Git 工具。 [Honk Part 1](https://engineering.atspotify.com/2025/11/spotifys-background-coding-agent-part-1) · [Honk Part 2](https://engineering.atspotify.com/2025/11/context-engineering-background-coding-agents-part-2) · [Honk Part 4](https://engineering.atspotify.com/2026/4/background-coding-agents-dataset-migrations-honk-part-4)
- **驗證與 PR。** 2025 年的 Part 3 記載，Claude Code 的 `Stop` hook 觸發開 PR 前的驗證。流程先跑格式、建置及測試等確定性檢查，再用 LLM Judge 比對指令與程式差異。驗證失敗會回報錯誤並阻止開 PR。當時讓 Agent 回應 GitHub PR 的 CI 檢查仍是後續整合方向。2026 年 6 月 3 日的分享說明了 Honk 如何使用 CI 環境的建置工具，以及 Spotify 如何在安全的情況下自動合併 PR。6 月 29 日發布的訪談另確認早期的 LLM Judge 已移除，Honk 仍可呼叫 CI 建置。 [Honk Part 3](https://engineering.atspotify.com/2025/12/feedback-loops-background-coding-agents-part-3) · [Spotify 工程分享](https://engineering.atspotify.com/2026/6/code-with-claude-coding-is-no-longer-the-constraint) · [訪談 09:45](https://www.youtube.com/watch?v=9DHZLw5653E&t=585s)
- **Xirp 與 Portal。** Xirp 是可搭配不同模型與 Coding Agent 的開發環境，支援平行工作階段與跨工作階段保留上下文。連接 Portal 後，可從軟體目錄取得元件架構、依賴、負責團隊和架構決策等背景資訊。 [Xirp 官方介紹](https://portal.spotify.com/blog/introducing-xirp)

### Honk 訪談與遷移案例

Claude 官方頻道於 2026-06-29 發布 Niklas Gustavsson 的訪談。 [How Spotify runs agents across 20M+ lines of code](https://www.youtube.com/watch?v=9DHZLw5653E)

| 影片時間 | 訪談重點 |
| --- | --- |
| [03:09](https://www.youtube.com/watch?v=9DHZLw5653E&t=189s) | Gustavsson 個人使用 tmux 與 Git worktrees 並行開啟 Claude Code 工作階段。他提到超過 2,000 萬行程式碼的是 Spotify 的後端 monorepo。 |
| [09:45](https://www.youtube.com/watch?v=9DHZLw5653E&t=585s) | Honk 使用 Claude Agent SDK，在 Kubernetes Pod 執行，並可呼叫 Linux 與 macOS 的 CI 建置。隨模型與執行環境改進，團隊移除了早期的 LLM Judge。 |
| [12:01](https://www.youtube.com/watch?v=9DHZLw5653E&t=721s) | 各元件有明確的負責團隊。Spotify 在 Fleet Management 開始自動合併變更時補強測試，後續 Agent 可以沿用這套驗證。 |
| [19:20](https://www.youtube.com/watch?v=9DHZLw5653E&t=1160s) | Gustavsson 以 Spotify 的經驗說明，程式碼、工具與框架越一致，Agent 越容易找到可參考的既有實作。 |

- **Java 遷移提示詞。** Spotify 工程師公開了 AutoValue 遷移至 Java records 的實際提示詞，其中包含改動範圍、改寫前後範例及通過測試的要求。 [Honk Part 2](https://engineering.atspotify.com/2025/11/context-engineering-background-coding-agents-part-2) · [完整遷移提示詞](https://gist.github.com/mbruggmann/5ca649914f58ea935f42ffd01c1343ee)
- **資料集遷移。** Scio 管線在團隊間的差異讓共用指令難以涵蓋各種情況，Spotify 當時暫停嘗試自動遷移 Scio。針對較一致的 dbt 和 BigQuery Runner，團隊補齊明確的欄位對照，最後推出 240 筆自動遷移 PR。由於這些儲存庫普遍缺少建置時的單元測試，合併前仍由負責團隊手動驗證。 [Honk Part 4](https://engineering.atspotify.com/2026/4/background-coding-agents-dataset-migrations-honk-part-4)

## Uber 數據口徑與技術描述

- **市集規模與使用量。** 共用 Skills 市集從 2025 年 10 月的 1 個 Skill，在約一年內增至超過 5,000 個。圖 2 的市集數量來自 Git 歷史。文章記載單月超過 100 萬次呼叫，圖 3 呈現 2026 年 3 月至 10 月的每月記錄，涵蓋 Claude Code、OpenCode、Codex 及 Uber 內部 Agent。 [Uber 官方文章](https://www.uber.com/us/en/blog/optimizing-ubers-agent-skills/)
- **版本與規範。** 共用 Git 儲存庫提供維護者、審查與版本歷史。`spec.md` 記錄目標、產出、工具操作限制及適用的 Agent 和模型。Skill Workshop 依規格產生 `SKILL.md` 與測試提示。Uber 的 linter 會檢查步驟是否有單一目標、可檢查的結果與具體指令，以及 Skill 是否提供錯誤復原說明。 [Uber 官方文章](https://www.uber.com/us/en/blog/optimizing-ubers-agent-skills/)
- **評估與回報。** Uber 會檢查任務產出和執行過程，也會用相同提示重跑有無 Skill 的案例，交由評審盲測比較。Agent 可提交稱為 `papercut` 的結構化問題回報。回報經分類、排查、修正及評估，修正發布後會量測效果並檢查退步。Uber 也會將部分未獲接受的自動維護 PR 納入評估案例。 [Uber 官方文章](https://www.uber.com/us/en/blog/optimizing-ubers-agent-skills/)
