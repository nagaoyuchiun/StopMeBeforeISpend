---
applyTo: ".copilot-tracking/changes/20260107-vue3-pinia-firebase-setup-changes.md"
---

<!-- markdownlint-disable-file -->

# Task Checklist: Vue 3 + Pinia + Firebase 專案建置

## Overview

建立大額消費影響還債年限系統的完整技術基礎架構，包含 Vue 3 前端、Pinia 狀態管理、Firebase 認證與資料庫整合。

## Objectives

- 建立可運行的 Vue 3 + Vite 專案基礎
- 整合 Pinia 狀態管理架構
- 配置 Firebase Authentication 與 Firestore
- 實作單一使用者認證機制
- 建立符合 PRD 需求的資料模型

## Research Summary

### Project Files

- docs/PRD.md - 產品需求定義，技術架構需求

### External References

- #file:../research/20260107-vue3-pinia-firebase-setup-research.md - Vue 3 + Pinia + Firebase 技術棧整合研究

### Standards References

- Vue 3 官方文件 - 現代化前端框架最佳實踐
- Pinia 官方文件 - Vue 3 官方狀態管理方案
- Firebase 官方文件 - Backend-as-a-Service 整合指南

## Implementation Checklist

### [ ] Phase 1: 專案初始化

- [ ] Task 1.1: 建立 Vue 3 專案基礎
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 9-24)

- [ ] Task 1.2: 安裝核心依賴
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 26-41)

- [ ] Task 1.3: 配置專案結構
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 43-62)

### [ ] Phase 2: Firebase 整合

- [ ] Task 2.1: 配置 Firebase 專案
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 66-82)

- [ ] Task 2.2: 建立 Firebase 配置檔案
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 84-103)

- [ ] Task 2.3: 實作 Firebase 服務模組
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 105-123)

### [ ] Phase 3: Pinia 狀態管理

- [ ] Task 3.1: 設定 Pinia
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 127-143)

- [ ] Task 3.2: 建立 authStore
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 145-165)

- [ ] Task 3.3: 建立 debtStore
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 167-185)

- [ ] Task 3.4: 建立 expenseStore
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 187-205)

### [ ] Phase 4: 路由與認證流程

- [ ] Task 4.1: 設定 Vue Router
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 209-226)

- [ ] Task 4.2: 實作路由守衛
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 228-244)

- [ ] Task 4.3: 建立登入頁面
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 246-262)

### [ ] Phase 5: 開發環境與安全性

- [ ] Task 5.1: 配置環境變數
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 266-280)

- [ ] Task 5.2: 設定 Firebase Security Rules
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 282-298)

- [ ] Task 5.3: 配置 .gitignore
  - Details: .copilot-tracking/details/20260107-vue3-pinia-firebase-setup-details.md (Lines 300-312)

## Dependencies

- Node.js 18+ 與 npm
- Firebase 專案 (需手動建立)
- Vue 3.4+
- Pinia 2.1+
- Firebase SDK 10.8+
- Vite 5.1+

## Success Criteria

- `npm run dev` 成功啟動開發伺服器
- Firebase 連線正常，無 console 錯誤
- 可成功登入並維持認證狀態
- Pinia stores 正常運作
- 路由守衛正確保護需認證頁面
- 環境變數正確載入
- Firestore 資料讀寫測試通過
