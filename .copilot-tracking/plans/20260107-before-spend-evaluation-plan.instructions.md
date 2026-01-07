---
applyTo: ".copilot-tracking/changes/20260107-before-spend-evaluation-changes.md"
---

<!-- markdownlint-disable-file -->

# Task Checklist: 大額消費評估功能

## Overview

實作消費前影響評估功能，讓使用者輸入消費金額後計算對還債期限的延長影響，提供基本版與保守版兩種評估結果。

## Objectives

- 建立消費評估表單 UI
- 實作延長月數與天數計算邏輯
- 整合 Pinia store 管理系統參數
- 實作「確認消費」資料儲存功能
- 提供清晰的評估結果呈現

## Research Summary

### Project Files

- docs/PRD.md (Lines 62-77) - 大額消費評估頁功能定義
- docs/PRD.md (Lines 35-59) - 大額消費定義與判定條件

### External References

- #file:../research/20260107-before-spend-evaluation-research.md - 消費評估功能技術研究
- #file:../research/20260107-vue3-pinia-firebase-setup-research.md - Vue 3 與 Pinia 基礎架構

### Standards References

- Vue 3 表單處理文件 - 響應式表單最佳實踐
- Pinia Getters 文件 - 狀態計算邏輯

## Implementation Checklist

### [ ] Phase 1: 評估表單 UI

- [ ] Task 1.1: 建立評估頁面組件
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 9-25)

- [ ] Task 1.2: 實作表單輸入欄位
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 27-45)

- [ ] Task 1.3: 加入表單驗證
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 47-63)

### [ ] Phase 2: 計算邏輯實作

- [ ] Task 2.1: 建立 evaluationStore
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 67-85)

- [ ] Task 2.2: 實作延長時間計算函式
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 87-104)

- [ ] Task 2.3: 整合即時計算功能
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 106-121)

### [ ] Phase 3: 結果顯示組件

- [ ] Task 3.1: 建立評估結果組件
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 125-142)

- [ ] Task 3.2: 實作結果格式化顯示
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 144-159)

- [ ] Task 3.3: 加入視覺化與提示
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 161-176)

### [ ] Phase 4: 資料儲存功能

- [ ] Task 4.1: 實作「確認消費」按鈕
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 180-196)

- [ ] Task 4.2: 寫入 Firestore largeExpenses
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 198-215)

- [ ] Task 4.3: 更新債務計畫資料
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 217-232)

### [ ] Phase 5: UX 優化

- [ ] Task 5.1: 加入大額消費說明
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 236-249)

- [ ] Task 5.2: 實作錯誤處理與提示
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 251-265)

- [ ] Task 5.3: 優化表單互動體驗
  - Details: .copilot-tracking/details/20260107-before-spend-evaluation-details.md (Lines 267-280)

## Dependencies

- Vue 3 專案已建立（20260107-vue3-pinia-firebase-setup）
- Pinia 已設定
- Firebase Firestore 已配置
- expenseStore 已建立
- debtStore 已建立

## Success Criteria

- 表單可正確輸入消費資訊
- 計算邏輯符合 PRD 定義公式
- 基本版與保守版結果同時顯示
- 即時計算功能運作正常
- 確認消費後資料正確寫入 Firestore
- 債務計畫延長時間正確更新
- UI 清晰易懂，符合設計原則
- 表單驗證與錯誤處理完整
