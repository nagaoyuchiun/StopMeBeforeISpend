<!-- markdownlint-disable-file -->

# Task Details: 大額消費評估功能

## Research Reference

**Source Research**: #file:../research/20260107-before-spend-evaluation-research.md

## Phase 1: 評估表單 UI

### Task 1.1: 建立評估頁面組件

建立消費評估主頁面，設定基本版面結構。

- **Files**:
  - src/views/EvaluateView.vue - 評估頁面主組件
- **Success**:
  - 頁面可透過路由訪問 `/evaluate`
  - 基本版面結構建立完成
  - 頁面標題「大額消費評估」顯示
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 44-56) - UI 設計考量
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 58-72) - 技術實作方式
- **Dependencies**:
  - Vue Router 已設定
  - 路由守衛已實作

### Task 1.2: 實作表單輸入欄位

建立消費金額、類型、動機的輸入表單。

- **Files**:
  - src/components/ExpenseEvaluationForm.vue - 表單組件
- **Success**:
  - 消費金額數字輸入框正常運作
  - 消費類型下拉選單包含所有類別
  - 消費動機文字輸入框（選填）
  - v-model 雙向綁定正確
  - 表單資料響應式更新
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 20-29) - 使用者輸入欄位定義
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 44-62) - 表單結構設計
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 74-84) - 響應式資料設計
- **Dependencies**:
  - Task 1.1 完成

### Task 1.3: 加入表單驗證

實作輸入驗證邏輯，確保資料正確性。

- **Files**:
  - src/components/ExpenseEvaluationForm.vue - 加入驗證邏輯
  - src/composables/useFormValidation.js - 驗證 composable（可選）
- **Success**:
  - 金額必填驗證
  - 金額必須 > 0 驗證
  - 金額 < 2000 時顯示警告提示
  - 類型必選驗證
  - 驗證錯誤訊息正確顯示
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 99-103) - 表單驗證需求
  - docs/PRD.md (Lines 37-38) - 大額消費判定條件（≥ 2000 元）
- **Dependencies**:
  - Task 1.2 完成

## Phase 2: 計算邏輯實作

### Task 2.1: 建立 evaluationStore

建立管理評估系統參數與計算邏輯的 Pinia store。

- **Files**:
  - src/stores/evaluation.js - 評估 store
- **Success**:
  - systemParams 狀態定義（eBase, eSafe）
  - expenseCategories 狀態定義
  - calculateDelayMonths action 實作
  - calculateDelayDays action 實作
  - store 可在組件中正常使用
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 105-139) - evaluationStore 設計
  - docs/PRD.md (Lines 69-77) - 系統參數與計算公式
- **Dependencies**:
  - Pinia 已設定

### Task 2.2: 實作延長時間計算函式

在 store 中實作符合 PRD 公式的計算邏輯。

- **Files**:
  - src/stores/evaluation.js - 加入計算方法
- **Success**:
  - 延長月數計算正確（C / E_base, C / E_safe）
  - 延長天數計算正確（月數 × 30）
  - 計算結果保留適當小數位數
  - 邊界值測試通過（0, 2000, 100000）
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 31-39) - 計算公式
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 86-97) - 計算邏輯實作
  - docs/PRD.md (Lines 73-77) - 計算公式定義
- **Dependencies**:
  - Task 2.1 完成

### Task 2.3: 整合即時計算功能

在表單組件中整合 store，實現金額輸入時即時計算。

- **Files**:
  - src/components/ExpenseEvaluationForm.vue - 整合計算邏輯
- **Success**:
  - 金額輸入時自動觸發計算
  - watch 監聽正確運作
  - 計算結果即時更新
  - 金額 < 2000 時不觸發計算（僅警告）
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 162-170) - 即時計算實作
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 86-97) - 計算邏輯整合
- **Dependencies**:
  - Task 2.2 完成
  - Task 1.2 完成

## Phase 3: 結果顯示組件

### Task 3.1: 建立評估結果組件

建立專門顯示計算結果的組件。

- **Files**:
  - src/components/EvaluationResult.vue - 結果顯示組件
- **Success**:
  - 接收計算結果 props
  - 基本版結果顯示區塊
  - 保守版結果顯示區塊
  - 月數與天數分別顯示
  - 組件可在評估頁面中正常使用
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 64-72) - 結果顯示區設計
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 58-62) - Vue 3 組件結構
- **Dependencies**:
  - Task 1.1 完成

### Task 3.2: 實作結果格式化顯示

優化數字顯示格式，提升可讀性。

- **Files**:
  - src/components/EvaluationResult.vue - 加入格式化邏輯
  - src/utils/formatters.js - 格式化工具函式（可選）
- **Success**:
  - 月數顯示一位小數（例：2.5 個月）
  - 天數顯示整數（例：75 天）
  - 數字千分位格式化（可選）
  - 顯示「相當於延後 X 個月」提示
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 172-179) - 視覺化建議
- **Dependencies**:
  - Task 3.1 完成

### Task 3.3: 加入視覺化與提示

使用顏色與樣式強調評估結果的嚴重程度。

- **Files**:
  - src/components/EvaluationResult.vue - 加入樣式邏輯
- **Success**:
  - 基本版與保守版使用不同顏色區分
  - 延長天數 > 90 時使用警告色
  - 加入圖示或進度條視覺化（可選）
  - 人性化提示文字
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 172-179) - 視覺化建議
  - docs/PRD.md (Lines 109-111) - 設計原則
- **Dependencies**:
  - Task 3.2 完成

## Phase 4: 資料儲存功能

### Task 4.1: 實作「確認消費」按鈕

在結果組件中加入確認消費的互動邏輯。

- **Files**:
  - src/components/EvaluationResult.vue - 加入按鈕與處理函式
- **Success**:
  - 「確認消費」按鈕正常顯示
  - 「取消」按鈕可清空表單
  - 點擊確認後觸發儲存流程
  - 按鈕狀態管理正確（loading, disabled）
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 64-72) - 結果顯示區設計
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 141-149) - 確認消費後流程
- **Dependencies**:
  - Task 3.1 完成

### Task 4.2: 寫入 Firestore largeExpenses

實作將評估結果儲存到 Firestore 的功能。

- **Files**:
  - src/stores/expense.js - 加入 addExpense action
  - src/services/firestore.js - 加入寫入函式（如未有）
- **Success**:
  - 消費記錄正確寫入 Firestore
  - 資料結構符合 collection schema
  - userId 正確關聯
  - timestamp 正確記錄
  - 寫入成功後返回 document ID
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 121-130) - largeExpenses collection 結構
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 141-149) - 資料持久化
- **Dependencies**:
  - Task 4.1 完成
  - expenseStore 已建立
  - Firestore 服務模組可用

### Task 4.3: 更新債務計畫資料

消費確認後更新債務計畫的預計清償日期。

- **Files**:
  - src/stores/debt.js - 加入 updatePayoffDate action
- **Success**:
  - currentPayoffDate 正確延長
  - 延長時間基於保守版計算（E_safe）
  - Firestore debtPlans 正確更新
  - 更新成功後導向儀表板
  - 錯誤處理完整
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 111-119) - debtPlans collection 結構
  - docs/PRD.md (Lines 83-87) - 實際消費影響儀表板資料需求
- **Dependencies**:
  - Task 4.2 完成
  - debtStore 已建立

## Phase 5: UX 優化

### Task 5.1: 加入大額消費說明

在頁面中顯示「什麼算大額消費」的說明資訊。

- **Files**:
  - src/components/ExpenseGuidelines.vue - 說明組件
  - src/views/EvaluateView.vue - 整合說明組件
- **Success**:
  - 顯示大額消費定義（≥ 2000 元）
  - 列出不算大額消費的項目
  - 列出算大額消費的項目
  - 可摺疊/展開說明內容（可選）
- **Research References**:
  - docs/PRD.md (Lines 35-59) - 大額消費定義
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 74-79) - 提示資訊
- **Dependencies**:
  - Task 1.1 完成

### Task 5.2: 實作錯誤處理與提示

加入完整的錯誤處理與使用者友善提示。

- **Files**:
  - src/components/ExpenseEvaluationForm.vue - 加入錯誤處理
  - src/composables/useToast.js - Toast 通知 composable（可選）
- **Success**:
  - 網路錯誤友善提示
  - Firestore 寫入失敗提示
  - 表單驗證錯誤提示
  - 成功儲存後顯示確認訊息
  - Loading 狀態視覺化
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 181-186) - 錯誤處理
- **Dependencies**:
  - Task 4.3 完成

### Task 5.3: 優化表單互動體驗

改善表單的整體使用體驗。

- **Files**:
  - src/components/ExpenseEvaluationForm.vue - UX 優化
  - src/views/EvaluateView.vue - 整體流程優化
- **Success**:
  - 金額輸入框自動 focus
  - Enter 鍵可觸發計算
  - 清空表單功能正常
  - 消費後表單自動重置
  - 過渡動畫流暢（可選）
  - 行動裝置體驗友善（可選）
- **Research References**:
  - #file:../research/20260107-before-spend-evaluation-research.md (Lines 162-179) - UI/UX 最佳實踐
- **Dependencies**:
  - Task 5.2 完成

## Dependencies

- Vue 3 專案已建立
- Pinia 已設定
- Firebase Firestore 已配置
- expenseStore 已建立
- debtStore 已建立
- authStore 已建立（取得 userId）

## Success Criteria

- 評估表單功能完整可用
- 計算邏輯符合 PRD 規格
- 即時計算運作正常
- 資料正確儲存到 Firestore
- 債務計畫正確更新
- UI/UX 直觀易用
- 錯誤處理完整
- 符合 PRD 設計原則
