---
applyTo: ".copilot-tracking/changes/20260107-after-spend-dashboard-changes.md"
---

<!-- markdownlint-disable-file -->

# Task Checklist: 實際大額消費影響儀表板

## Overview

實作消費後影響追蹤儀表板，顯示原預計清償年月、新預估清償年月、累計延長月數、累計大額消費金額等核心指標，並提供消費記錄列表與時間軸視覺化。

## Objectives

- 建立儀表板主頁面與核心指標卡片
- 實作累計計算邏輯（延長月數、消費金額）
- 顯示大額消費記錄列表
- 實作時間軸視覺化
- 整合 Firestore 即時監聽機制
- 提供清晰的資料呈現與 UX 優化

## Research Summary

### Project Files

- docs/PRD.md (Lines 80-87) - 實際大額消費影響儀表板功能定義
- docs/PRD.md (Lines 35-59) - 大額消費定義

### External References

- #file:../research/20260107-after-spend-dashboard-research.md - 消費後儀表板技術研究
- #file:../research/20260107-vue3-pinia-firebase-setup-research.md - Vue 3 與 Firestore 基礎

### Standards References

- Firestore 即時監聽文件 - Real-time data synchronization
- Pinia Store Composition - 跨 store 資料組合

## Implementation Checklist

### [ ] Phase 1: 資料層擴充

- [ ] Task 1.1: 擴充 debtStore 功能
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 9-26)

- [ ] Task 1.2: 擴充 expenseStore 功能
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 28-47)

- [ ] Task 1.3: 建立 dashboardStore
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 49-67)

### [ ] Phase 2: 核心指標顯示

- [ ] Task 2.1: 建立儀表板主頁面
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 71-87)

- [ ] Task 2.2: 建立核心指標卡片組件
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 89-108)

- [ ] Task 2.3: 實作日期格式化工具
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 110-126)

### [ ] Phase 3: 消費記錄列表

- [ ] Task 3.1: 建立消費記錄列表組件
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 130-148)

- [ ] Task 3.2: 實作記錄項目顯示
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 150-167)

- [ ] Task 3.3: 加入排序功能
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 169-184)

### [ ] Phase 4: 時間軸視覺化

- [ ] Task 4.1: 建立時間軸組件
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 188-205)

- [ ] Task 4.2: 實作進度條視覺化
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 207-223)

- [ ] Task 4.3: 標記重要時間節點
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 225-240)

### [ ] Phase 5: 即時更新與優化

- [ ] Task 5.1: 整合 Firestore 即時監聽
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 244-262)

- [ ] Task 5.2: 實作 Loading 與空狀態
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 264-280)

- [ ] Task 5.3: 錯誤處理與 UX 優化
  - Details: .copilot-tracking/details/20260107-after-spend-dashboard-details.md (Lines 282-298)

## Dependencies

- Vue 3 專案已建立
- Pinia stores (auth, debt, expense) 已建立
- Firebase Firestore 已配置
- debtPlans collection 有資料
- 消費評估功能已實作（可產生 largeExpenses 資料）

## Success Criteria

- 儀表板正確顯示四個核心指標
- 累計計算邏輯正確
- 消費記錄列表正常運作
- 時間軸視覺化清晰易懂
- Firestore 即時監聽正常同步資料
- 空狀態與 Loading 狀態友善
- 錯誤處理完整
- 日期格式化正確（zh-TW）
- 響應式設計適配手機與桌面
