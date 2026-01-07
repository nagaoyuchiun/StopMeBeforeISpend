<!-- markdownlint-disable-file -->

# Research: 大額消費評估功能 (Before-Spend Feature)

**Research Date**: 2026-01-07
**Task**: 實作消費前影響評估計算與 UI

## 研究來源

### 專案需求文件
- docs/PRD.md (Lines 62-77) - 大額消費評估頁功能定義
- docs/PRD.md (Lines 35-59) - 大額消費定義與判定條件

## 功能需求分析

### 核心功能
根據 PRD 定義，此功能需要：
1. 輸入消費金額並評估對還債期限的影響
2. 提供兩種計算模式（基本版與保守版）
3. 顯示延長天數與月數
4. 記錄消費類型與動機

### 使用者輸入欄位
- **消費金額 (C)**: 數字輸入，必填，≥ 2000
- **消費類型**: 下拉選單，必填
- **消費動機**: 文字輸入，選填

### 系統參數
- **E_base**: 每月可持續額外還款能力，預設 10,000 元
- **E_safe**: 保守版額外還款能力，預設 8,000 元

### 計算公式
```
延長月數_base = C / E_base
延長月數_safe = C / E_safe
延長天數_base = 延長月數_base × 30
延長天數_safe = 延長月數_safe × 30
```

## UI 設計考量

### 表單結構
```
[消費評估]
├─ 消費金額輸入框（數字）
├─ 消費類型選單
│  ├─ 3C 與電子產品
│  ├─ 非必要訂閱或升級
│  ├─ 旅遊、住宿、娛樂
│  ├─ 情緒性購物
│  └─ 非必要課程或服務
├─ 消費動機（選填）
└─ [計算影響] 按鈕
```

### 結果顯示區
```
[影響評估結果]
├─ 基本版延長時間
│  ├─ X.X 個月
│  └─ XX 天
├─ 保守版延長時間
│  ├─ X.X 個月
│  └─ XX 天
└─ [確認消費] / [取消] 按鈕
```

### 提示資訊
- 需顯示「什麼不算大額消費」的說明
- 需提供 2000 元門檻的提示

## 技術實作方式

### Vue 3 組件結構
```
EvaluateView.vue (頁面)
├─ ExpenseEvaluationForm.vue (表單組件)
└─ EvaluationResult.vue (結果顯示組件)
```

### 響應式資料設計
```javascript
const form = reactive({
  amount: null,
  category: '',
  motivation: ''
})

const result = reactive({
  delayMonthsBase: 0,
  delayMonthsSafe: 0,
  delayDaysBase: 0,
  delayDaysSafe: 0
})
```

### 計算邏輯實作
```javascript
const calculateImpact = () => {
  const E_BASE = 10000
  const E_SAFE = 8000
  
  if (form.amount < 2000) {
    // 顯示警告：低於大額消費門檻
    return
  }
  
  result.delayMonthsBase = form.amount / E_BASE
  result.delayMonthsSafe = form.amount / E_SAFE
  result.delayDaysBase = result.delayMonthsBase * 30
  result.delayDaysSafe = result.delayMonthsSafe * 30
}
```

### 表單驗證
- 金額必填且 > 0
- 類型必選
- 金額建議 ≥ 2000（警告但不阻擋）

## Pinia Store 整合

### evaluationStore 設計
```javascript
// stores/evaluation.js
import { defineStore } from 'pinia'

export const useEvaluationStore = defineStore('evaluation', {
  state: () => ({
    systemParams: {
      eBase: 10000,
      eSafe: 8000
    },
    expenseCategories: [
      '3C 與電子產品',
      '非必要訂閱或升級',
      '旅遊、住宿、娛樂',
      '情緒性購物',
      '非必要課程或服務'
    ]
  }),
  
  actions: {
    calculateDelayMonths(amount) {
      return {
        base: amount / this.systemParams.eBase,
        safe: amount / this.systemParams.eSafe
      }
    },
    
    calculateDelayDays(delayMonths) {
      return {
        base: delayMonths.base * 30,
        safe: delayMonths.safe * 30
      }
    }
  }
})
```

## 資料持久化（選擇性）

### 暫存評估記錄
如果使用者想要保留評估記錄（未實際消費），可考慮：
- localStorage 暫存
- 或直接導向「確認消費」流程

### 確認消費後
- 寫入 Firestore `largeExpenses` collection
- 更新 `debtPlans` 的 `currentPayoffDate`
- 導向儀表板頁面

## UI/UX 最佳實踐

### 即時計算
使用 `watch` 或 `computed` 在金額輸入時即時顯示結果。

```javascript
watch(() => form.amount, (newAmount) => {
  if (newAmount >= 2000) {
    calculateImpact()
  }
})
```

### 視覺化建議
- 使用不同顏色區分基本版與保守版
- 延長天數較大時使用警告色
- 提供「這相當於延後 X 個月」的人性化提示

### 錯誤處理
- 金額為 0 或負數時顯示錯誤
- 未選擇類型時阻止提交
- 網路錯誤時友善提示

## 測試考量

### 單元測試
- 計算公式正確性
- 邊界值測試（金額 = 0, 1999, 2000, 100000）
- 系統參數變更時計算更新

### 整合測試
- 表單提交流程
- Firestore 寫入驗證
- 路由導向正確性

## 可選優化

### 系統參數可調整
允許使用者在設定頁面調整 E_base 和 E_safe。

### 歷史評估記錄
顯示過去的評估記錄（未實際消費的預估）。

### 比較功能
輸入多個金額同時比較影響程度。

## 實作順序建議

### Phase 1: 基本 UI 與計算
1. 建立 EvaluateView 頁面
2. 實作表單輸入
3. 實作計算邏輯
4. 顯示計算結果

### Phase 2: Store 整合
1. 建立 evaluationStore
2. 將計算邏輯移至 store
3. 將系統參數移至 store

### Phase 3: 資料儲存
1. 實作「確認消費」功能
2. 寫入 Firestore
3. 更新債務計畫資料

### Phase 4: UX 優化
1. 加入即時計算
2. 改善視覺呈現
3. 加入驗證與錯誤處理

## 參考資源

- Vue 3 表單處理: https://vuejs.org/guide/essentials/forms.html
- Pinia Getters: https://pinia.vuejs.org/core-concepts/getters.html
- Firestore 資料新增: https://firebase.google.com/docs/firestore/manage-data/add-data

## 研究結論

大額消費評估功能的核心在於簡單明確的數學計算，搭配清晰的 UI 呈現。建議採用 Vue 3 響應式特性實現即時計算，並透過 Pinia store 管理系統參數與計算邏輯，確保程式碼可維護性。
