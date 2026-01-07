<!-- markdownlint-disable-file -->

# Research: 實際大額消費影響儀表板 (After-Spend Dashboard)

**Research Date**: 2026-01-07
**Task**: 實作消費後影響追蹤與儀表板顯示

## 研究來源

### 專案需求文件
- docs/PRD.md (Lines 80-87) - 實際大額消費影響儀表板功能定義
- docs/PRD.md (Lines 35-59) - 大額消費定義與判定條件

## 功能需求分析

### 核心指標
根據 PRD 定義，儀表板需要顯示：
1. **原預計清償年月** - 最初設定的還債目標日期
2. **新預估清償年月** - 受消費影響後的新預估日期
3. **累計延長月數** - 總共延後了多少個月
4. **累計大額消費金額** - 所有大額消費的總和

### 資料來源
- `debtPlans` collection - 債務計畫基本資料
- `largeExpenses` collection - 所有大額消費記錄

## UI 設計考量

### 儀表板版面結構
```
[儀表板]
├─ 核心指標卡片
│  ├─ 原預計清償年月
│  ├─ 新預估清償年月
│  ├─ 累計延長月數
│  └─ 累計大額消費金額
├─ 時間軸視覺化
│  ├─ 原計畫時間線
│  ├─ 當前進度
│  └─ 新預估時間線
└─ 大額消費記錄列表
   ├─ 日期
   ├─ 金額
   ├─ 類型
   └─ 延長月數
```

### 視覺呈現優先級
1. **強調對比** - 原計畫 vs 新預估的差異
2. **時間感知** - 讓使用者清楚知道「延後了多久」
3. **累積效應** - 顯示多次消費的累積影響

## 資料計算邏輯

### 累計延長月數計算
```javascript
const totalDelayMonths = expenses.reduce((sum, expense) => {
  return sum + expense.delayMonthsSafe
}, 0)
```

### 累計消費金額計算
```javascript
const totalExpenseAmount = expenses.reduce((sum, expense) => {
  return sum + expense.amount
}, 0)
```

### 新預估清償日期計算
```javascript
const newPayoffDate = new Date(originalPayoffDate)
newPayoffDate.setMonth(newPayoffDate.getMonth() + totalDelayMonths)
```

## 技術實作方式

### Vue 3 組件結構
```
DashboardView.vue (頁面)
├─ DebtSummaryCard.vue (核心指標卡片)
├─ TimelineVisualization.vue (時間軸視覺化)
└─ ExpenseHistoryList.vue (消費記錄列表)
```

### Pinia Store 資料流
```javascript
// stores/dashboard.js
import { defineStore } from 'pinia'
import { useDebtStore } from './debt'
import { useExpenseStore } from './expense'

export const useDashboardStore = defineStore('dashboard', {
  getters: {
    // 從其他 stores 組合資料
    summaryData() {
      const debtStore = useDebtStore()
      const expenseStore = useExpenseStore()
      
      return {
        originalPayoffDate: debtStore.originalPayoffDate,
        currentPayoffDate: debtStore.currentPayoffDate,
        totalDelayMonths: expenseStore.totalDelayMonths,
        totalExpenseAmount: expenseStore.totalExpenseAmount
      }
    }
  }
})
```

### 響應式資料設計
```javascript
// 在 DashboardView.vue 中
const { summaryData } = storeToRefs(useDashboardStore())
const { expenses } = storeToRefs(useExpenseStore())

// 自動響應資料變化
watch(summaryData, (newData) => {
  // 更新視覺化
})
```

## 時間軸視覺化方案

### 選項 1: 簡單進度條
使用 CSS 進度條表示：
- 起點：債務開始日期
- 終點：新預估清償日期
- 標記：原預計清償日期
- 當前：今天的位置

### 選項 2: 月份刻度時間軸
顯示按月份的時間軸，標記重要節點：
- 原計畫完成點
- 當前時間點
- 新預估完成點
- 每次大額消費的影響

### 選項 3: 使用圖表庫
考慮使用 Chart.js 或類似庫繪製時間軸。

### 建議實作
Phase 1 採用選項 1（簡單進度條），後續可優化至選項 2 或 3。

## 大額消費記錄列表

### 列表資料結構
```javascript
[
  {
    id: 'expense-id',
    spentAt: Timestamp,
    amount: 25000,
    category: '3C 與電子產品',
    motivation: '筆電壞掉需要工作',
    delayMonthsSafe: 3.125
  }
]
```

### 顯示格式
- **日期**: 2026/01/05
- **金額**: NT$ 25,000
- **類型**: 3C 與電子產品
- **影響**: 延後 3.1 個月

### 排序與篩選
- 預設按日期降序排列（最新在上）
- 可選：按金額、影響程度排序
- 可選：按類型篩選

## Firestore 查詢設計

### 取得債務計畫
```javascript
const getDebtPlan = async (userId) => {
  const q = query(
    collection(db, 'debtPlans'),
    where('userId', '==', userId),
    limit(1)
  )
  const snapshot = await getDocs(q)
  return snapshot.docs[0]?.data()
}
```

### 取得大額消費記錄
```javascript
const getExpenses = async (userId) => {
  const q = query(
    collection(db, 'largeExpenses'),
    where('userId', '==', userId),
    orderBy('spentAt', 'desc')
  )
  const snapshot = await getDocs(q)
  return snapshot.docs.map(doc => ({
    id: doc.id,
    ...doc.data()
  }))
}
```

## 即時更新機制

### Firestore Snapshot Listeners
使用 `onSnapshot` 實現即時資料同步：

```javascript
const listenToExpenses = (userId, callback) => {
  const q = query(
    collection(db, 'largeExpenses'),
    where('userId', '==', userId)
  )
  
  return onSnapshot(q, (snapshot) => {
    const expenses = snapshot.docs.map(doc => ({
      id: doc.id,
      ...doc.data()
    }))
    callback(expenses)
  })
}
```

## 日期格式化

### 年月顯示
```javascript
const formatYearMonth = (date) => {
  return new Intl.DateTimeFormat('zh-TW', {
    year: 'numeric',
    month: 'long'
  }).format(date)
}
// 輸出: "2026年1月"
```

### 完整日期顯示
```javascript
const formatFullDate = (date) => {
  return new Intl.DateTimeFormat('zh-TW', {
    year: 'numeric',
    month: '2-digit',
    day: '2-digit'
  }).format(date)
}
// 輸出: "2026/01/07"
```

## UX 考量

### 空狀態處理
當尚無大額消費記錄時：
- 顯示友善提示「目前沒有大額消費記錄」
- 提供「評估消費」按鈕導向評估頁

### Loading 狀態
- 資料載入時顯示 skeleton 或 loading spinner
- 避免閃爍，設定最小 loading 時間

### 錯誤處理
- Firestore 讀取失敗時友善提示
- 提供重試按鈕

### 響應式設計
- 手機版：卡片垂直堆疊
- 桌面版：卡片水平排列

## 測試考量

### 單元測試
- 累計計算邏輯正確性
- 日期計算準確性
- 格式化函式輸出

### 整合測試
- Firestore 查詢正確性
- 即時更新機制運作
- Store 資料流正確

### E2E 測試
- 儀表板載入流程
- 新增消費後儀表板更新
- 各種資料狀態的顯示

## 效能優化

### 資料快取
使用 Pinia 快取 Firestore 資料，避免重複查詢。

### 分頁載入
如果消費記錄很多，考慮分頁或虛擬滾動。

### 計算快取
使用 computed 快取計算結果，避免重複運算。

## 實作順序建議

### Phase 1: 資料層
1. 擴充 debtStore 和 expenseStore
2. 實作 Firestore 查詢函式
3. 建立 dashboardStore

### Phase 2: 核心指標顯示
1. 建立 DebtSummaryCard 組件
2. 顯示四個核心指標
3. 實作格式化與樣式

### Phase 3: 消費記錄列表
1. 建立 ExpenseHistoryList 組件
2. 顯示消費記錄
3. 實作排序功能

### Phase 4: 視覺化
1. 建立時間軸組件
2. 實作進度條視覺化
3. 標記重要節點

### Phase 5: 即時更新與優化
1. 整合 Firestore listeners
2. 實作 loading 與錯誤處理
3. UX 優化

## 參考資源

- Firestore 即時監聽: https://firebase.google.com/docs/firestore/query-data/listen
- Vue 3 Composition API: https://vuejs.org/guide/extras/composition-api-faq.html
- Pinia Store Composition: https://pinia.vuejs.org/core-concepts/#using-the-store
- Intl.DateTimeFormat: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat

## 研究結論

實際消費影響儀表板的重點在於清晰呈現「消費累積效應」與「時間延後影響」。建議採用卡片式佈局配合簡單的時間軸視覺化，並使用 Firestore 即時監聽確保資料同步，讓使用者能直觀感受到每次消費對還債計畫的實際影響。
