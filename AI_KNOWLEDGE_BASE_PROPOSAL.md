# 企業 AI 知識庫系統建設方案書

> **適用場景：** 50 名員工 × 1,500 名客戶 × VIP/非VIP 分群管理  
> **撰寫日期：** 2026 年 4 月  
> **語言：** 全程使用白話文，避免術語

---

## 目錄

1. [為什麼要建這個系統？](#1-為什麼要建這個系統)
2. [系統能做到什麼？](#2-系統能做到什麼)
3. [五大方案比較](#3-五大方案比較)
4. [推薦組合：Dify + LLM Wiki 雙軌並行](#4-推薦組合dify--llm-wiki-雙軌並行)
5. [整合現有 JCK Web System](#5-整合現有-jck-web-system)
6. [自定義介面是否可行？](#6-自定義介面是否可行)
7. [硬件要求與費用](#7-硬件要求與費用)
8. [推薦方案詳細規劃](#8-推薦方案詳細規劃)
9. [開發時間表](#9-開發時間表)
10. [總預算估算](#10-總預算估算)
11. [風險與注意事項](#11-風險與注意事項)
12. [最終建議](#12-最終建議)
13. [實作細節補充（2026年4月）](#13-實作細節補充2026年4月)

---

## 1. 為什麼要建這個系統？

### 現在的問題

| 現況問題 | 造成的影響 |
|---------|----------|
| 員工要翻舊 Email 找資料 | 浪費時間，可能找不到 |
| 客戶資料散落各處 | 服務不一致，VIP 客戶體驗差 |
| 新員工上手慢 | 需要花大量時間培訓 |
| 文件靠人腦記憶 | 員工離職帶走知識 |
| Com Sec 資料難以查詢 | 合規風險增加 |

### 建了系統後

- **員工問 AI**，即時取得答案，不用翻文件
- **客戶資料統一管理**，VIP 客戶自動獲得優先處理
- **AI 自動整理**會議記錄、Email 重點
- **知識永久保存**，不隨員工離職消失

---

## 2. 系統能做到什麼？

```
員工 → 問問題 → AI 搜尋知識庫 → 給答案
                     ↑
              (Email + 會議記錄 + 
               客戶資料 + Com Sec文件)
```

**具體功能清單：**

- [ ] 用中文/英文問問題，AI 即時回答
- [ ] 查詢客戶過往往來記錄
- [ ] 搜尋 Com Sec / 公司秘書相關法規
- [ ] 自動整理會議記錄成重點
- [ ] VIP 客戶資料與一般客戶完全分開
- [ ] 所有資料留在公司伺服器，不上傳外部雲端

---

## 3. 五大方案比較

### 3.1 方案速覽表

| | **方案 A**<br>Dify | **方案 B**<br>AnythingLLM | **方案 C**<br>FastGPT | **方案 D**<br>LLM Wiki | **方案 E**<br>自行開發 |
|--|:--:|:--:|:--:|:--:|:--:|
| **難度** | ★★★ | ★★ | ★★ | ★★★ | ★★★★★ |
| **功能豐富度** | ★★★★★ | ★★★ | ★★★★ | ★★（後台工具） | 按需決定 |
| **中文支援** | ★★★★ | ★★★ | ★★★★★ | ★★★★ | ★★★★★ |
| **VIP隔離** | 需設定 | ✅ 原生支援 | 需設定 | N/A（後台） | ✅ 完全自控 |
| **多人使用** | ✅ | ✅ | ✅ | ❌ 後台工具 | 需開發 |
| **可自行修改** | 部分 | 部分 | 部分 | ✅ 純 Python | ✅ 完全 |
| **Email 處理** | 需 connector | 需 connector | 需 connector | ✅ **原生強項** | 自行開發 |
| **知識累積** | RAG（每次搜尋） | RAG | RAG | ✅ **編譯累積** | 自行設計 |
| **初始硬件成本** | 中 | 低 | 低 | 極低 | 中 |
| **開發時間** | 2-3 個月 | 1-1.5 個月 | 1.5-2 個月 | 2-4 週 | 6-12 個月 |
| **長期維護難度** | 中 | 低 | 低-中 | 低 | 高 |

---

### 3.2 方案 A — Dify（功能最完整）

**用一句話說明：** 像 AI 版的「辦公室管理系統」，功能最多但設定最複雜。

**優點：**
- 可以設計複雜的工作流程（例如：客戶查詢 → 搜尋資料 → 自動回覆 → 記錄在案）
- 有清晰的網頁介面，非技術人員也能管理知識庫
- 支援多種 AI 模型，可以隨時換更好的模型
- 完整的 API，可以接入現有的 JCK Web System

**缺點：**
- 設定和維護需要技術人員
- VIP 隔離需要額外配置
- 系統比較「重」，需要較多記憶體

**適合情況：** 公司有技術能力，想要最完整的功能。

---

### 3.3 方案 B — AnythingLLM（最容易上手）

**用一句話說明：** 像「即插即用」的 AI 助手，最快可以用起來。

**優點：**
- 一個 Docker 指令就能啟動
- **VIP 隔離是原生功能**，不需要額外設定
- 介面簡單，員工容易學
- 有 Desktop 版本（單機測試用）和 Server 版本（多人用）

**缺點：**
- 功能比 Dify 少，複雜的自動化流程做不到
- 自定義能力有限
- 社群相對較小

**適合情況：** 想快速試用、評估效果，或作為第一步。

---

### 3.4 方案 C — FastGPT（中文場景最佳）

**用一句話說明：** 專為中文文件設計的知識庫系統，處理中英混合文件最好。

**優點：**
- **最適合中英混合文件**（Com Sec 文件、客戶信件）
- 知識庫管理介面直覺易用
- 中文搜尋準確率高
- 有流程設計器（雖然不如 Dify 強大）

**缺點：**
- 主要開發團隊在中國大陸，更新節奏跟隨國內市場
- 英文文件處理略遜於 Dify

**適合情況：** 文件以中文為主，希望中文搜尋效果最好。

---

### 3.5 方案 D — LLM Wiki（後台知識整理引擎）

**用一句話說明：** 不是給員工用的前台系統，而是在後台把 Email、會議記錄自動整理成結構化知識，再餵給 Dify。

> **核心概念：**
> - **RAG（Dify 的方式）：** 員工問問題 → AI 去一堆原始文件裡搜尋 → 找到片段 → 組合回答
> - **LLM Wiki 的方式：** AI 先把所有 Email/會議記錄讀一遍，整理成清晰的 Wiki 頁面 → 員工問問題時查的是已整理好的資料，更準確

**特別適合的資料類型：**
- 📧 大量 Email 往來（客戶溝通歷史）
- 📋 會議記錄（提取決議和行動項目）
- 💬 Messenger 對話（整理成客戶喜好檔案）
- 📁 Com Sec 案例（整理成法規知識庫）

**優點：**
- 知識**越用越豐富**（每次整理都累積，不像 RAG 每次從頭搜）
- 輸出是乾淨的 Markdown，可以直接 ingest 進 Dify
- 純 Python 腳本，你已有能力自行修改
- 搜尋比 RAG 更準確（因為查的是已整理好的摘要，非原始文件）

**缺點：**
- 不是一個可以讓員工直接使用的介面
- 需要定期執行（例如每天凌晨自動跑一次）
- 整理質素取決於 AI 模型能力

**適合情況：** 配合 Dify 一起用，作為資料前處理層。單獨用沒有意義。

---

### 3.6 方案 E — 完全自行開發（最大自由度）

**用一句話說明：** 從零開始建造，想要什麼功能就有什麼功能。

**優點：**
- 100% 按公司需求設計
- 可直接整合現有系統（ABSS、MYOB、JCK Web System）
- 長期不受第三方工具限制

**缺點：**
- **開發時間最長（6-12 個月）**
- 費用最高
- 需要全職開發人員
- 風險最大

**適合情況：** 有充足預算、時間，且需求非常特殊。

---

## 4. 推薦組合：Dify + LLM Wiki 雙軌並行

> **這是整個方案書最重要的一節。**  
> 兩個工具不是競爭關係，而是**分工合作**：LLM Wiki 負責整理，Dify 負責查詢。

### 4.1 分工示意圖

```
【後台：LLM Wiki 自動整理】

每天凌晨自動執行：
  Email → LLM Wiki → 整理成「客戶A 溝通摘要.md」
  會議記錄 → LLM Wiki → 整理成「2026年Q1決議.md」
  Messenger → LLM Wiki → 整理成「客戶B 喜好與投訴.md」
  Com Sec 案例 → LLM Wiki → 整理成「法規Q&A.md」
          ↓
       自動 ingest 進 Dify 知識庫

【前台：員工使用 Dify 查詢】

員工問：「客戶 ABC 上次投訴什麼？」
  → Dify 搜尋「客戶ABC 溝通摘要.md」（已整理好）
  → 即時給出準確答案
```

### 4.2 為什麼這樣做比純 Dify 好？

| 對比 | 純 Dify（RAG） | Dify + LLM Wiki |
|------|:---:|:---:|
| 搜尋對象 | 原始 Email 原文 | 已整理好的摘要 |
| 回答準確度 | 中（可能搜到不相關片段） | 高（搜的是結構化知識） |
| Email 累積效果 | 越多 Email 越難搜 | 越多 Email 知識越豐富 |
| 儲存空間 | 大（存所有原文） | 小（只存摘要） |
| 適合客戶數 | 數百以內 | **1,500 客戶完全沒問題** |

### 4.3 LLM Wiki 的 Python 腳本邏輯（概念）

```python
# 每天凌晨自動執行
# 這個腳本你可以用現有的 Python 能力自行維護

for client in get_new_emails_today():          # 讀取今天的新 Email
    summary = llm.summarize(client.emails)     # AI 整理成摘要
    wiki.update(f"clients/{client.name}.md")  # 更新 Wiki 頁面
    dify.ingest(f"clients/{client.name}.md")  # 自動餵給 Dify

# 結果：Dify 的知識庫每天自動更新，不需要人手操作
```

### 4.4 知識庫結構（LLM Wiki 輸出格式）

```
wiki/
├── clients/              ← 每個客戶一個檔案
│   ├── 客戶A_ABC Ltd.md  ← 溝通歷史、喜好、投訴記錄
│   ├── 客戶B_XYZ Co.md
│   └── ...（1,500 個）
├── comsec/               ← 公司秘書知識
│   ├── 週年申報Q&A.md
│   ├── 董事變更程序.md
│   └── ...
├── meetings/             ← 會議記錄整理
│   ├── 2026Q1_決議.md
│   └── ...
└── internal/             ← 內部政策
    ├── VIP客戶服務標準.md
    └── ...
```

---

## 5. 整合現有 JCK Web System

### 5.1 可行嗎？

**完全可行。** Dify 提供完整的 REST API，意思是：

> 你在 JCK Web System 的任何頁面，都可以加一個「問 AI」的按鈕，背後呼叫 Dify，結果直接顯示在你的系統裡。員工根本不需要知道 Dify 的存在。

### 5.2 整合方式（三個層次）

#### 層次 1：最簡單 — 嵌入聊天視窗（1-2 週開發）

```
JCK Web System 頁面
┌─────────────────────────────────────┐
│  客戶列表  │  文件管理  │  報告      │
│                                     │
│  [客戶資料表格...]                   │
│                                ┌──────────┐
│                                │ 🤖 問AI  │
│                                │          │
│                                │ 你：這個  │
│                                │ 客戶上次  │
│                                │ 欠款幾多？│
│                                │          │
│                                │ AI：根據  │
│                                │ 記錄...  │
└────────────────────────────────└──────────┘
```

**技術做法：** Dify 直接提供一段 HTML/JavaScript 嵌入碼，貼入 JCK Web System 即完成。

#### 層次 2：中等 — 按頁面智能查詢（4-6 週開發）

```
員工開啟「客戶 ABC」的頁面
  → JCK Web System 自動把「客戶 ABC」傳給 Dify
  → AI 自動顯示這個客戶的相關摘要、未完成事項
  → 員工不需要自己輸入問題，AI 主動提示
```

#### 層次 3：深度整合 — 雙向資料同步（2-3 個月開發）

```
JCK Web System  ←──→  Dify AI 知識庫
       ↑                      ↑
  ABSS/MYOB 資料          LLM Wiki 整理
       ↑                      ↑
   Com Sec DB            Email / 會議記錄

任何一方更新，兩邊同步
```

### 5.3 你現有的 Python 工具如何接入

| 現有工具 | 接入方式 |
|---------|--------|
| MYOB Azure OCR Tool | OCR 輸出的文字 → 自動 ingest 進 Dify |
| CSA Database | 定期 export → LLM Wiki 整理 → Dify |
| Settlement Tools | 結算報告 PDF → OCR → Dify 知識庫 |
| JCK Web System API | Dify API 雙向呼叫 |

---

## 6. 自定義介面是否可行？

### 6.1 Dify 介面可以改到什麼程度？

**Dify 是開源的（修改版 Apache 2.0，附加多租戶 SaaS 及 Logo 限制），前端是 Next.js，理論上可以改任何東西。**

| 改動類型 | 難度 | 說明 |
|---------|:----:|------|
| 換 Logo、公司名稱、顏色 | ★ 極易 | 改設定檔，1 小時完成 |
| 中文化介面 | ★★ 易 | Dify 已內建中文，調整翻譯即可 |
| 隱藏不需要的功能 | ★★ 易 | 修改前端設定 |
| 加入公司特定欄位 | ★★★ 中 | 需改前端程式碼 |
| 完全重做介面外觀 | ★★★★ 難 | 需要前端開發能力（Next.js）|
| 換掉整個前端 | ★★★★★ 極難 | 只用 Dify API，自己寫前端 |

### 6.2 最實際的自定義方案

**推薦做法：不改 Dify 本身，另外寫一個簡單的前端接 Dify API**

```
員工看到的介面（你自己開發，完全自定）
  ↓  呼叫 API
Dify（在後台運行，員工看不到）
  ↓  查詢知識庫
LLM Wiki 整理好的資料
```

**優點：**
- 前端完全按你的需求設計
- Dify 更新不影響你的介面
- 可以做成與 JCK Web System 一模一樣的風格
- 你用 Python 就能寫一個簡單的 Web 介面（Flask/FastAPI）

### 6.3 具體可以自定義的功能例子

```
自定義介面可以做到：
✅ 員工登入後，自動顯示「今天需要跟進的客戶」
✅ 輸入客戶名稱，AI 自動顯示完整客戶檔案
✅ 一鍵生成客戶會議摘要
✅ VIP 客戶頁面有特別標記和優先顯示
✅ 中文介面，完全沒有英文
✅ 手機版介面（員工外出時可用）
✅ 與 JCK Web System 同一個登入帳號（SSO）
```

---

## 7. 硬件要求與費用

### 7.1 概念說明

> **什麼是本地 AI？**  
> 正常用 ChatGPT 是把問題送到美國的電腦去處理，資料會離開公司。  
> **本地 AI** 是把 AI 裝在自己公司的電腦裡，資料永遠不出門。

### 7.2 硬件配置選項

#### 選項 1：入門級（先試再說）
> 適合：先測試效果，用現有電腦

| 組件 | 規格 | 估算價格（HKD） |
|------|------|:---:|
| 現有電腦 | 至少 16GB RAM | 已有 |
| GPU 升級 | RTX 3060 12GB | ~$3,500 |
| 額外 RAM | 升至 32GB | ~$800 |
| SSD | 2TB NVMe | ~$800 |
| **合計** | | **~$5,100** |

**能跑的 AI 模型：** Llama3 8B、Qwen2.5 7B（基本夠用）  
**限制：** 同時使用的人不能太多（約 5-10 人）

---

#### 選項 2：中端伺服器（推薦起步）
> 適合：正式投入使用，支援 20-30 人同時使用

| 組件 | 規格 | 估算價格（HKD） |
|------|------|:---:|
| 工作站主機 | Dell/HP 工作站，32 核心 | ~$15,000 |
| GPU | NVIDIA RTX 4090 24GB | ~$18,000 |
| RAM | 128GB | ~$5,000 |
| SSD | 4TB NVMe | ~$2,000 |
| UPS 不斷電 | 2000VA | ~$3,000 |
| **合計** | | **~$43,000** |

**能跑的 AI 模型：** Qwen2.5 32B（Q8 量化）、Llama3 8B / 14B  
**限制：** 24GB VRAM 無法跑 70B 級模型，最大約 32B-34B  
**支援人數：** 30-40 人同時使用

---

#### 選項 3：Dell Precision 7960（您提及的高端方案）
> 適合：VIP + 非VIP 雙伺服器，完全隔離，長期投資

**VIP 伺服器（Dell 7960）：**

| 組件 | 規格 | 估算價格（HKD） |
|------|------|:---:|
| Dell Precision 7960 | Xeon W 32核 | ~$35,000 |
| GPU | NVIDIA A6000 48GB | ~$55,000 |
| RAM | 256GB ECC | ~$15,000 |
| SSD RAID | 8TB NVMe RAID1 | ~$8,000 |
| UPS | 3000VA | ~$5,000 |
| **小計** | | **~$118,000** |

**非VIP 伺服器（中端機）：**

| 組件 | 規格 | 估算價格（HKD） |
|------|------|:---:|
| 工作站 | 16核心 | ~$8,000 |
| GPU | RTX 4090 24GB | ~$18,000 |
| RAM | 64GB | ~$2,500 |
| SSD | 2TB | ~$800 |
| **小計** | | **~$29,300** |

**選項 3 硬件總計：~$147,300**

**能跑的 AI 模型：** Qwen2.5 72B Q4（~43GB，可放入 A6000 48GB 單卡）、Llama3 70B Q4  
**注意：** 405B 級模型需 200GB+ VRAM，此配置無法運行  
**支援人數：** 50 人完全無壓力，VIP 客戶資料完全實體隔離

---

### 7.3 硬件方案對比

```
          便宜 ◄──────────────────► 貴
          弱   ◄──────────────────► 強

選項 1：  |████░░░░░░░░░░░░░░░░|  $5,100   (測試用)
選項 2：  |████████░░░░░░░░░░░░|  $43,000  (推薦起步)
選項 3：  |████████████████████|  $147,300 (最完整)
```

---

## 8. 推薦方案詳細規劃

### 推薦組合：AnythingLLM + 選項 2 硬件 → 升級至 Dify

**理由：** 風險最低，可以先用低成本驗證效果，確認有用再加大投入。

### 推薦組合：AnythingLLM → Dify + LLM Wiki 雙軌

**理由：** 風險最低，可以先用低成本驗證效果，確認有用再加大投入。

### 第一階段：驗證期（第 1-2 個月）

```
目標：用最少資源，確認 AI 知識庫對公司有用

硬件：選項 1（現有電腦 + RTX 3060）約 $5,100
軟件：AnythingLLM（免費開源）
人力：1 名有基礎 IT 知識的員工兼職管理

驗證問題：
✓ 員工真的會用嗎？
✓ 搜尋 Com Sec 文件準確嗎？
✓ 中英混合問答效果如何？
✓ 速度夠快嗎？
```

### 第二階段：正式部署（第 3-5 個月）

```
目標：全公司 50 人使用，引入 VIP 客戶資料

硬件：升級至選項 2（約 $43,000）
軟件：從 AnythingLLM 遷移至 Dify（或 FastGPT）
人力：外判開發 connector（接 ABSS、MYOB 資料）

新增功能：
✓ 員工用帳號登入，各自有記憶
✓ VIP 客戶資料隔離
✓ 自動 ingest 新 Email 和文件
✓ 接入現有 JCK Web System
```

### 第三階段：優化期（第 6 個月起）

```
目標：按實際使用情況持續改善

可選升級：
- 需要更快 → 加 GPU（選項 3）
- 需要 VIP 完全隔離 → 買第二台伺服器
- 需要更多功能 → 自行開發特定模組
```

---

## 9. 開發時間表

```
月份    工作內容                               負責人
────────────────────────────────────────────────────────
第1月   ▶ 採購硬件（選項1）                   公司
        ▶ 安裝 Ollama + AnythingLLM           IT / 外判
        ▶ 導入第一批文件（Com Sec 法規）       指定員工

第2月   ▶ 員工試用，收集意見                  全體員工
        ▶ 調整設定，改善搜尋準確度             IT
        ▶ 評估：是否值得繼續投入               管理層

        ── 評估點：如決定繼續 ──

第3月   ▶ 採購正式伺服器（選項2）             公司
        ▶ 安裝 Dify 或 FastGPT               外判開發
        ▶ 遷移 AnythingLLM 的知識庫           IT

第4月   ▶ 開發 ABSS/MYOB 資料 connector      外判開發
        ▶ 設定 VIP/非VIP 知識庫分區           外判開發
        ▶ 員工培訓（新介面）                   培訓師

第5月   ▶ 全公司正式上線                       全體
        ▶ 監控系統穩定性                       IT
        ▶ 收集使用數據，報告給管理層           IT

第6月+  ▶ 按需求增加功能                       外判開發
        ▶ 定期更新 AI 模型                     IT
        ▶ 評估是否需要 VIP 獨立伺服器          管理層
```

---

## 10. 總預算估算

### 方案 A：保守起步（驗證先行）

| 項目 | 費用（HKD） |
|------|:---:|
| 硬件（選項 1） | $5,100 |
| 軟件授權（全開源，免費） | $0 |
| 外判設定費 | $8,000 |
| 員工培訓 | $2,000 |
| **第一階段總計** | **$15,100** |
| 第二階段硬件升級（選項2） | $43,000 |
| 外判開發（connector + 整合） | $30,000 - $50,000 |
| **全部完成預計總費用** | **$88,000 - $108,000** |

---

### 方案 B：直接正式部署

| 項目 | 費用（HKD） |
|------|:---:|
| 硬件（選項 2） | $43,000 |
| 軟件授權（全開源，免費） | $0 |
| 外判設定 + 開發 | $50,000 - $80,000 |
| 員工培訓 | $5,000 |
| 第一年維護費 | $12,000 |
| **總計** | **$110,000 - $140,000** |

---

### 方案 C：VIP 雙伺服器完整方案

| 項目 | 費用（HKD） |
|------|:---:|
| 硬件（選項 3，雙伺服器） | $147,300 |
| 軟件授權（全開源，免費） | $0 |
| 外判設定 + 全功能開發 | $80,000 - $120,000 |
| 員工培訓 | $8,000 |
| 第一年維護費 | $20,000 |
| **總計** | **$255,300 - $295,300** |

---

### 每年持續費用（所有方案）

| 項目 | 每年費用（HKD） |
|------|:---:|
| 電費（伺服器 24/7 運行） | $8,000 - $20,000 |
| 維護和更新 | $10,000 - $20,000 |
| 軟件授權 | $0（全開源） |
| **每年合計** | **$18,000 - $40,000** |

> **對比：** 如果用 ChatGPT Enterprise，50 人每年費用約 HKD $150,000 以上，且資料在外部雲端。

---

## 11. 風險與注意事項

### 11.1 技術風險

| 風險 | 可能性 | 解決方法 |
|------|:------:|---------|
| AI 回答不準確 | 中 | 先用小量文件測試，逐步增加 |
| 系統太慢 | 低-中 | 選對 GPU，硬件足夠就不會慢 |
| 資料外洩 | 低 | 本地部署，不連外網，加防火牆 |
| 員工不願意用 | 中 | 從最常用的查詢場景入手，讓員工看到效益 |

### 11.2 重要決策點

**Q：應該用雲端 AI 還是本地 AI？**

| | 雲端 AI（ChatGPT/Claude） | 本地 AI（Ollama） |
|--|:--:|:--:|
| 費用 | 按月付費，越用越貴 | 一次性硬件投資 |
| 速度 | 快 | 取決於硬件 |
| 資料安全 | 資料上傳外部 | 資料留在公司 |
| 模型質素 | 最好 | 略遜（但足夠） |
| 適合你嗎？ | ❌ 客戶資料敏感 | ✅ 推薦 |

**Q：Dify vs AnythingLLM 應該選哪個？**

```
如果你是第一次嘗試 → AnythingLLM（簡單快速）
如果你要長期認真用 → Dify + LLM Wiki（功能最全 + Email整理最好）
如果文件以中文為主 → FastGPT（中文最好）
```

**Q：LLM Wiki 是否必要？**

```
你有 1,500 個客戶 + 多年 Email 記錄 → 強烈建議加入 LLM Wiki
原因：
  - 純 RAG 在大量 Email 裡搜尋容易找到不相關片段
  - LLM Wiki 先把每個客戶的 Email 整理成一頁摘要
  - Dify 搜尋這些摘要，準確度大幅提升
  - 你已有 Python 能力，LLM Wiki 腳本可以自行維護
```

### 11.3 不應做的事

- ❌ 不要一開始就把所有文件全部導入（先測試，確認效果好再增加）
- ❌ 不要跳過驗證期直接買最貴的硬件
- ❌ 不要讓非 IT 人員在沒有指導下自行設定
- ❌ 不要忽略定期備份（AI 系統的資料庫需要備份）

---

## 12. 最終建議

### 給決策者的一頁總結

```
┌─────────────────────────────────────────────┐
│              我們的建議路徑                   │
│                                              │
│  現在          3個月後         6個月後        │
│                                              │
│  試驗         正式部署        按需擴展        │
│  $15,100      +$73,000       +$50,000起      │
│                                              │
│  AnythingLLM  Dify/FastGPT   VIP獨立伺服器   │
│  選項1硬件    選項2硬件       選項3硬件        │
└─────────────────────────────────────────────┘
```

### 核心原則

1. **先小後大** — 用 $15,100 驗證效果，確認有用才加大投入
2. **資料安全第一** — 本地部署，客戶資料不出公司
3. **開源為主** — 主要軟件全部免費，不依賴任何訂閱費
4. **按需增長** — 硬件和功能可以逐步升級，不需要一次到位

### 對比現有選擇

| | 我們的方案 | 買 Microsoft Copilot | 什麼都不做 |
|--|:--:|:--:|:--:|
| 第一年費用 | $88,000 - $108,000 | $120,000+（50人） | $0 |
| 資料安全 | ✅ 留在公司 | ❌ 上傳微軟雲端 | ✅ |
| 可客製化 | ✅ 高度 | ❌ 幾乎不能改 | N/A |
| 長期費用 | 每年 ~$20,000 維護 | 每年 $120,000+ | $0 |
| 效率提升 | 高 | 高 | 無 |

**結論：投資回報期約 18-24 個月**（以省去的人手時間計算）

---

## 附錄：技術詞彙解釋

| 詞彙 | 白話解釋 |
|------|---------|
| **Ollama** | 讓 AI 模型在自己電腦上運行的程式，等於本地版 ChatGPT 引擎 |
| **Dify** | 管理 AI 知識庫的平台，等於 AI 版的辦公室管理系統 |
| **RAG** | AI 回答前先在你的文件裡搜尋，確保答案來自你的資料 |
| **Docker** | 安裝軟件的簡單方法，一個指令就能起動整個系統 |
| **Connector** | 讓不同系統互相連接的程式，例如讓 AI 讀取 ABSS 的資料 |
| **Vector DB** | AI 用來快速搜尋文件的特殊資料庫 |
| **GPU** | 圖形處理器，跑 AI 模型需要這個，越強越快 |
| **VIP 隔離** | VIP 客戶的資料放在獨立的地方，一般員工看不到 |
| **Ingest** | 把文件「餵」給 AI，讓它學習你的資料 |
| **LLM** | 大型語言模型，就是 ChatGPT 這類 AI 的正式名稱 |

---

*本方案書由 GitHub Copilot 協助整理，最終決策請結合公司實際情況評估。*

---

## 13. 實作細節補充（2026年4月）

> 本節記錄 2026 年 4 月深入討論後的架構確認與實作細節。

### 13.1 正式系統架構（已確認）

```
員工
  ↓
JCK Web System（前台，架設中）
  ↓  對話框呼叫 Dify API
Dify（AI 後台）
  ↙                ↘
知識庫（RAG）      Agent 工具
  ↑
LLM Wiki（Markdown 條目，pgvector 向量化）
  ↑
Email 清洗管道（後台，n8n 排程觸發）
```

**關鍵架構決定：**
- **JCK Web System 是唯一前台**，員工不直接接觸 Dify 介面
- **Dify 在後台**，員工只見到 JCK Web System 裡的對話框
- **n8n 不在對話流程中**，只作後台排程/餵料工具

---

### 13.2 n8n 的正確角色

n8n 不是對話中間層，是**後台資料管道**：

| n8n 的用途 | 說明 |
|-----------|------|
| IMAP 輪詢 / Webhook | 監測新 Email 進來 |
| 觸發清洗流程 | 呼叫 Python 清洗腳本 |
| 呼叫 LLM 提取 | 把清洗後的文字送 Ollama/GPT |
| 上傳 Dify 知識庫 | 呼叫 Dify Knowledge API |
| 排程同步 | 每晚定時更新整個知識庫 |

```
新 Email 進來
  → n8n 觸發
  → Python 清洗
  → LLM 提取成 Wiki 條目
  → 上傳 Dify Knowledge Base
  （員工對話時已有最新知識）
```

---

### 13.3 Email 去雜訊：三個開源 Python 套件

**直接 pip install，無需商業服務，資料不離開公司：**

```bash
pip install mail-parser
pip install email_reply_parser
pip install talon
```

| 套件 | 功能 | 需要 LLM？ |
|------|------|:---:|
| `mail-parser` | 解析 .eml，HTML 轉純文字，提取寄件人/主旨 | 否 |
| `email_reply_parser` | 切走引用回覆鏈（`> 原文...`），只留最新段落 | 否 |
| `talon` | 切走簽名塊（姓名/職銜/電話），ML 內建模型 | 否（內建ML） |

**注意：** `talon` 依賴 numpy/scikit-learn，如環境有限制可只用 `email_reply_parser`（純規則，已解決 80% 雜訊）。

**完整清洗流程：**

```python
import mailparser
from email_reply_parser import EmailReplyParser
import talon
talon.init()
from talon import signature

# 1. 解析原始 .eml
mail = mailparser.parse_from_file("incoming.eml")
raw_body = mail.body  # HTML 自動轉純文字

# 2. 切走引用鏈，只留最新回覆
latest_reply = EmailReplyParser.parse_reply(raw_body)

# 3. 切走簽名塊
clean_text, sig = signature.extract(latest_reply, sender=mail.from_[0][1])

# clean_text 已去除：HTML標籤、引用舊郵件、簽名塊、免責聲明
```

---

### 13.4 Email → LLM Wiki 完整流程

**兩步走：去雜訊 → LLM 提取成 Wiki 條目**

```
原始 Email
  ↓
[第一層：三個套件去雜訊]  ← 不需要 LLM，快速便宜
  ↓
[第二層：LLM 提取結構化知識]  ← 需要 LLM，把清洗後文字變 Wiki 條目
  ↓
Markdown Wiki 條目（這才是知識庫的內容）
  ↓
上傳 Dify Knowledge Base
```

**LLM 提取的 System Prompt：**

```
你是知識提取助手。從以下 Email 中提取：
1. 關鍵決定/結論
2. 涉及的公司/客戶名稱
3. 涉及的日期/截止日
4. 行動項目（誰要做什麼）
5. 費用/金額（如有）

忽略問候語、簽名、免責聲明、引用的舊郵件。
輸出格式：Markdown，含 YAML Front Matter。
```

**輸出的 Wiki 條目範例：**

```markdown
---
title: 陳大文有限公司 - 周年申報日期更改
category: clients
client: 陳大文有限公司
date: 2026-04-10
source: email
tags: [周年申報, 日期更改]
---

## 事件摘要
客戶申請將周年申報日期由 6 月改為 9 月。

## 關鍵細節
- 申請日期：2026-04-10
- 負責員工：Mary
- 狀態：待處理

## 行動項目
- [ ] 提交 NAR1 表格至公司註冊處
```

**核心原則：不存原始 Email，只存提取後的知識條目。**
原始 Email 的雜訊會稀釋向量相似度，導致 RAG 找錯文件。

---

### 13.5 LLM Wiki 資料夾結構（標準設計）

```
jck-llm-wiki/
├── clients/                    ← 客戶相關
│   ├── 陳大文有限公司.md        ← 一個客戶一個檔案（累積更新）
│   ├── 永興貿易.md
│   └── _index.md              ← 這個分類的目錄
│
├── procedures/                 ← 作業程序
│   ├── 周年申報流程.md
│   ├── 更改董事程序.md
│   └── 轉讓股份流程.md
│
├── fees/                       ← 費用資料
│   ├── 政府費用一覽.md
│   └── JCK收費標準.md
│
├── emails/                     ← Email 提取的知識條目（按月份）
│   ├── 2026-04/
│   │   ├── 陳大文_周年申報_20260410.md
│   │   └── 永興貿易_更改董事_20260415.md
│   └── 2026-03/
│
├── regulations/                ← 法規知識
│   └── 公司條例重點.md
│
└── README.md                   ← Wiki 總目錄
```

**YAML Front Matter 的作用：** Dify 上傳時可設定 Metadata Filter，按 `client` 或 `tags` 過濾，大幅提升搜尋準確度：

```
員工問：「陳大文公司有什麼待辦？」
→ Dify 先過濾 client = 陳大文有限公司 的條目
→ 再語義搜尋
→ 準確度大幅提升，不會找到其他客戶的資料
```

---

### 13.6 PostgreSQL 在整個系統的角色

整個系統只需**一個 PG 伺服器**，加裝 `pgvector` extension：

```
同一個 PostgreSQL 伺服器（192.168.111.10）
├── jck_db          ← 現有 JCK Web System（客戶/帳單/公司秘書）
├── dify_db         ← Dify 後台（對話記錄/用戶/知識庫 metadata）
└── wiki_vectors    ← LLM Wiki 的 embedding（pgvector）
```

**pgvector 安裝與用法：**

```sql
-- 一次性安裝
CREATE EXTENSION vector;

-- Wiki 條目的向量表
CREATE TABLE wiki_chunks (
    id          SERIAL PRIMARY KEY,
    title       TEXT,
    content     TEXT,
    embedding   vector(1536),   -- 維度取決於 embedding 模型（OpenAI ada-002=1536, nomic-embed-text=768, bge-m3=1024）
    client      TEXT,           -- 用作 Metadata Filter
    category    TEXT,
    source      TEXT,           -- email / manual / procedure
    created_at  TIMESTAMP DEFAULT NOW()
);

-- 語義搜尋：找最相似的 5 條
SELECT title, content
FROM wiki_chunks
WHERE client = '陳大文有限公司'   -- Metadata Filter
ORDER BY embedding <=> $1::vector  -- 向量相似度排序
LIMIT 5;
```

**Dify 已原生支援 pgvector**，在 Dify 設定頁面選擇 pgvector 作向量資料庫即可，不需要另裝 Weaviate/Qdrant。

---

### 13.7 推薦的開源工具組合（配合 Dify）

| 類別 | 工具 | GitHub | 用途 |
|------|------|--------|------|
| **PDF 解析** | MinerU | `opendatalab/MinerU` | 中文 PDF/掃描件解析，支援 OCR + 版面分析 |
| **本地 LLM** | Ollama | `ollama/ollama` | 本地跑模型，Dify 直接支援，零 API 費用 |
| **後台管道** | n8n | `n8n-io/n8n` | Email 監測、排程同步、呼叫 Dify API |
| **監控** | LangFuse | `langfuse/langfuse` | 追蹤每次問答品質、Token 用量（Dify 直接整合） |
| **Email 清洗** | email_reply_parser | `zapier/email-reply-parser` | 切走引用回覆鏈 |
| **簽名識別** | talon | `mailgun/talon` | 切走簽名塊（Mailgun 出品） |

**優先安裝順序：**
1. Dify + Ollama（本地模型，零成本試用）
2. LangFuse（監控回答品質，Dify 5 分鐘整合）
3. n8n + email_reply_parser（Email 自動清洗入庫）
4. MinerU（如需處理 PDF 文件）

---

### 13.8 Karpathy LLM Wiki Vault 架構：三層 Skill 設計（參考 `jason-effi-lab/karpathy-llm-wiki-vault`）

> 此開源 Vault 把知識管理的「靜態程式碼分析」概念引入 Wiki，與 JCK 的場景高度吻合。以下是將其核心設計**適配至 JCK 場景**的版本。

#### 核心概念：兩層目錄、三個操作命令

```
jck-llm-wiki/
├── raw/                     ← 原始資料收件箱（只讀，絕不修改）
│   ├── 01-emails/           ← 從 M365 / IMAP 拉下的原始郵件（.eml / .md）
│   ├── 02-meeting-notes/    ← 會議記錄原文
│   ├── 03-cosec-docs/       ← Com Sec 文件、政府通告
│   ├── 04-fee-schedules/    ← 費用表、報價單
│   └── 09-archive/          ← 已處理後自動移到此處，禁止再讀取
│
├── wiki/                    ← 編譯輸出層（AI 寫入，人員閱讀）
│   ├── index.md             ← 全局索引：所有 wiki 頁面的一句話目錄
│   ├── log.md               ← 操作日誌（只能 append，不能刪改）
│   ├── sources/             ← 原始資料摘要（1 份 raw 對應 1 個摘要頁）
│   ├── entities/            ← 實體層：客戶、公司、負責員工、政府機構
│   ├── concepts/            ← 概念層：作業程序、法規、服務類型
│   └── syntheses/           ← 綜合層：複雜問題的深度分析報告
│
└── CLAUDE.md / AI_RULES.md  ← 全局規範：定義 AI 讀寫權限與 Wiki Schema
```

**`raw/` 是唯一事實來源，任何工具（人或 AI）都不能修改它。**

---

#### 三個操作命令（Skill）

##### `/ingest` — 把原始資料編譯進 Wiki

這就是 n8n 每晚觸發的核心操作。六個步驟，嚴格順序執行：

```
步驟 1：讀取 raw/ 收件箱中的待處理檔案
步驟 2：提煉核心內容（Email 去雜訊 → 主旨/客戶/日期/行動項目）
步驟 3：在 wiki/sources/ 建立來源摘要頁
步驟 4：在 wiki/entities/ 或 wiki/concepts/ 建立/更新知識頁
步驟 5：更新 wiki/index.md（新增條目）和 wiki/log.md（追加日誌）
步驟 6：確認以上全部完成後，把 raw/ 檔案移到 raw/09-archive/（自動歸檔）
```

**衝突處理（重要）：** 如果新資料與已有 Wiki 內容矛盾，**不能靜默覆蓋**。在頁面新增 `## 知識衝突` 區塊，把兩種說法都保留並做對比，等人工確認。

**適配 JCK 的資料分類：**

| `raw/` 資料 | 編譯到 `wiki/` | 類型 |
|------------|---------------|------|
| 客戶郵件 `.eml` | `wiki/entities/陳大文有限公司.md`（累積） | entity |
| 會議記錄 `.md` | `wiki/sources/摘要-會議-20260410.md` | source |
| Com Sec 文件 | `wiki/concepts/周年申報程序.md` | concept |
| 複雜法規分析 | `wiki/syntheses/NAR1-vs-ND2A-比較.md` | synthesis |

---

##### `/query <問題>` — 查詢 Wiki 知識庫

這是 JCK Web System 員工對話框背後的操作邏輯：

```
步驟 1：必須先讀取 wiki/index.md（禁止憑記憶直接回答）
步驟 2：找到相關頁面後，深度讀取完整內容
步驟 3：以 [[wiki雙鏈]] 格式標注引用來源
步驟 4：若回答具有高分析價值，詢問是否存入 wiki/syntheses/
步驟 5：在 wiki/log.md 追加查詢記錄
```

**降級策略：** 若 `wiki/index.md` 中無相關內容：
```
本地知識庫中未找到相關內容，以下為通用知識回答：[直接回答]
```

---

##### `/lint` — 知識庫定期健康體檢

建議每週一次，由 n8n 排程觸發：

```
檢查 1：索引一致性
  wiki/index.md 中有注冊但檔案不存在？
  檔案存在但未在 index.md 注冊？

檢查 2：雙向連結健康
  找出所有「死鏈」（指向不存在頁面的 [[雙鏈]]）
  找出所有「孤島頁面」（從未被任何頁面引用）

檢查 3：知識衝突審查
  掃描所有含「## 知識衝突」區塊的頁面
  列出尚未解決的矛盾

輸出報告：
  ✅ 綠燈項（運行正常）
  ⚠️ 黃燈項（建議處理）
  ❌ 紅燈項（需要即時修復）
```

**硬性規定：** `/lint` 只讀掃描，生成報告前禁止修改任何檔案。確認後才執行修復。

---

#### 全局規範檔案：`AI_RULES.md`（適配 CLAUDE.md）

JCK 版本的 `AI_RULES.md`（放在 wiki 根目錄，給 AI 工具讀取）：

```markdown
# JCK LLM Wiki — AI 操作規範

## 讀寫權限邊界
- raw/       → 絕對只讀。禁止修改或刪除任何原始檔案。
- wiki/      → AI 完全寫入權限。這是 AI 的工作區。
- raw/09-archive/ → 禁止讀取（已處理完畢的歷史存檔）。

## 語言規範
- 使用繁體中文編寫所有 wiki 頁面
- 客戶公司名稱以原文（中/英）為準，不翻譯

## 頁面命名規範
- Entities（客戶/公司/員工）：使用原始名稱，如「陳大文有限公司.md」
- Concepts（程序/法規）：使用描述性名稱，如「周年申報程序.md」
- Sources（摘要）：使用 kebab-case，如「摘要-email-陳大文-20260410.md」

## 強制 Frontmatter
所有 wiki/ 頁面必須包含：
---
title: "頁面標題"
type: entity | concept | source | synthesis
client: 相關客戶名稱（如適用）
tags: [標籤]
source: email | meeting | cosec-doc | manual
last_updated: YYYY-MM-DD
---

## 衝突處理
發現新舊資料矛盾 → 不能靜默覆蓋 → 在頁面加 ## 知識衝突 區塊 → 等待人工確認

## 禁止行為
- 不存原始 Email 全文（只存提取後的知識摘要）
- 不猜測未在 raw/ 中出現的事實
- 不刪除 raw/09-archive/ 以外的任何原始檔案
```

---

#### 與現有設計的對比

| 舊設計（第 13.5 節） | 新設計（Karpathy Vault 架構） |
|---------------------|---------------------------|
| 直接分 `clients/` / `emails/` 等資料夾 | 分 `raw/`（只讀）和 `wiki/`（編譯輸出）兩層 |
| 無操作日誌 | 強制維護 `wiki/log.md`（只可追加） |
| 無全局索引 | 強制維護 `wiki/index.md`（所有頁面目錄） |
| 無健康檢查機制 | `/lint` 定期掃描死鏈/孤島/知識衝突 |
| 無衝突處理協議 | 發現矛盾必須標注，禁止靜默覆蓋 |
| 無頁面類型區分 | 明確區分 entity / concept / source / synthesis |

**建議：** 以 Karpathy Vault 架構替換第 13.5 節的資料夾設計，更嚴謹且可維護。

---

---

### 13.9 硬件選型補充：大模型還是小模型？

> **結論先說：有好硬件就直接跑大模型，沒有理由刻意降級。**

#### 為什麼之前說「小模型做 Ingest」？

之前建議「14B 做 Ingest、32B 做 Query」的前提是：假設 GPU 只有 **24GB VRAM**（如 RTX 4090），只能跑 14B，沒得選。那個建議是資源分配的妥協，不是能力上的判斷。

#### 如果你買了 RTX Pro 6000 Blackwell（96GB VRAM）

```
直接跑 qwen2.5:72b Q4（佔 ~40GB VRAM）
還剩 56GB 空間

→ 一個模型，Ingest + Query + Lint 全包
→ 不用管「哪個任務用哪個模型」
→ 品質最好，架構最簡單
```

#### 96GB VRAM 下的量化選擇

| 量化 | 72B 佔用 | 品質 | 建議 |
|------|---------|------|------|
| Q8_0 | ~77GB | 幾乎無損 | ✅ **首選，完整放入** |
| Q4_K_M | ~43GB | 品質/大小平衡 | 備選 |
| Q3_K_M | ~33GB | 可接受 | 不必要 |

**96GB → 選 Q8_0，一次到位。**

#### 並發壓力時的升級路徑

50 人輪流查詢，Ollama 預設逐一處理（`OLLAMA_NUM_PARALLEL=1`），可能讓第 5 個人等待。雖可調高 `OLLAMA_NUM_PARALLEL`，但其並發機制是複製 context（記憶體倍增），高負載下效率不如 llama-server 的 continuous batching。解法：

```
初期（測試/少量使用）  → Ollama + 72B Q8_0（足夠）
正式上線（50人並發）  → 換 llama-server（見 13.11 節，效率更高）
極高並發（100人+）   → 考慮第二張卡（192GB NVLink）
```

模型不需要換，只是換部署方式。

---

### 13.10 Tool Use：本地模型接外部 API 查詢

> 本地模型沒有的知識，可以即時查詢外部來源，再整合成答案。這個模式叫做 **Tool Use（工具呼叫）**。

#### 運作流程

```
員工問：「ABC Ltd 今年的法定申報期限是什麼？」

本地 72B 模型判斷：
  → 知識庫中無此資料，需要即時查詢
  → 呼叫工具：查詢 CR API（companies.gov.hk）

外部 API 回傳結果
  → 本地模型整合，用自然語言組合成答案
```

#### JCK 可接入的外部來源

| 來源 | 用途 |
|------|------|
| 公司查冊 (CR) API | 公司狀態、董事、申報紀錄 |
| 香港法例 (legislation.gov.hk) | 法規原文查詢 |
| 內部 PostgreSQL | 客戶資料、歷史紀錄 |
| JCK Web System REST API | 現有系統雙向呼叫 |
| 外部模型 API（OpenAI / Claude） | 本地模型信心不足時備援 |

#### 實作方式（Python 中介層）

```python
# 模型輸出 JSON 工具呼叫指令
# {"tool": "cr_search", "company_no": "12345678"}

# Python 攔截 → 呼叫對應 API → 把結果回注 messages
messages.append({"role": "tool", "content": cr_api_result})

# 模型再用這個結果生成最終答案
```

**安全邊界：** 模型不能直接上網，只能呼叫你授權的工具清單。所有外部呼叫經過 Python 中介層，敏感欄位（客戶名稱等）送出前先遮蔽。

#### 接外部大模型（Model Cascade）

本地模型信心不足時，可自動升級至外部 API：

```
本地 72B 嘗試回答
  → 判斷為超出範圍或信心不足
  → 遮蔽敏感欄位
  → 呼叫 OpenAI / Claude API
  → 外部模型回答
  → 本地模型整合 + 過濾後輸出給員工
```

**前提：** Qwen2.5 72B 支援 Function Calling，需以支援格式呼叫 llama-server（`--jinja` 參數）。

---

### 13.11 Windows 並發部署：llama-server

> **Windows 不原生支援 vLLM，但可透過 WSL2 + Docker 取得完整 vLLM 體驗（見 13.14 節）。`llama-server`（llama.cpp 內建）則是最簡部署方案，適合快速上線。**

#### 為什麼不用 Ollama 做並發

Ollama 預設 `OLLAMA_NUM_PARALLEL=1`（逐一處理）。雖可設定大於 1 來啟用並發，但其實作方式是複製 context（例如 4 並發 = 4 倍記憶體），且無 continuous batching，高並發下效率不如 `llama-server`。

#### 安裝方式

從 [llama.cpp Releases](https://github.com/ggml-org/llama.cpp/releases) 下載預編譯 Windows 執行檔（無需 Python 環境）：

```
llama-b????-bin-win-cuda-cu12.x.x-x64.zip  ← NVIDIA GPU 版本
```

解壓後直接執行 `llama-server.exe`。

#### 啟動指令（96GB VRAM，72B Q8_0）

```bat
llama-server.exe ^
  -m qwen2.5-72b-instruct-Q8_0.gguf ^
  -ngl 99 ^
  --parallel 8 ^
  --cont-batching ^
  --jinja ^
  --port 8080
```

| 參數 | 說明 |
|------|------|
| `-ngl 99` | 所有層放 VRAM（GPU 全速） |
| `--parallel 8` | 同時處理 8 個請求 |
| `--cont-batching` | Continuous batching，預設開啟 |
| `--jinja` | 啟用 Function Calling / Tool Use |

#### 與其他方案對比

| | Ollama | llama-server | vLLM |
|--|:--:|:--:|:--:|
| Windows 原生 | ✅ | ✅ | ❌（WSL2/Docker） |
| 真正並發 | ⚠️ 可設定但效率較低 | ✅ | ✅ |
| Continuous batching | ❌ | ✅ | ✅ |
| PagedAttention | ❌ | ❌ | ✅ |
| OpenAI 兼容 API | ✅ | ✅ | ✅ |
| Function Calling | ✅ | ✅ `--jinja` | ✅ |
| 效能 vs vLLM | — | ~80-90% | 100% |
| 設定複雜度 | 極簡 | 簡單 | 中等（WSL2+Docker） |

#### Python 呼叫方式（完全不變）

```python
import openai

client = openai.OpenAI(
    base_url="http://localhost:8080/v1",
    api_key="none"
)

response = client.chat.completions.create(
    model="qwen2.5-72b",
    messages=[{"role": "user", "content": "你好"}]
)
```

llama-server 完全兼容 OpenAI API 格式，現有代碼無需修改。

#### 支援的模型格式

**凡是 GGUF 格式的模型都可以跑。** 主要支援：

| 系列 | 代表模型 |
|------|---------|
| **Qwen** | Qwen2.5 7B / 14B / 32B / 72B ← JCK 首選 |
| **Llama** | Llama 3.1 / 3.2 / 3.3（Meta） |
| **DeepSeek** | DeepSeek-R1（含思維鏈） |
| **Gemma** | Google Gemma 3 |
| **Phi** | Microsoft Phi-4 |

模型可從 Hugging Face 下載，或直接用 `-hf` 參數讓 llama-server 自動下載：

```bat
llama-server.exe -hf bartowski/Qwen2.5-72B-Instruct-GGUF:Q8_0 -ngl 99 --parallel 8
```

---

---

### 13.12 兩條 Ingest 路徑如何匯入同一個 Dify RAG

> **結論：** 路徑 A 和路徑 B 的終點完全相同——都是 Dify Knowledge Base，都走 Dify 的 RAG 搜尋。差異只在「進入 Dify 之前，資料經過了多少整理」。

#### 全局架構圖

```
原始資料來源
├── Email (.eml)          ─────────────────────────────┐
├── 會議記錄 (.docx/.md)  ─── 路徑 B：LLM Wiki 自建  ──┤
├── Com Sec 案例         ─────────────────────────────┘
│                                   ↓
│                         Python 清洗（三個套件）
│                                   ↓
│                         LLM 提取成 Markdown Wiki 頁
│                         wiki/entities/陳大文有限公司.md ← 人類可審閱
│                         wiki/concepts/周年申報程序.md
│                                   ↓
│                         呼叫 Dify Knowledge API 上傳
│                                   ↓
│                    ┌──────────────────────────────────┐
├── 法規文件 (PDF)   ─┤                                  │
├── 費用表 (Excel)   ─┤   Dify Knowledge Base            │
├── 內部政策 (Word)  ─┤   （pgvector 向量資料庫）         │
│   路徑 A：直接上傳 ─┤   所有 chunks 都在這裡            │
└────────────────────┤                                  │
                     └──────────────┬───────────────────┘
                                    │ RAG 搜尋（向量相似度）
                                    ↓
                               員工發問
                                    ↓
                            JCK Web System UI
                                    ↓
                               Dify Workflow
                                    ↓
                           Local LLM（llama-server）
                           ← RAG 找到的 chunks 作為 Context
                                    ↓
                                 答案回傳
```

---

#### 兩條路進入 Dify 後的形態差異

| | 路徑 A（直接上傳） | 路徑 B（LLM Wiki 預處理後上傳） |
|--|:--|:--|
| **Dify 收到的內容** | 原始文件切塊（自動分段） | 整理好的 Wiki 頁面（結構化 Markdown） |
| **一個 chunk 的典型內容** | 某封 Email 的第 2-3 段原文 | 「陳大文有限公司」所有往來的整合摘要 |
| **RAG 搜到的品質** | 可能撈到無關段落 | 直接命中整理好的客戶檔案 |
| **重複資料問題** | 同一件事在多封 Email 出現 → 多個 chunks | 同一件事只在一個 wiki 頁面出現一次 |
| **舊資料會否干擾** | 會（2023 年舊 chunk 和 2026 年新 chunk 並存） | 不會（wiki 頁面累積更新，舊資料被整合） |

---

#### 混用策略（兩條路同時走）

```
同一個 Dify Knowledge Base 裡同時有：

知識庫 A（路徑 A 上傳）：
  ├── 公司條例重點.pdf          ← 法規，不常變，直接上傳
  ├── JCK收費標準2026.xlsx      ← 費用表，一年改幾次
  └── 內部政策手冊.docx         ← 靜態文件

知識庫 B（路徑 B 上傳）：
  ├── wiki/entities/*.md        ← 1,500 個客戶摘要，每天更新
  ├── wiki/emails/2026-04/*.md  ← 本月 Email 提取的知識條目
  └── wiki/concepts/*.md        ← 程序知識（由路徑 B 整理）

員工問：「陳大文公司的周年申報狀況？」
  → Dify RAG 同時搜兩個知識庫
  → 從知識庫 B 找到：陳大文有限公司.md（客戶摘要）
  → 從知識庫 A 找到：周年申報流程.pdf 的相關段落
  → LLM 整合兩者，組合出完整答案
```

---

#### 判斷用哪條路的簡單規則

```
這份資料...

會隨時間累積更新嗎？
├── 是（Email、客戶往來、會議記錄）→ 路徑 B（LLM Wiki）
└── 否（法規、費用表、靜態手冊）  → 路徑 A（Dify 直接上傳）

一個客戶/主題有很多相關文件嗎？
├── 是（一個客戶幾十封 Email）    → 路徑 B（整合成一頁 wiki）
└── 否（一份獨立文件）           → 路徑 A（直接上傳）
```

---

---

### 13.13 路徑 B 上傳後：確保 Dify LLM 讀懂 Wiki 頁面的三個配置

> 路徑 B 的 `.md` 不是原生 Dify Workflow 產生的，但 **Dify 的 RAG 不在乎文件怎麼來**。它只在乎兩件事：chunk 夠不夠清晰、LLM 有沒有被告知「你在讀什麼」。以下是組裝時必須配置的三個地方。

---

#### 配置 1：上傳時的 Chunking 策略（Custom Separator）

Dify 預設按固定字數切塊。Wiki 頁面的 `## 事件摘要` 和 `## 行動項目` 會被切斷在不同 chunk，LLM 拿到的是殘缺段落。

**解法：** 上傳 Wiki 頁面時，在 Dify Knowledge 設定選 **Custom Separator**，填入 `\n## `，讓每個二級標題各自成一個完整 chunk：

```
原始 wiki 頁面：
  ---
  title: 陳大文有限公司 - 周年申報日期更改
  client: 陳大文有限公司
  ---
  ## 事件摘要
  客戶申請將周年申報日期由 6 月改為 9 月。
  ## 行動項目
  - [ ] 提交 NAR1 表格至公司註冊處

切出來的 chunks（正確）：
  Chunk 1：「陳大文有限公司 - 周年申報 ／ 事件摘要 ／ 客戶申請...」
  Chunk 2：「陳大文有限公司 - 周年申報 ／ 行動項目 ／ 提交 NAR1」

若用預設字數切塊（錯誤）：
  Chunk X：「...申請將周年申報日期由 6 月改...」← 無頭無尾，語義殘缺
```

---

#### 配置 2：上傳 API 呼叫時，Frontmatter 分離為 Metadata

YAML frontmatter 不是正文內容，不應進入向量搜尋。把它分離出來作為 Dify 的文件 Metadata，才能做 **Metadata Filter**（先按客戶名縮小範圍，再語義搜尋）。

```python
import yaml, re

def split_frontmatter(md_text):
    """把 YAML frontmatter 和正文分開"""
    match = re.match(r'^---\n(.*?)\n---\n(.*)', md_text, re.DOTALL)
    if match:
        meta = yaml.safe_load(match.group(1))
        body = match.group(2)
    else:
        meta, body = {}, md_text
    return meta, body

meta, body = split_frontmatter(wiki_page_content)

# 上傳到 Dify Knowledge API
dify_api.upload_document(
    dataset_id=DATASET_ID,
    content=body,                         # 只傳正文，不含 frontmatter
    metadata={
        "client":   meta.get("client"),   # 啟用 Metadata Filter
        "category": meta.get("type"),     # entity / concept / source
        "tags":     meta.get("tags"),
    },
    indexing_technique="high_quality",
    process_rule={
        "mode": "custom",
        "rules": {
            "segmentation": {
                "separator": "\n## ",     # 對應配置 1
                "max_tokens": 800
            },
            "pre_processing_rules": [
                {"id": "remove_extra_spaces", "enabled": True},
                {"id": "remove_urls_emails",  "enabled": False}
            ]
        }
    }
)
```

**效果：** 員工問「陳大文公司的事」，Dify 先按 `client = 陳大文有限公司` 過濾，語義搜尋範圍從全庫縮小至單一客戶的 chunks，準確度大幅提升。

---

#### 配置 3：Dify Workflow 的 LLM Node System Prompt

這是最關鍵的一步。System Prompt 告訴 LLM「你拿到的 context 是結構化筆記，不是隨機文章」，LLM 才知道如何解讀各個欄位。

```
你是 JCK 公司秘書服務的 AI 助手。

你會收到從知識庫中找到的相關段落（以 <context> 標注）。
這些段落來自兩種來源：
1. 客戶往來摘要（已整理的結構化筆記，包含「事件摘要」、「行動項目」等欄位）
2. 靜態參考文件（法規、費用表、作業程序）

讀取 <context> 時的規則：
- 「行動項目」欄位（含 [ ] 符號）代表尚未完成的待辦事項
- 「事件摘要」欄位是已確認的事實
- 「知識衝突」欄位代表資料有矛盾，需明確告知員工
- 日期格式為 YYYY-MM-DD
- 如果 context 中沒有相關資料，直接說「知識庫中未找到相關記錄」，不要猜測或編造

回答語言：繁體中文。
引用來源時，標注文件標題，例如「根據《陳大文有限公司》客戶檔案...」。
```

---

#### 三個配置的關係

```
上傳時（一次性設定）：
  Frontmatter → Dify Metadata（供 Metadata Filter 用）
  正文 → 按 "\n## " 切 chunk → 向量化 → pgvector

查詢時（每次觸發）：
  員工問題
    → Dify 先做 Metadata Filter（縮小範圍）
    → 再語義搜尋（找最相關 chunks）
    → chunks 注入 System Prompt 的 <context>
    → LLM 按 System Prompt 規則解讀結構化筆記
    → 組合答案回傳

LLM 讀不讀懂路徑 B 的 Wiki 頁面？
  ✅ 讀得懂——因為：
     1. Chunk 是按 ## 完整切出的有意義段落
     2. System Prompt 解釋了「行動項目」「事件摘要」等欄位的含義
     3. 對 LLM 而言，Wiki Markdown 和原生 Dify Workflow 輸出的文字完全一樣
```

---

### 13.14 效能上頂：WSL2 + vLLM 部署方案

> llama-server 可以快速上線，但若要榨取 RTX Pro 6000 Blackwell 96GB 的全部性能，**WSL2 + vLLM 是正確的升級路線**。以下分析為什麼值得直接上 vLLM，以及實際部署步驟。

---

#### 為什麼 WSL2 + vLLM 不是「將就」，而是「上頂」

##### 1. PagedAttention — 最關鍵的差距

llama-server 的並發模式是固定 slot：`--parallel 8` 代表預留 8 個完整 context window 的 KV cache。72B Q8_0 佔 ~77GB VRAM，剩 ~19GB 給 KV cache，固定分 8 份 = 每 slot 只有 ~2.4GB，能支撐的 context 長度極有限。

vLLM 的 PagedAttention 把 KV cache 像虛擬記憶體一樣分頁管理：
- **動態分配**：用多少佔多少，不用的馬上釋放
- **無碎片浪費**：記憶體利用率接近 100%
- **同樣 19GB 剩餘 VRAM**，可服務更多並發請求、更長 context

```
llama-server（固定 slot）：
  77GB 模型 + [ slot1 2.4GB | slot2 2.4GB | ... | slot8 2.4GB ] = 96GB
  → 每人只有 ~2.4GB KV cache
  → 長對話時快速用完

vLLM（PagedAttention）：
  77GB 模型 + [ 動態分頁池 19GB ]
  → 短對話用 0.5GB，長對話用 5GB，自動調整
  → 同樣 19GB 可服務更多人
```

##### 2. WSL2 GPU 性能幾乎零損耗

WSL2 的 GPU passthrough 不是模擬，而是 CUDA kernel 直接在 GPU 硬件上執行：
- **VRAM**：100% 可用（96GB 全部給 WSL2 內的 vLLM）
- **計算**：~98% 原生效能（差距來自 driver 層的微量開銷）
- **瓶頸不在 GPU**：LLM 推理的瓶頸是 GPU 記憶體頻寬，WSL2 不影響這部分

##### 3. Blackwell 架構原生優勢

RTX Pro 6000 Blackwell 支援硬體 FP8 運算。vLLM 原生支援 FP8 量化：

| 量化方式 | 模型大小 | 剩餘 VRAM 給 KV cache | 來源 |
|---------|---------|---------------------|------|
| GGUF Q8_0 (llama-server) | ~77GB | ~19GB | llama.cpp 社區量化 |
| HuggingFace FP8 (vLLM) | ~72GB | ~24GB | 硬體原生精度 |

FP8 不是「壓縮後的近似」，而是 Blackwell GPU 原生支援的運算精度，精確度損失極小。用 vLLM + FP8 同時獲得：更小模型 + 更多 KV cache 空間 + 硬體加速。

##### 4. vLLM 也支援 GGUF

vLLM 從 v0.5.0 起支援直接載入 GGUF 格式模型（見官方文件 [GGUF Quantization](https://docs.vllm.ai/en/latest/features/quantization/gguf.html)）。這意味着：
- 可以直接使用已下載的 `qwen2.5-72b-instruct-Q8_0.gguf`
- 無需重新下載 HuggingFace 格式
- 遷移時零成本切換

---

#### 實際部署：Docker 一行啟動

##### 前置條件

1. **Windows 11**（WSL2 已內建）
2. **NVIDIA 驅動**：安裝 Windows 版 NVIDIA 驅動（≥ R570 以支援 CUDA 12.8+ / Blackwell）
3. **Docker Desktop**：啟用 WSL2 backend（安裝時預設選項）
4. **.wslconfig 調整記憶體**（`%UserProfile%\.wslconfig`）：

```ini
[wsl2]
memory=32GB          # LLM 推理主要吃 VRAM，系統 RAM 給 32GB 足夠
swap=8GB
localhostForwarding=true
networkingMode=mirrored  # Windows 11 支援，讓 WSL2 port 直接可從 Windows 存取
```

##### Docker 啟動 vLLM（使用現有 GGUF）

```bash
docker run --gpus all \
  -v /mnt/c/Models:/models \
  -p 8000:8000 \
  --ipc=host \
  vllm/vllm-openai:latest \
  --model /models/qwen2.5-72b-instruct-Q8_0.gguf \
  --gpu-memory-utilization 0.95 \
  --max-model-len 8192
```

##### Docker 啟動 vLLM（使用 HuggingFace FP8，推薦）

```bash
docker run --gpus all \
  -v ~/.cache/huggingface:/root/.cache/huggingface \
  -p 8000:8000 \
  --ipc=host \
  vllm/vllm-openai:latest \
  --model Qwen/Qwen2.5-72B-Instruct \
  --quantization fp8 \
  --gpu-memory-utilization 0.95 \
  --max-model-len 8192
```

##### 呼叫方式（與 llama-server 完全相同）

```python
import openai

client = openai.OpenAI(
    base_url="http://localhost:8000/v1",  # port 8000（vLLM 預設）
    api_key="none"
)

response = client.chat.completions.create(
    model="Qwen/Qwen2.5-72B-Instruct",
    messages=[{"role": "user", "content": "你好"}]
)
```

Dify 連接時只需改 `base_url` port 即可，其餘配置不變。

---

#### llama-server vs WSL2 + vLLM：JCK 場景實際對比

| 維度 | llama-server | WSL2 + vLLM |
|------|-------------|-------------|
| **部署難度** | 解壓即用（5 分鐘） | Docker Desktop + 拉 image（30 分鐘） |
| **記憶體管理** | 固定 slot，VRAM 利用率 ~60-80% | PagedAttention，VRAM 利用率 ~95%+ |
| **8 人同時問問題** | 勉強撐住，context 長度受限 | 輕鬆應對，動態分配 KV cache |
| **長對話（8K+ tokens）** | 可能觸發 OOM | 優雅降級，不會崩潰 |
| **量化選項** | 僅 GGUF | GGUF + FP8 + AWQ + GPTQ + INT8 |
| **Speculative Decoding** | 不支援 | 支援（可加速 1.5-2x 生成速度） |
| **維護複雜度** | Windows 原生，零依賴 | 需 Docker Desktop 常駐 |
| **社群 / 更新頻率** | llama.cpp 活躍 | vLLM v0.19.1，77K stars，極活躍 |
| **Dify 官方整合** | 自行配 OpenAI-compatible | [官方文件](https://docs.vllm.ai/en/latest/deployment/frameworks/dify/) 有指引 |
| **故障排除** | 簡單（單一 exe） | 需基本 Docker/Linux 知識 |

---

#### 推薦策略：直接上 vLLM，保留 llama-server 作 fallback

```
部署架構：

  Windows 11 主機
  ├── Docker Desktop（WSL2 backend）
  │   └── vllm/vllm-openai container
  │       ├── Qwen2.5-72B-Instruct FP8  ← 主力推理引擎
  │       └── port 8000 → Dify 連接
  │
  └── llama-server.exe（Windows 原生）
      ├── qwen2.5-72b-instruct-Q8_0.gguf  ← 備用
      └── port 8080 → 緊急 fallback

日常運作：
  Dify → http://localhost:8000/v1  → vLLM（PagedAttention 全速）

Docker 出問題時：
  Dify → http://localhost:8080/v1  → llama-server（即時切換，零停機）
```

**為什麼不純用 llama-server？** 50 人公司，尖峰時段可能 5-15 人同時查詢。PagedAttention 在這個並發量下的 KV cache 利用率差距，直接決定「能不能用」和「用得順不順」。

**為什麼保留 llama-server？** Docker Desktop 更新或 WSL2 異常時（Windows Update 偶發），有原生 exe 可立即頂上，不影響業務。

---

#### WSL2 + vLLM 的已知注意事項

| 項目 | 說明 | 解法 |
|------|------|------|
| WSL2 RAM 預設 50% | `.wslconfig` 預設只分系統記憶體的一半 | 設定 `memory=32GB`（LLM 吃 VRAM，不吃 RAM） |
| 模型檔案位置 | 放 `/mnt/c/` 跨檔案系統讀取較慢 | 首次載入稍慢（~1 分鐘），載入後全在 VRAM，不影響推理速度 |
| Docker Desktop 佔資源 | 常駐約 1-2GB RAM | 對 LLM 伺服器而言可忽略 |
| CUDA 版本 | Blackwell 需要 CUDA ≥ 12.8 | vLLM Docker image 已內建 CUDA 12.9，無需手動安裝 |
| 網路 | WSL2 NAT 模式需 port forwarding | 使用 `networkingMode=mirrored`（Windows 11），port 直通 |

---

---

## 13.15 CSA 資料庫 Text-to-SQL：讓 LLM 即時查詢公司秘書資料

### 核心問題：CSA 結構化資料不應該放進 Knowledge Base

CSA 資料庫有 5,666 間公司、14,004 個人物、40,910 條持股/任職記錄。若將這些資料 embed 成 RAG chunks：

| 問題 | 原因 |
|------|------|
| **語義搜索無法精確匹配** | "A020 有幾個董事" → 搜索 embedding 找不到精確數字，只找到相似文字片段 |
| **無法做聚合查詢** | "本月有幾間 AR 到期" → RAG 沒有 COUNT / DATE range 能力 |
| **跨公司資料污染** | chunk 切塊後，陳大文在 A020 的記錄可能混入 B197 的查詢結果 |
| **資料即時性問題** | 每次更新都需要重新 embed，有 sync 延遲 |

**正確做法：CSA 業務資料用 Tool Calling（Text-to-SQL），不進 Knowledge Base。**

唯一放進 Knowledge Base 的是 **CSA_DATABASE.md**（本手冊），讓 LLM 理解 schema 結構，才能生成正確 SQL。

---

### 架構設計

```
用戶問題（自然語言）
        │
        ▼
  Dify Agent（LLM，Qwen2.5-72B）
        │
        ├─ 問題含具體公司/人名/日期？
        │         │
        │         ▼
        │   呼叫 Tool: query_csa
        │         │
        │         ▼
        │   FastAPI middleware（Python）
        │         ├── 接收自然語言問題
        │         ├── LLM 參考 schema → 生成 T-SQL
        │         ├── 安全檢查（只允許 SELECT，攔截敏感欄位）
        │         ├── pyodbc → localhost\SQLEXPRESS → CSA_Latest
        │         └── 回傳 JSON 結果
        │
        └─ 問題屬程序/法規類？
                  │
                  ▼
            Knowledge Base（RAG）
            procedures/ regulations/ fees/
```

**LLM 自動判斷走哪條路，無需人手設規則。** 判斷依據是 Tool 的 description（見下節）。

---

### 關鍵一：Dify Tool 定義

在 Dify → Tools → Custom Tools 中新增：

```yaml
# OpenAPI schema for CSA query tool
openapi: 3.1.0
info:
  title: CSA Database Query
  version: 1.0.0
paths:
  /query:
    post:
      operationId: query_csa
      summary: >
        查詢 CSA 公司秘書管理系統的即時資料庫，包括：
        公司基本資料（名稱、公司號、成立日期、狀態）、
        現任或歷任董事、公司秘書、股東持股比例、
        Annual Return / AGM / Business Registration 到期日、
        股份類別及已發行股數、公司聯絡人及地址。
        當用戶問及某特定公司（CLIENTID 或公司名）或
        某特定人物（董事、股東、秘書）的具體實時資料時，
        必須呼叫此工具。不適用於查詢程序知識或法規條文。
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                question:
                  type: string
                  description: 用戶的自然語言問題
              required: [question]
      responses:
        '200':
          description: 查詢結果（JSON 陣列）
```

**description 是決策關鍵**：LLM 讀完 description 就知道「有具體公司/人名/日期的問題」要呼叫此 Tool。

---

### 關鍵二：FastAPI Middleware

```python
# csa_query_api.py
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
import pyodbc, openai, json, re

app = FastAPI()

# 禁止輸出的敏感欄位（在 SELECT 清單中攔截）
BLOCKED_COLUMNS = {"EREGPW", "EWEBPW", "ESIGNPW", "USERPW", "IDNUM", "PASSPORT"}

# SQL Server 連線（只讀帳號）
CONN_STR = (
    "DRIVER={ODBC Driver 18 for SQL Server};"
    "SERVER=localhost\\SQLEXPRESS;"
    "DATABASE=CSA_Latest;"
    "Trusted_Connection=yes;"
    "TrustServerCertificate=yes;"
)

# LLM 用於生成 SQL（本機 vLLM / llama-server）
llm = openai.OpenAI(base_url="http://localhost:8000/v1", api_key="none")

# CSA schema 摘要（注入 LLM prompt）
CSA_SCHEMA_SUMMARY = """
CSA_Latest 資料庫核心表（T-SQL，SQL Server 2025）：
- CLIENT(CLIENTID, NAME_1, NAME_C, COMNUM, JURISD, COMSTA, ISACTIVE, INCDATE, DISDATE, LASTAGM, LASTAR, ARSCH, STAFF)
- ENTITY(ENTITYID, SNUM, TYPE, NAME_1, NAME_1C, BIRTHDAY, OCC, NAT)
- MASTER(CLIENTID, MTYPE, ENTITYID, SNUM, SINCEDATE, TILLDATE)
  MTYPE: 11D=Director, 13S=Secretary, 01M=Shareholder, 45S=Significant Controller
- CLISHA(CLIENTID, CLASSID, DES, PAR, XSHARES, ASHARES, ISHARES)
- MEMSHA(CLIENTID, MTYPE, ENTITYID, CLASSID, SHARES)
- ANNRTN(CLIENTID, NEXTARD) — Annual Return 下次到期日
- BUSREG(CLIENTID, BUSDUE) — Business Registration 到期日
- AGMMIN(CLIENTID, AGMDTE) — AGM 日期
- MTPDEF(MTYPE, DES) — 角色代碼對照表
- JURISD(JURISD, DESCRIPT, ISO_2) — 司法管轄區
注意：無 Foreign Key，靠 CLIENTID / ENTITYID / MTYPE 關聯。日期為 datetime2。
只能用 SELECT，禁止 INSERT/UPDATE/DELETE/DROP。
"""


class QueryRequest(BaseModel):
    question: str


def is_safe_sql(sql: str) -> bool:
    """只允許 SELECT，阻擋任何寫入或破壞性操作"""
    sql_upper = sql.strip().upper()
    if not sql_upper.startswith("SELECT"):
        return False
    forbidden = ["INSERT", "UPDATE", "DELETE", "DROP", "TRUNCATE", "EXEC", "EXECUTE", "XP_"]
    return not any(kw in sql_upper for kw in forbidden)


def mask_sensitive_columns(sql: str) -> str:
    """把敏感欄位替換成 NULL AS 欄位名"""
    for col in BLOCKED_COLUMNS:
        # 用正則替換 SELECT 清單中的敏感欄位
        sql = re.sub(
            rf'\b{col}\b',
            f"NULL AS {col}",
            sql,
            flags=re.IGNORECASE
        )
    return sql


def generate_sql(question: str) -> str:
    """呼叫本機 LLM 把自然語言問題轉成 T-SQL"""
    resp = llm.chat.completions.create(
        model="Qwen/Qwen2.5-72B-Instruct",
        messages=[
            {
                "role": "system",
                "content": (
                    f"你是 CSA 資料庫的 SQL 生成助手。\n"
                    f"以下是資料庫 schema：\n{CSA_SCHEMA_SUMMARY}\n"
                    "請根據用戶問題生成一條正確的 T-SQL SELECT 語句。\n"
                    "只輸出 SQL，不要解釋，不要 markdown code block。\n"
                    "TILLDATE IS NULL 代表現任。結果限制 TOP 50 防止過大輸出。"
                )
            },
            {"role": "user", "content": question}
        ],
        temperature=0.0,
        max_tokens=512
    )
    return resp.choices[0].message.content.strip()


@app.post("/query")
def query_csa(req: QueryRequest):
    sql = generate_sql(req.question)

    if not is_safe_sql(sql):
        raise HTTPException(status_code=400, detail=f"生成的 SQL 不安全，已拒絕執行：{sql}")

    sql = mask_sensitive_columns(sql)

    try:
        conn = pyodbc.connect(CONN_STR)
        cursor = conn.cursor()
        cursor.execute(sql)
        columns = [col[0] for col in cursor.description]
        rows = [dict(zip(columns, row)) for row in cursor.fetchall()]
        conn.close()
    except Exception as e:
        raise HTTPException(status_code=500, detail=f"SQL 執行錯誤：{e}\nSQL: {sql}")

    return {"sql": sql, "rows": rows, "count": len(rows)}
```

啟動方式：
```powershell
pip install fastapi uvicorn pyodbc openai
uvicorn csa_query_api:app --host 0.0.0.0 --port 7788
```

Dify Tool 的 Server URL 填 `http://localhost:7788`。

---

### 關鍵三：LLM 需要的 Schema 上下文

LLM 生成 SQL 時需要知道表結構。做法有兩種，**二選一**：

| 做法 | 優點 | 缺點 |
|------|------|------|
| **A. 硬編碼在 middleware System Prompt**（上面 `CSA_SCHEMA_SUMMARY`） | 無需額外配置 | 更新 schema 需改程式碼 |
| **B. 把 CSA_DATABASE.md 放進 Dify Knowledge Base** | Dify 自動 RAG 注入 schema 到 LLM context | 需要 Dify 支援 Tool + Knowledge Base 同時使用 |

**推薦做法 A**：middleware 自帶 schema summary，簡單可靠。CSA_DATABASE.md 可同時放進 Knowledge Base 供一般知識查詢（如"MTYPE 11D 是什麼意思？"），兩者互不干擾。

---

### 安全邊界

| 威脅 | 防禦 |
|------|------|
| **LLM 生成 DELETE/UPDATE** | `is_safe_sql()` — SQL 不以 SELECT 開頭即拒絕 |
| **敏感欄位外洩**（密碼、身份證、護照） | `mask_sensitive_columns()` — 自動替換為 NULL |
| **無限大結果集** | SQL prompt 要求加 `TOP 50` |
| **SQL Injection（LLM 幻覺）** | 用 `pyodbc` parameterized-ready 執行，LLM 生成的 SQL 只走只讀連線 |
| **連線帳號權限** | 建議用 Windows Auth 只讀帳號，或 SQL 帳號只授 `SELECT` 權限 |

建立只讀 SQL 帳號（可選）：
```sql
CREATE LOGIN csa_readonly WITH PASSWORD = 'StrongPassword!';
USE CSA_Latest;
CREATE USER csa_readonly FOR LOGIN csa_readonly;
EXEC sp_addrolemember 'db_datareader', 'csa_readonly';
```

---

### 對話範例

**範例 1：查具體公司**
```
用戶：A020 公司現在有幾個董事？
LLM 判斷：有具體 CLIENTID → 呼叫 query_csa Tool
生成 SQL：
  SELECT TOP 50 e.NAME_1, e.NAME_1C, m.SINCEDATE
  FROM MASTER m
  JOIN ENTITY e ON m.ENTITYID = e.ENTITYID AND m.SNUM = e.SNUM
  WHERE m.CLIENTID = 'A020'
    AND m.MTYPE = '11D'
    AND m.TILLDATE IS NULL
  ORDER BY m.SINCEDATE
回覆：A020 公司現時有 2 位董事：CHAN TAI MAN（陳大文，2018年起）、LEE SIU MING（李小明，2021年起）
```

**範例 2：聚合查詢**
```
用戶：下個月有幾間公司 Annual Return 到期？
生成 SQL：
  SELECT TOP 50 c.CLIENTID, c.NAME_1, a.NEXTARD
  FROM ANNRTN a
  JOIN CLIENT c ON a.CLIENTID = c.CLIENTID
  WHERE a.NEXTARD >= DATEADD(month, DATEDIFF(month, 0, GETDATE()) + 1, 0)
    AND a.NEXTARD < DATEADD(month, DATEDIFF(month, 0, GETDATE()) + 2, 0)
  ORDER BY a.NEXTARD
回覆：下個月共有 12 間公司 Annual Return 到期，最早是 B197 (2026-05-03)...
```

**範例 3：法規問題 → 走 RAG，不呼叫 Tool**
```
用戶：香港私人公司最少要有幾個董事？
LLM 判斷：法規概念，無具體公司 → 查 Knowledge Base（regulations/）
回覆：根據《公司條例》第 79 條，香港私人公司最少須有 1 名董事...
```

**範例 4：混合查詢（同時呼叫 Tool + RAG）**
```
用戶：Annual Return 要提交什麼？A020 幾時到期？
LLM 同時：
  ① RAG → procedures/ 找 AR 程序說明
  ② Tool → 查 ANNRTN WHERE CLIENTID='A020'
回覆：Annual Return 需提交 NAR1 表格...（RAG 結果）
      A020 的 Annual Return 下次到期日為 2026-08-15。（Tool 結果）
```

---

### Dify Agent 配置要求

| 設定項 | 值 |
|--------|-----|
| **應用類型** | Agent（不是普通 Chatbot） |
| **模型** | Qwen2.5-72B-Instruct（需支援 Function Calling） |
| **llama-server 啟動參數** | 需加 `--jinja`（啟用 function calling template） |
| **vLLM 啟動** | 預設支援，無需額外參數 |
| **Tools** | query_csa（Custom Tool，server: localhost:7788） |
| **Knowledge Bases** | procedures/ + regulations/ + fees/ + CSA_DATABASE.md |
| **Max Iterations** | 3（防止 Tool 無限循環） |

llama-server 啟用 Function Calling：
```powershell
.\llama-server.exe `
  -m qwen2.5-72b-instruct-Q8_0.gguf `
  --host 0.0.0.0 --port 8080 `
  -ngl 99 --jinja `
  -c 8192 --parallel 4
```

---

## 13.16 Email LLM Wiki + 企業 RBAC 架構預備設計（開發前準備）

> 本節根據 2026 年 4 月與 AI 系統的深度討論整理，記錄在正式開發 Email LLM Wiki 前必須釐清的架構決策、權限設計原則與安全邊界。所有技術規格已對照官方資料核實。

---

### 13.16.1 核心架構原則：讀寫分離（Ingest/Lint 脫離 Dify）

Email LLM Wiki 的讀寫操作必須嚴格分開，Dify 只負責 Query 一環：

| 職責 | 執行者 | 原因 |
|------|--------|------|
| **Ingest**（新郵件 → Markdown） | Python 守護程序 + 本地 vLLM | Dify Workflow 為同步響應設計，批量長時間任務容易超時崩潰 |
| **Lint**（每夜知識庫整理） | Python 排程腳本 + 本地 vLLM | 需滿載 GPU 算力，在 Dify 內除錯困難 |
| **Query**（員工查詢） | Dify Chatflow + Custom Tool | Dify 擅長調度對話、可視化流程、快速迭代 Prompt |

**Ingest 流程（Python 控制）：**

```
[郵件服務器 IMAP/Exchange]
    → Python Daemon：提取郵件為純文字，剔除 HTML/簽名/免責聲明
    → 呼叫本地 vLLM（Qwen3.6-35B-A3B）：合併入對應客戶 .md 檔案
    → 寫入磁碟（/wiki/clients/<客戶名>.md）
    → （可選）通知 Dify Dataset API 更新向量索引
```

**Lint 流程（每夜 cron）：**
遍歷所有 `.md` 檔案，交給 vLLM 檢查邏輯矛盾、修復格式、標記過時資訊，並更新 `last_linted` 時間戳。

---

### 13.16.2 Dify 作為 Query 框架的正確姿勢

Dify 在此系統中**不使用**傳統切片 RAG，而是透過 Custom Tool 讀取完整 Markdown 文件，充分發揮 Qwen3.6-35B-A3B 的長上下文能力。

**FastAPI 橋接服務（極簡實現）：**

```python
# wiki_api.py — 運行於本地，供 Dify Custom Tool 呼叫
from fastapi import FastAPI
import os

app = FastAPI()
WIKI_BASE = "/data/wiki"

@app.get("/api/read_wiki")
def read_wiki(client_name: str, user_id: str):
    # 權限驗證由 Python 負責（見 13.16.3）
    if not check_permission(user_id, client_name):
        return {"error": "權限不足，無法讀取此客戶資料"}
    path = os.path.join(WIKI_BASE, f"{client_name}.md")
    if not os.path.exists(path):
        return {"content": f"找不到 {client_name} 的資料，可能尚未建立 Wiki。"}
    with open(path, encoding="utf-8") as f:
        return {"content": f.read()}
```

**Dify Custom Tool 描述（餵給 LLM 判斷何時調用）：**

```yaml
name: read_client_wiki
description: |
  當用戶詢問特定客戶的歷史往來、需求變更、項目進度或郵件溝通記錄時，
  調用此工具並輸入客戶名稱，獲取該客戶完整的 Markdown 百科檔案。
  不適用於通用法規查詢或 FAQ 問答。
parameters:
  client_name:
    type: string
    description: 客戶公司名稱（精確匹配）
```

**為何優於傳統 RAG：**
Qwen3.6-35B-A3B 原生支援 262,144 token 上下文（使用 YaRN 擴展可達約 1,010,000 token），可一次讀入整份客戶 Markdown 而不丟失跨時間線關聯，避免切片 RAG 拼接碎片的失真風險。

---

### 13.16.3 Python Middleware RBAC（不讓 LLM 判斷權限）

**核心鐵律：LLM 只判斷意圖，Python 負責守門。**

員工從 Web 前台提問的完整路徑：

```
員工登入 Web → Web 後端驗證 JWT/Session
  → 呼叫 Dify Chat API（附帶 user_id）
  → Dify Chatflow：LLM 判斷意圖 → 呼叫 Custom Tool read_wiki
  → Python API 查 CSA SQL 驗證權限
    → 有權：讀取 .md，返回內容 → LLM 根據內容生成回答
    → 無權：返回拒絕訊息 → LLM 回覆「您沒有權限查詢此客戶」
```

**Python 驗權核心邏輯（概念）：**

```python
def check_permission(user_id: str, client_name: str) -> bool:
    # 1. 從 CSA 查詢員工在職狀態與負責客戶清單
    result = query_csa(
        "SELECT is_active, client_name FROM Staff_Client_Map "
        "WHERE user_id = ? AND client_name = ?",
        (user_id, client_name)
    )
    # 2. 嚴格比對，不在名單或已離職一律拒絕
    return bool(result and result["is_active"])
```

**安全特性：**
- LLM 從未收到它「無法看」的資料，Prompt Injection 無從套取機密
- 員工離職後，CSA 資料庫狀態更新即生效，無需改任何 AI 代碼
- 所有 API 呼叫記錄寫入 Audit Log（`user_id` + 查詢客戶 + 回傳結果 + 時間戳）

---

### 13.16.4 VIP 客戶雙軌物理隔離

單靠代碼邏輯仍有 Bug 風險，VIP 客戶須在**作業系統層**實施物理隔離：

**Linux OS 雙用戶架構：**

```bash
# 創建兩個 OS 用戶，各自擁有獨立目錄
useradd user_general    # 讀寫 /data/wiki_general/  （普通客戶）
useradd user_vip        # 讀寫 /data/wiki_vip/      （VIP 客戶）

# user_general 在 OS 層無法讀取 /data/wiki_vip/
# 即使代碼有 Bug，物理權限仍阻止跨界存取
chmod 700 /data/wiki_vip
chown user_vip:user_vip /data/wiki_vip
```

**雙軌 Pipeline 對照：**

| | 普通軌道 | VIP 專軌 |
|-|---------|---------|
| 執行 OS 用戶 | `user_general` | `user_vip` |
| 郵件來源 | 普通客戶郵箱 | VIP 客戶郵箱（獨立加密存儲） |
| 寫入目錄 | `/data/wiki_general/` | `/data/wiki_vip/` |
| 跨目錄讀取 | ❌ OS 層硬性禁止 | ❌ OS 層硬性禁止 |
| vLLM 實例 | ✅ 共用同一台 GB10（模型無狀態） | ✅ 共用同一台 GB10（模型無狀態） |

> **模型無狀態說明：** vLLM 每次請求完成後立即清空 KV Cache，A 客戶資料不會殘留影響 B 客戶的請求，兩條 Pipeline 可安全共用同一個推理引擎。

**VIP 額外驗證（代碼層）：**

```python
# wiki_api.py — VIP 雙重鎖
if client.is_vip:
    if not check_vip_clearance(user_id):
        return {"error": "需要 VIP 查閱許可（VIP Clearance），請聯繫主管申請"}
    path = f"/data/wiki_vip/{client_name}.md"
else:
    path = f"/data/wiki_general/{client_name}.md"
```

---

### 13.16.5 CSA 作為身份來源 + AI 權限覆蓋層

**CSA 的角色（只讀）：** 提供「這個人是誰、所屬部門、是否在職、負責哪些客戶」。

**新建 `AI_Permissions` 覆蓋層（不改動 CSA 現有結構）：**

| 欄位 | 類型 | 說明 |
|------|------|------|
| `dept_code` | VARCHAR | 部門代碼（對應 CSA 部門） |
| `ai_scope` | VARCHAR | 可查客戶範疇：`own`（只限自己負責）/`dept`（整個部門）/`all` |
| `wiki_tags_allowed` | VARCHAR | 可讀取的知識標籤（如 `cosec,tax,pricing`） |
| `vip_clearance` | BIT | VIP 查閱許可（預設 False，需主管手動開啟） |
| `public_wiki_access` | BIT | 可查公共法規庫（`/wiki_public/`） |

**動態效果：**
- HR 在 CSA 更新員工狀態 → Python 即時讀取最新 → 無需改任何代碼
- VIP Clearance 獨立管理，不依賴 CSA 現有欄位，避免改動遺留系統

---

### 13.16.6 知識庫分層（公私分明）

除機密的客戶私有庫外，設立公共知識庫讓通用知識全員可查：

| 目錄 | 內容 | 存取控制 |
|------|------|---------|
| `/wiki/clients/` | 含真實客戶名稱、金額、往來記錄的完整 Markdown | 嚴格 RBAC（Python SQL 驗權） |
| `/wiki/vip/` | VIP 客戶完整檔案（OS 層物理隔離） | VIP Clearance 雙重鎖 |
| `/wiki/public/` | 去識別化的通用業務見解、法規應對案例 | 全體員工可讀 |

**Ingest 時自動生成公共庫版本（去識別化）：**

Ingest Python Daemon 在寫完私有庫後，額外呼叫 vLLM：

```
System Prompt：
你是資料脫敏助手。根據以下客戶郵件，提取通用業務見解和法規應對經驗。
去除所有客戶名稱、人名、具體金額、日期等可識別資訊。
以匿名案例格式輸出（如「某 BVI 公司轉股案例」），供公司知識庫分享。
```

**效果：** 解決「SQL 守太嚴 LLM 拿不到足夠背景、SQL 放太鬆又有機密外洩風險」的矛盾——通用知識走公共庫，機密細節走私有庫。

---

### 13.16.7 對外聯網查詢的隱私保護（Query Sanitisation）

員工詢問外部法規時，系統須確保客戶名稱**絕對不隨搜尋詞洩露**：

**Dify Workflow 強制脫敏流程（節點設計）：**

```
用戶問題（含客戶名）
  → Node 1：意圖識別 LLM
      輸出：{"intent": "legal_query", "client": "Apex Capital", "topic": "BVI 轉股稅務"}
  → Node 2：脫敏 LLM（System Prompt：提取純知識關鍵字，嚴禁包含真實公司名、人名、金額）
      輸入："Apex Capital BVI 轉股 2026 稅務影響"
      輸出："BVI 公司轉股 2026 稅務法規 影響"（客戶名已清除）
  → Node 3：Tavily/Google 聯網搜尋（只收到脫敏關鍵字）
  → Node 4：read_client_wiki（用原始 client 名稱，在內網查，不外傳）
  → Node 5：融合 LLM（結合外部法規 + 本地客戶檔案 → 生成回答）
```

**Python 硬攔截（最後保障）：**

```python
# 每次呼叫外部 API 前強制過濾
def sanitize_query(query: str) -> str:
    client_names = load_client_names_from_csa()  # 每日從 CSA 同步快取
    for name in client_names:
        if name.lower() in query.lower():
            raise PrivacyViolationError(
                f"搜尋詞包含客戶名稱「{name}」，已攔截，請通報系統管理員"
            )
    return query
```

---

### 13.16.8 縱深防禦（Defense-in-Depth）安全模型

不追求「完美防禦」，而是以多層疊加實現「損害控制」：

| 防線 | 機制 | 攔截效果 |
|------|------|---------|
| **Layer 1**（物理隔離） | Linux OS 用戶權限 + 雙軌 Pipeline | 攔截 99% 跨權限存取 |
| **Layer 2**（代碼守門） | Python SQL 驗權，拒絕後不傳文件給 LLM | LLM 無資料可洩露 |
| **Layer 3**（輸出審查） | 小型模型（如 Qwen3-8B）在回答送出前掃描敏感詞 | 捕捉 Layer 2 漏網之魚 |
| **Layer 4**（威懾） | 全量 Audit Log + 定期人工抽查 | 強化內部人員自我約束 |

**Layer 3 輸出審查 Prompt（概念）：**
```
你是安全審查員。檢查以下即將發送給員工的文字。
若包含具體銀行帳號、身份證號、未公開財務金額，將其替換為 [已隱藏]。
其餘內容保持原文不變。
```

> **重要提醒：** 系統上線初期應進行灰度測試（3–5 人核心小組），收集真實使用案例與潛在翻車場景，修補後再全面推廣。

---

### 13.16.9 Dify 傳統 RAG 的適用場景

以下場景 Dify 內建切片 RAG 才是最佳選擇，不應強行用 LLM Wiki 替代：

| 場景 | 原因 |
|------|------|
| HR 員工手冊、IT FAQ | 問題高度具體，只需一小段答案，切片效率高 |
| 產品技術規格書 | BM25 + 向量混合檢索確保型號精確匹配 |
| 多格式文件批量入庫（PDF/Word/Excel） | Dify 內建 ETL 管道，開箱即用 |
| 高並發對外客服機器人 | 只傳 2K token 給 LLM，首字回應快，算力壓力小 |

**選擇準則：**
- 問「這件事怎麼做？」→ Dify RAG（FAQ 型）
- 問「這個客戶怎麼了？」→ LLM Wiki 全文讀入（歷史分析型）
- 問「這個欄位等於什麼？」→ CSA Text-to-SQL（見 §13.15）

---

### 13.16.10 開發前 Checklist

正式開始開發前，須確認以下事項：

**基礎設施**
- [ ] GB10 / 服務器安裝 Linux，創建 `user_general` 和 `user_vip` 兩個 OS 用戶
- [ ] vLLM 部署 Qwen3.6-35B-A3B（需 `vllm>=0.19.0`），啟用 Tool Calling：
  ```bash
  vllm serve Qwen/Qwen3.6-35B-A3B --port 8000 \
    --max-model-len 262144 \
    --enable-auto-tool-choice --tool-call-parser qwen3_coder
  ```
- [ ] Dify 本地 Docker 部署，配置本地 vLLM 為 LLM Provider
- [ ] FastAPI `wiki_api.py` 在本地可運行，Dify 能成功呼叫 Custom Tool

**數據與權限**
- [ ] CSA 資料庫中「員工 → 負責客戶」對應關係表確認完整且準確
- [ ] 新建 `AI_Permissions` 覆蓋層表格（部門/職級 → AI 查詢範圍映射）
- [ ] VIP 客戶名單確認，`VIP_Clearance` 名單建立
- [ ] 員工 `user_id` 可從 Web System 登入 Session 可靠傳遞至 Dify

**安全與合規**
- [ ] 所有 SQL 查詢帳號設為 Read-Only（無 UPDATE/DELETE 權限）
- [ ] Audit Log 機制確認（user_id + 查詢客戶 + 回傳結果 + 時間戳）
- [ ] 外部聯網工具（Tavily/Google）已加 Python 客戶名稱攔截層
- [ ] 測試：離職員工是否自動失去存取（更新 CSA → 即時生效）
- [ ] 測試：Prompt Injection 能否繞過 Python 守門取得其他客戶資料

**Ingest Pipeline**
- [ ] 郵件清洗腳本能正確去除 HTML、簽名檔、免責聲明
- [ ] Markdown 合併邏輯測試（新知識加入，舊知識不丟失，無重複）
- [ ] 單一客戶 `.md` 大小估算（建議控制在 128K token 以內，保留 context 空間）
- [ ] 郵件連接方式確認：IMAP / Exchange / `.eml` 匯出

**MVP 開發順序**
1. 寫 Python 腳本讀取單封測試郵件，呼叫 vLLM，生成標準 Markdown
2. 寫 `wiki_api.py`（`read_wiki` + `check_permission`）
3. 在 Dify 註冊 `read_client_wiki` 工具，端對端測試 Query 流程
4. 完善 Ingest 合併邏輯（讀取舊頁面 + 融合新內容 + 寫回）
5. 加入 VIP 雙軌 Pipeline 與 OS 用戶隔離
6. 加入 Query Sanitisation（聯網搜尋脫敏攔截）
7. Web 系統對接 Dify Chat API
8. 灰度測試（3–5 人核心小組）→ 收集問題 → 全面推廣

---

*本方案書由 GitHub Copilot 協助整理，最終決策請結合公司實際情況評估。*
*第 13 節於 2026 年 4 月 18 日更新；第 13.8 節於 2026 年 4 月 19 日新增（參考 jason-effi-lab/karpathy-llm-wiki-vault）。*
*第 13.9–13.11 節於 2026 年 4 月 19 日新增（硬件模型選型、Tool Use、llama-server Windows 並發）。*
*第 13.12 節於 2026 年 4 月 19 日新增（兩條 Ingest 路徑串聯 Dify RAG 全局架構圖）。*
*第 13.13 節於 2026 年 4 月 19 日新增（路徑 B 上傳後確保 LLM 讀懂的三個配置：Chunking / Metadata / System Prompt）。*
*第 13.14 節於 2026 年 4 月 19 日新增（WSL2 + vLLM 效能上頂部署方案：PagedAttention / FP8 / Docker 部署）。*
*第 13.15 節於 2026 年 4 月 19 日新增（CSA Text-to-SQL：Dify Tool Calling 直查 SQL Server，含安全邊界、FastAPI middleware、對話範例）。*
*第 13.16 節於 2026 年 4 月 22 日新增（Email LLM Wiki + 企業 RBAC 架構預備設計：讀寫分離、Python Middleware 守門、VIP 雙軌物理隔離、Query Sanitisation、縱深防禦）。*
