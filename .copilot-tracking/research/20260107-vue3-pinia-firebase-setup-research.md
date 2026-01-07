<!-- markdownlint-disable-file -->

# Research: Vue 3 + Pinia + Firebase 專案建置

**Research Date**: 2026-01-07
**Task**: 建立大額消費影響還債年限系統的技術基礎架構

## 研究來源

### 專案需求文件
- docs/PRD.md - 產品需求定義，明確技術棧為 Vue 3 + Pinia + Firebase

## 技術棧研究

### Vue 3 專案初始化

**官方推薦方式**:
- 使用 `npm create vue@latest` 初始化專案
- 預設使用 Vite 作為建置工具
- TypeScript 支援可選

**專案結構最佳實踐**:
```
project-root/
├── src/
│   ├── assets/          # 靜態資源
│   ├── components/      # 可複用組件
│   ├── views/           # 頁面組件
│   ├── stores/          # Pinia stores
│   ├── services/        # API 服務層
│   ├── firebase/        # Firebase 配置
│   ├── router/          # Vue Router
│   ├── App.vue          # 根組件
│   └── main.js          # 入口文件
├── public/              # 公開資源
└── package.json
```

### Pinia 狀態管理

**安裝與設定**:
```bash
npm install pinia
```

**基本設定** (main.js):
```javascript
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const pinia = createPinia()
const app = createApp(App)

app.use(pinia)
app.mount('#app')
```

**Store 架構設計** (基於 PRD 需求):
- `useAuthStore` - Firebase 認證狀態
- `useDebtStore` - 債務計畫資料
- `useExpenseStore` - 大額消費記錄

### Firebase 整合

**SDK 安裝** (v9+ modular SDK):
```bash
npm install firebase
```

**Firebase 配置結構** (src/firebase/config.js):
```javascript
import { initializeApp } from 'firebase/app'
import { getAuth } from 'firebase/auth'
import { getFirestore } from 'firebase/firestore'

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
  appId: import.meta.env.VITE_FIREBASE_APP_ID
}

const app = initializeApp(firebaseConfig)
const auth = getAuth(app)
const db = getFirestore(app)

export { auth, db }
```

### Firebase Auth - 單一使用者配置

**認證方式選擇**:
- Email/Password - 最簡單的單一使用者認證
- 可考慮持久化登入狀態

**實作模式**:
```javascript
import { signInWithEmailAndPassword, onAuthStateChanged } from 'firebase/auth'
import { auth } from '@/firebase/config'

// 登入
export const login = async (email, password) => {
  return await signInWithEmailAndPassword(auth, email, password)
}

// 監聽認證狀態
export const initAuthListener = (callback) => {
  return onAuthStateChanged(auth, callback)
}
```

### Firestore 資料模型設計

**基於 PRD 需求的 Collections**:

#### 1. users collection
```javascript
{
  uid: string,
  email: string,
  createdAt: timestamp
}
```

#### 2. debtPlans collection
```javascript
{
  userId: string,
  totalDebt: number,
  monthlyExtraPayment: number,      // E_base (預設 10000)
  safeMonthlyExtraPayment: number,  // E_safe (預設 8000)
  originalPayoffDate: timestamp,
  currentPayoffDate: timestamp,
  createdAt: timestamp,
  updatedAt: timestamp
}
```

#### 3. largeExpenses collection
```javascript
{
  userId: string,
  amount: number,
  category: string,
  motivation: string,              // 選填
  delayMonths: number,             // 延長月數
  delayMonthsSafe: number,         // 保守版延長月數
  spentAt: timestamp,
  createdAt: timestamp
}
```

### 環境變數配置

**檔案**: `.env.local`
```
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
```

**注意事項**:
- `.env.local` 加入 `.gitignore`
- 提供 `.env.example` 作為範本

## 開發工具與依賴

### 必要套件
```json
{
  "dependencies": {
    "vue": "^3.4.0",
    "pinia": "^2.1.0",
    "vue-router": "^4.3.0",
    "firebase": "^10.8.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.0.0",
    "vite": "^5.1.0"
  }
}
```

### 開發指令
- `npm run dev` - 啟動開發伺服器
- `npm run build` - 生產環境建置
- `npm run preview` - 預覽建置結果

## 實作順序建議

### Phase 1: 專案基礎建置
1. 使用 create-vue 初始化專案
2. 安裝 Pinia 和 Firebase SDK
3. 設定 Firebase 配置檔案
4. 配置環境變數

### Phase 2: 認證機制
1. 建立 Firebase Auth 服務模組
2. 建立 authStore (Pinia)
3. 實作登入/登出功能
4. 設定路由守衛

### Phase 3: 資料層準備
1. 定義 Firestore collections 結構
2. 建立 Firestore 服務模組
3. 建立 debtStore 和 expenseStore
4. 實作基本 CRUD 操作

### Phase 4: 基本 UI 框架
1. 建立路由結構
2. 建立基本版面組件
3. 整合認證流程到 UI

## 安全性考量

### Firebase Rules 基礎設定
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    match /debtPlans/{planId} {
      allow read, write: if request.auth != null && 
        resource.data.userId == request.auth.uid;
    }
    match /largeExpenses/{expenseId} {
      allow read, write: if request.auth != null && 
        resource.data.userId == request.auth.uid;
    }
  }
}
```

## 參考資源

- Vue 3 官方文件: https://vuejs.org/
- Pinia 官方文件: https://pinia.vuejs.org/
- Firebase 官方文件: https://firebase.google.com/docs
- Vite 官方文件: https://vitejs.dev/

## 研究結論

技術棧選擇合理且成熟：
- Vue 3 提供現代化響應式框架
- Pinia 提供簡潔的狀態管理
- Firebase 提供快速的後端服務
- Vite 提供高效的開發體驗

建議採用標準的 Vue 3 + Vite 專案結構，使用 Firebase v9+ modular SDK，並遵循單一使用者認證模式。
