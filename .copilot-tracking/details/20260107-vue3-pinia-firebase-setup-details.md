<!-- markdownlint-disable-file -->

# Task Details: Vue 3 + Pinia + Firebase 專案建置

## Research Reference

**Source Research**: #file:../research/20260107-vue3-pinia-firebase-setup-research.md

## Phase 1: 專案初始化

### Task 1.1: 建立 Vue 3 專案基礎

使用 create-vue 官方工具初始化專案，選擇必要的功能選項。

- **Files**:
  - package.json - 專案配置與依賴管理
  - vite.config.js - Vite 建置工具配置
  - src/main.js - 應用程式入口
  - src/App.vue - 根組件
- **Success**:
  - 專案目錄結構建立完成
  - `npm run dev` 可成功啟動
  - 瀏覽器可開啟預設頁面
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 11-33) - Vue 3 專案初始化方式
- **Dependencies**:
  - Node.js 18+ 已安裝

### Task 1.2: 安裝核心依賴

安裝 Pinia、Firebase SDK 與其他必要套件。

- **Files**:
  - package.json - 新增依賴項目
- **Success**:
  - pinia 2.1+ 安裝成功
  - firebase 10.8+ 安裝成功
  - vue-router 4.3+ 安裝成功
  - node_modules 目錄建立
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 35-49) - Pinia 安裝設定
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 51-60) - Firebase SDK 安裝
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 148-161) - 必要套件清單
- **Dependencies**:
  - Task 1.1 完成

### Task 1.3: 配置專案結構

建立符合最佳實踐的目錄結構，為後續開發做準備。

- **Files**:
  - src/stores/ - Pinia stores 目錄
  - src/services/ - API 服務層目錄
  - src/firebase/ - Firebase 配置目錄
  - src/views/ - 頁面組件目錄
  - src/components/ - 可複用組件目錄
  - src/router/ - Vue Router 配置目錄
- **Success**:
  - 所有目錄建立完成
  - 目錄結構符合研究文件建議
  - 加入適當的 .gitkeep 檔案
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 15-33) - 專案結構最佳實踐
- **Dependencies**:
  - Task 1.2 完成

## Phase 2: Firebase 整合

### Task 2.1: 配置 Firebase 專案

在 Firebase Console 建立新專案並取得配置參數。

- **Files**:
  - README.md - 記錄 Firebase 專案設定步驟
- **Success**:
  - Firebase 專案建立完成
  - Web App 已註冊
  - 取得 Firebase 配置物件
  - Firestore 資料庫已啟用
  - Authentication Email/Password 已啟用
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 51-79) - Firebase 配置結構
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 81-103) - Firebase Auth 單一使用者配置
- **Dependencies**:
  - Firebase 帳號

### Task 2.2: 建立 Firebase 配置檔案

建立 Firebase SDK 初始化檔案，使用環境變數管理敏感資訊。

- **Files**:
  - src/firebase/config.js - Firebase 初始化配置
  - .env.local - 環境變數檔案（不提交到 git）
  - .env.example - 環境變數範本
- **Success**:
  - Firebase app 初始化成功
  - auth 與 db 物件正確匯出
  - 環境變數正確載入
  - 無 console 錯誤
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 61-79) - Firebase 配置結構範例
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 132-146) - 環境變數配置
- **Dependencies**:
  - Task 2.1 完成
  - Task 1.3 完成

### Task 2.3: 實作 Firebase 服務模組

建立 Firebase 認證與 Firestore 操作的服務層。

- **Files**:
  - src/services/auth.js - 認證服務
  - src/services/firestore.js - Firestore 操作服務
- **Success**:
  - 登入/登出函式可用
  - 認證狀態監聽器可用
  - Firestore CRUD 基礎函式可用
  - 服務模組可正常匯入
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 81-103) - Firebase Auth 實作模式
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 105-130) - Firestore 資料模型設計
- **Dependencies**:
  - Task 2.2 完成

## Phase 3: Pinia 狀態管理

### Task 3.1: 設定 Pinia

在應用程式中註冊 Pinia，啟用狀態管理功能。

- **Files**:
  - src/main.js - 註冊 Pinia plugin
- **Success**:
  - Pinia 成功註冊到 Vue app
  - 應用程式正常啟動
  - Vue DevTools 可看到 Pinia stores
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 35-49) - Pinia 基本設定
- **Dependencies**:
  - Task 1.2 完成

### Task 3.2: 建立 authStore

建立管理使用者認證狀態的 Pinia store。

- **Files**:
  - src/stores/auth.js - 認證狀態管理
- **Success**:
  - user 狀態定義完成
  - login/logout actions 實作完成
  - 認證狀態監聽器整合
  - 可在組件中使用 useAuthStore
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 35-49) - Pinia Store 架構設計
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 81-103) - Firebase Auth 整合
- **Dependencies**:
  - Task 3.1 完成
  - Task 2.3 完成

### Task 3.3: 建立 debtStore

建立管理債務計畫資料的 Pinia store。

- **Files**:
  - src/stores/debt.js - 債務計畫狀態管理
- **Success**:
  - 債務計畫狀態定義（符合 PRD 需求）
  - fetchDebtPlan action 實作
  - updateDebtPlan action 實作
  - 計算延長月數的 getter 函式
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 111-119) - debtPlans collection 結構
  - docs/PRD.md (Lines 69-77) - 計算公式與系統參數
- **Dependencies**:
  - Task 3.1 完成
  - Task 2.3 完成

### Task 3.4: 建立 expenseStore

建立管理大額消費記錄的 Pinia store。

- **Files**:
  - src/stores/expense.js - 大額消費狀態管理
- **Success**:
  - 大額消費列表狀態定義
  - fetchExpenses action 實作
  - addExpense action 實作
  - 累計金額與延長月數的 getters
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 121-130) - largeExpenses collection 結構
  - docs/PRD.md (Lines 62-77) - 大額消費評估功能
- **Dependencies**:
  - Task 3.1 完成
  - Task 2.3 完成

## Phase 4: 路由與認證流程

### Task 4.1: 設定 Vue Router

建立應用程式路由配置，定義主要頁面路徑。

- **Files**:
  - src/router/index.js - 路由配置
  - src/views/LoginView.vue - 登入頁面
  - src/views/DashboardView.vue - 主儀表板
  - src/views/EvaluateView.vue - 消費評估頁
- **Success**:
  - Router 成功註冊到 Vue app
  - 路由配置包含所有主要頁面
  - 頁面可透過路由切換
  - 404 fallback 設定完成
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 15-33) - 專案結構包含 router
  - docs/PRD.md (Lines 24-32) - 系統功能總覽
- **Dependencies**:
  - Task 1.2 完成（vue-router 已安裝）

### Task 4.2: 實作路由守衛

建立認證檢查機制，保護需要登入才能訪問的頁面。

- **Files**:
  - src/router/index.js - 加入 beforeEach 守衛
- **Success**:
  - 未登入使用者自動導向登入頁
  - 已登入使用者可訪問受保護頁面
  - 登入後自動導向儀表板
  - 路由守衛邏輯正確運作
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 81-103) - 認證狀態監聽
- **Dependencies**:
  - Task 4.1 完成
  - Task 3.2 完成（authStore）

### Task 4.3: 建立登入頁面

實作使用者登入介面與邏輯。

- **Files**:
  - src/views/LoginView.vue - 登入頁面組件
- **Success**:
  - Email/Password 輸入表單
  - 登入按鈕與處理邏輯
  - 錯誤訊息顯示
  - 登入成功後導向儀表板
  - UI 基本樣式完成
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 81-103) - Firebase Auth 登入實作
- **Dependencies**:
  - Task 4.2 完成

## Phase 5: 開發環境與安全性

### Task 5.1: 配置環境變數

確保 Firebase 配置與敏感資訊透過環境變數管理。

- **Files**:
  - .env.local - 實際環境變數（不提交）
  - .env.example - 環境變數範本
  - .gitignore - 確保 .env.local 被忽略
- **Success**:
  - .env.local 包含所有必要的 Firebase 配置
  - .env.example 提供完整範本
  - .env.local 不會被 git 追蹤
  - import.meta.env 可正確讀取變數
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 132-146) - 環境變數配置範例
- **Dependencies**:
  - Task 2.2 完成

### Task 5.2: 設定 Firebase Security Rules

配置 Firestore 安全規則，確保單一使用者只能存取自己的資料。

- **Files**:
  - firestore.rules - Firestore 安全規則（需在 Firebase Console 設定）
  - README.md - 記錄安全規則設定步驟
- **Success**:
  - 安全規則確保認證檢查
  - userId 驗證規則正確
  - 測試讀寫權限符合預期
  - 規則已部署到 Firebase
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 178-198) - Firebase Rules 基礎設定
- **Dependencies**:
  - Task 2.1 完成

### Task 5.3: 配置 .gitignore

確保敏感檔案與建置產物不被版本控制追蹤。

- **Files**:
  - .gitignore - Git 忽略規則
- **Success**:
  - node_modules/ 被忽略
  - dist/ 被忽略
  - .env.local 被忽略
  - IDE 配置檔案被忽略
  - .gitignore 包含 Vue/Vite 標準規則
- **Research References**:
  - #file:../research/20260107-vue3-pinia-firebase-setup-research.md (Lines 132-146) - 環境變數安全性考量
- **Dependencies**:
  - Task 1.1 完成

## Dependencies

- Node.js 18+
- npm
- Firebase 專案
- Vue 3.4+
- Pinia 2.1+
- Firebase SDK 10.8+

## Success Criteria

- 完整的 Vue 3 專案可成功運行
- Firebase 整合無錯誤
- 使用者可成功登入/登出
- Pinia stores 正常管理狀態
- 路由守衛正確保護頁面
- 環境變數正確配置
- 安全規則已設定
