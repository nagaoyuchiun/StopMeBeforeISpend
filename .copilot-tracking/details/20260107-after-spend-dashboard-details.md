<!-- markdownlint-disable-file -->

# Task Details: 實際大額消費影響儀表板

## Research Reference

**Source Research**: #file:../research/20260107-after-spend-dashboard-research.md

## Phase 1: 資料層擴充

### Task 1.1: 擴充 debtStore 功能

在現有 debtStore 中加入儀表板所需的額外功能。

- **Files**:
  - src/stores/debt.js - 擴充功能
- **Success**:
  - originalPayoffDate getter 實作
  - currentPayoffDate getter 實作
  - fetchDebtPlan action 可正確載入資料
  - 資料從 Firestore debtPlans 正確讀取
  - store 狀態正確更新
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 111-119) - debtPlans collection 結構
  - docs/PRD.md (Lines 82-85) - 核心指標定義
- **Dependencies**:
  - debtStore 基礎版本已建立
  - Firestore 服務模組可用

### Task 1.2: 擴充 expenseStore 功能

在現有 expenseStore 中加入累計計算功能。

- **Files**:
  - src/stores/expense.js - 加入 getters
- **Success**:
  - totalDelayMonths getter 實作（累計延長月數）
  - totalExpenseAmount getter 實作（累計消費金額）
  - fetchExpenses action 可正確載入資料
  - expenses 陣列狀態管理正確
  - getters 計算結果正確
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 36-50) - 資料計算邏輯
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 121-130) - largeExpenses collection 結構
- **Dependencies**:
  - expenseStore 基礎版本已建立
  - Firestore 服務模組可用

### Task 1.3: 建立 dashboardStore

建立專門用於儀表板的 Pinia store，組合其他 stores 的資料。

- **Files**:
  - src/stores/dashboard.js - 新建 dashboard store
- **Success**:
  - summaryData getter 實作（組合債務與消費資料）
  - 正確引用 debtStore 和 expenseStore
  - getter 回傳完整的儀表板摘要資料
  - store 可在組件中正常使用
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 52-73) - Pinia Store 資料流設計
  - docs/PRD.md (Lines 82-87) - 核心指標需求
- **Dependencies**:
  - Task 1.1 完成
  - Task 1.2 完成

## Phase 2: 核心指標顯示

### Task 2.1: 建立儀表板主頁面

建立儀表板頁面組件，作為其他子組件的容器。

- **Files**:
  - src/views/DashboardView.vue - 儀表板主頁面
- **Success**:
  - 頁面可透過路由訪問 `/dashboard`
  - 頁面載入時觸發資料獲取
  - 基本版面結構建立完成
  - onMounted 生命週期正確使用
  - 頁面標題「還債進度儀表板」顯示
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 20-34) - 儀表板版面結構
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 52-54) - Vue 3 組件結構
- **Dependencies**:
  - Vue Router 已設定
  - Task 1.3 完成

### Task 2.2: 建立核心指標卡片組件

實作顯示四個核心指標的卡片組件。

- **Files**:
  - src/components/DebtSummaryCard.vue - 指標卡片組件
- **Success**:
  - 顯示原預計清償年月
  - 顯示新預估清償年月
  - 顯示累計延長月數
  - 顯示累計大額消費金額
  - 數字格式化正確（千分位）
  - 卡片樣式清晰易讀
  - 原計畫與新預估的對比明顯
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 20-34) - 核心指標卡片設計
  - docs/PRD.md (Lines 82-87) - 核心指標定義
- **Dependencies**:
  - Task 2.1 完成
  - Task 1.3 完成（dashboardStore）

### Task 2.3: 實作日期格式化工具

建立日期格式化工具函式，統一管理日期顯示格式。

- **Files**:
  - src/utils/dateFormatter.js - 日期格式化工具
- **Success**:
  - formatYearMonth 函式實作（例：2026年1月）
  - formatFullDate 函式實作（例：2026/01/07）
  - 使用 Intl.DateTimeFormat 正確處理 zh-TW locale
  - Timestamp 轉換為 Date 正確處理
  - 工具函式可在多個組件中複用
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 140-161) - 日期格式化實作
- **Dependencies**:
  - 無

## Phase 3: 消費記錄列表

### Task 3.1: 建立消費記錄列表組件

建立顯示所有大額消費記錄的列表組件。

- **Files**:
  - src/components/ExpenseHistoryList.vue - 消費記錄列表
- **Success**:
  - 列表正確顯示所有消費記錄
  - 從 expenseStore 取得資料
  - 記錄按日期降序排列
  - 空狀態顯示友善提示
  - 列表項目結構清晰
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 95-118) - 大額消費記錄列表設計
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 20-34) - 儀表板版面結構
- **Dependencies**:
  - Task 1.2 完成（expenseStore）
  - Task 2.1 完成（DashboardView）

### Task 3.2: 實作記錄項目顯示

在列表中實作單筆消費記錄的顯示格式。

- **Files**:
  - src/components/ExpenseHistoryList.vue - 加入項目顯示邏輯
  - src/components/ExpenseItem.vue - 記錄項目組件（可選）
- **Success**:
  - 日期格式化顯示（使用 formatFullDate）
  - 金額顯示千分位格式（NT$ 25,000）
  - 消費類型正確顯示
  - 延長月數顯示（例：延後 3.1 個月）
  - 動機資訊顯示（如有）
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 99-118) - 列表資料結構與顯示格式
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 140-161) - 日期格式化
- **Dependencies**:
  - Task 3.1 完成
  - Task 2.3 完成（日期格式化工具）

### Task 3.3: 加入排序功能

實作消費記錄的排序功能（選擇性）。

- **Files**:
  - src/components/ExpenseHistoryList.vue - 加入排序邏輯
- **Success**:
  - 預設按日期降序排列
  - 可選按金額排序
  - 可選按延長月數排序
  - 排序控制 UI 直觀
  - 排序切換流暢
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 109-118) - 排序與篩選
- **Dependencies**:
  - Task 3.2 完成

## Phase 4: 時間軸視覺化

### Task 4.1: 建立時間軸組件

建立時間軸視覺化組件，顯示還債進度。

- **Files**:
  - src/components/TimelineVisualization.vue - 時間軸組件
- **Success**:
  - 組件可接收債務計畫資料 props
  - 基本 SVG 或 div 結構建立
  - 組件可在儀表板中正常顯示
  - 響應式寬度調整
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 75-93) - 時間軸視覺化方案
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 20-34) - 儀表板版面結構
- **Dependencies**:
  - Task 2.1 完成

### Task 4.2: 實作進度條視覺化

使用進度條方式呈現還債時間軸。

- **Files**:
  - src/components/TimelineVisualization.vue - 加入進度條邏輯
- **Success**:
  - 起點標記（債務開始日期）
  - 終點標記（新預估清償日期）
  - 當前進度位置正確計算
  - 進度百分比計算正確
  - 視覺樣式清晰
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 77-86) - 簡單進度條方案
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 36-50) - 日期計算邏輯
- **Dependencies**:
  - Task 4.1 完成

### Task 4.3: 標記重要時間節點

在時間軸上標記原計畫完成點與新預估完成點。

- **Files**:
  - src/components/TimelineVisualization.vue - 加入節點標記
- **Success**:
  - 原預計清償日期標記顯示
  - 新預估清償日期標記顯示
  - 今天日期標記顯示
  - 標記位置計算正確
  - 標記文字說明清楚
  - 使用顏色區分原計畫與新預估
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 77-86) - 時間軸標記設計
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 36-39) - 視覺呈現優先級
- **Dependencies**:
  - Task 4.2 完成

## Phase 5: 即時更新與優化

### Task 5.1: 整合 Firestore 即時監聽

使用 Firestore onSnapshot 實現資料即時同步。

- **Files**:
  - src/stores/expense.js - 加入即時監聽 action
  - src/stores/debt.js - 加入即時監聽 action
  - src/services/firestore.js - 加入監聽函式
- **Success**:
  - listenToExpenses 函式實作
  - listenToDebtPlan 函式實作
  - 資料變更時自動更新 UI
  - 監聽器在組件卸載時正確清理
  - 無記憶體洩漏
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 135-154) - 即時更新機制
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 120-133) - Firestore 查詢設計
- **Dependencies**:
  - Task 1.2 完成（expenseStore）
  - Task 1.1 完成（debtStore）

### Task 5.2: 實作 Loading 與空狀態

加入資料載入中與無資料的友善 UI 狀態。

- **Files**:
  - src/views/DashboardView.vue - 加入 loading 狀態
  - src/components/ExpenseHistoryList.vue - 加入空狀態
  - src/components/LoadingSkeleton.vue - Loading 骨架組件（可選）
- **Success**:
  - 資料載入時顯示 loading 指示器
  - 無消費記錄時顯示空狀態提示
  - 空狀態提供「評估消費」按鈕
  - Loading 狀態避免閃爍
  - 骨架屏或 spinner 視覺效果友善
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 163-173) - UX 考量
- **Dependencies**:
  - Task 2.1 完成
  - Task 3.1 完成

### Task 5.3: 錯誤處理與 UX 優化

實作完整的錯誤處理與使用者體驗優化。

- **Files**:
  - src/views/DashboardView.vue - 加入錯誤處理
  - src/components/ErrorAlert.vue - 錯誤提示組件（可選）
- **Success**:
  - Firestore 讀取失敗友善提示
  - 提供重試按鈕
  - 網路錯誤狀態處理
  - 手機版響應式佈局正確
  - 桌面版卡片水平排列
  - 整體 UI 符合 PRD 設計原則
- **Research References**:
  - #file:../research/20260107-after-spend-dashboard-research.md (Lines 169-178) - 錯誤處理與響應式設計
  - docs/PRD.md (Lines 109-111) - 設計原則
- **Dependencies**:
  - Task 5.2 完成

## Dependencies

- Vue 3 專案已建立
- Pinia stores (auth, debt, expense) 已建立
- Firebase Firestore 已配置
- Firestore collections (debtPlans, largeExpenses) 結構已定義

## Success Criteria

- 儀表板完整顯示所有核心指標
- 累計計算邏輯正確無誤
- 消費記錄列表功能完整
- 時間軸視覺化清晰呈現進度
- Firestore 即時監聽正常運作
- Loading 與空狀態友善
- 錯誤處理完整
- 響應式設計適配各裝置
- 符合 PRD 設計原則
