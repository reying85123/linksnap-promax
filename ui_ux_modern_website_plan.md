# 現代 UI/UX 網站建構學習計畫：LinkSnap (QR Code Pro Max 實戰)

本計畫旨在透過一個實際案例，引導您掌握如何運用 AI Agent (如 Gemini/ChatGPT) 的「UI/UX Pro Max」能力，從零打造一個符合 2024+ 設計趨勢的現代化網站。

---

## 🎯 實戰案例：LinkSnap (QR Code 生成與管理)

我們將建立一個現代化的 **QR Code 生成與分享工具**。
*   **核心功能**：即時生成 QR Code、圖片分享/下載、**歷史紀錄 (瀏覽器暫存)**。
*   **設計風格**：Cyberpunk/Neon (賽博龐克霓虹)、Glassmorphism (毛玻璃)、Input Focus (輸入框為核心)。
*   **技術堆疊**：Vue 3, TypeScript, Vite, Tailwind CSS, @vueuse/core (互動), qrcode.vue (二維碼)。
*   **資料儲存**：使用瀏覽器的 `localStorage` 進行持久化儲存 (無需後端)。

---

## 🛠️ 準備工作：環境建置與 Agent 啟動

由於本專案的核心目標是學習 **UI/UX Pro Max Skill**，請務必先完成以下設定，讓 AI 進入正確的角色模式。

### 步驟 0.1：建立專案環境
請在您的電腦上執行以下命令建立基礎環境：

```bash
# 1. 建立 Vite + Vue 專案 (TypeScript)
yarn create vite linksnap-promax --template vue-ts

# 2. 進入資料夾
cd linksnap-promax

# 3. 安裝 Tailwind CSS v4 (Vite 流程)
yarn add -D tailwindcss @tailwindcss/vite

# 4. 安裝動畫庫與圖標庫
yarn add @vueuse/motion @vueuse/core qrcode.vue lucide-vue-next clsx tailwind-merge
```

### 步驟 0.1.1：設定 Tailwind CSS v4
在 Vite 專案中使用 Tailwind v4，必須修改 `vite.config.ts` 以及 CSS 進入點：

1. **修改 `vite.config.ts`**：
   ```typescript
   import { defineConfig } from 'vite'
   import vue from '@vitejs/plugin-vue'
   import tailwindcss from '@tailwindcss/vite'

   export default defineConfig({
     plugins: [vue(), tailwindcss()],
   })
   ```

2. **修改 CSS 檔案** (例如 `src/style.css`)：
   全選並清空原內容，加入以下 Tailwind v4 的引入語法：
   ```css
   @import "tailwindcss";
   ```

```bash
# 5. 啟動開發伺服器
yarn dev
```

### 步驟 0.2：啟動 UI/UX Pro Max Agent
使用 `uipro-cli` 快速注入 Agent 設定檔 (針對 Antigravity)：

```bash
# 1. 安裝 CLI 工具 (若尚未安裝)
npm install -g uipro-cli

# 2. 初始化 Agent (確保您在專案資料夾內)
uipro init --ai antigravity
```

**協作循環 (The Loop):**
1. **複製 Prompt** -> 2. **AI 生成代碼** -> 3. **貼入專案** -> 4. **預覽迭代**

---

## 階段一：UX 策略與結構 (User Experience Strategy)

在這個階段，我們不寫程式碼，而是要求 Agent 扮演**產品設計師**，定義網站的靈魂。

### 步驟 1.1：定義使用者旅程與結構
**您的指令 (Prompt) 範例：**
> 「你現在是 UI/UX Pro Max Agent。我們要開發一個名為 'LinkSnap' 的 QR Code 生成工具。請幫我規劃 SPA 結構。核心體驗是用戶輸入文字或網址時，QR Code 要『即時流暢』地變化。右側或下方要有一個『儲存的網址 (Saved Links)』區塊。請解釋如何設計 Layout 讓輸入區與預覽區在手機上也能完美呈現。」

**預期學習成果：**
*   理解 **Input-Centric Design** (以輸入框為中心的設計)。
*   學習 **Reactive UI** (響應式介面：輸入變，結果就變)。
*   規劃 **Saved Links Sidebar** (儲存網址側邊欄) 的 UX，包含手動儲存與刪除功能。

---

## 階段二：UI 視覺設計系統 (Visual Design System)

現代網站的質感來自於統一的「設計系統」。我們要求 Agent 定義色彩、字體與間距。

### 步驟 2.1：生成現代化配色與字體
**您的指令 (Prompt) 範例：**
> 「請為 LinkSnap 建立 Design System，並使用 Tailwind CSS v4 的 `@theme` 語法在 CSS 檔案中配置。風格要求：深色模式 (Dark Mode)，主色調使用 'Neon Cyan' (#06b6d4) 搭配 'Hot Pink' (#ec4899) 作為強調色。請提供完整的 CSS 設定碼，包含霓虹光暈 (Glow) 的 box-shadow 設定與現代無襯線字體。」

### 步驟 2.2：定義元件風格 (Glassmorphism & Borders)
**您的指令 (Prompt) 範例：**
> 「請定義一個 'Input Field' (輸入框) 的樣式。常態下是半透明毛玻璃，Focus 時要有強烈的霓虹邊框光暈動畫。請提供 Tailwind Utility Class 組合。」

---

## 階段三：核心元件開發 (Component Implementation)

進入程式碼實作，我們將使用 **Bento Grid (便當盒佈局)**，這是目前最流行的 UI 趨勢。

### 步驟 3.1：開發核心輸入區 (The Magic Input)
**您的指令 (Prompt) 範例：**
> 「請使用 Vue 3 (Script Setup + TypeScript) 和 Tailwind CSS 開發核心區塊。畫面中央是一個巨大的輸入框 (Textarea)，支援多行輸入。當使用者輸入內容時，使用 `qrcode.vue` 在下方即時渲染 QR Code。請為 QR Code 加上一個漂亮的毛玻璃背景卡片容器。」

### 步驟 3.2：開發結果卡片 (Result Card with QR)
**您的指令 (Prompt) 範例：**
> 「請優化 QR Code 卡片。加入兩個功能按鈕：1. 'Download PNG' (將 Canvas 轉存為圖片)，2. 'Share' (呼叫手機原生分享選單)。請使用 `@vueuse/core` 的 `useShare` 來實作分享功能。」

---

## 階段四：高階互動與動畫 (Interaction & Polish)

這是區分「普通網站」與「Pro Max 網站」的關鍵。

### 步驟 4.1：狀態切換動畫
**您的指令 (Prompt) 範例：**
> 「請使用 `@vueuse/motion` 為 QR Code 加入動畫。當輸入內容改變時，我希望 QR Code 有一個輕微的『呼吸』或『重繪』過場效果，不要閃爍得太厲害。」

### 步驟 4.2：下載按鈕的微互動
**您的指令 (Prompt) 範例：**
> 「請為 'Download' 按鈕加入微互動。點擊時按鈕要縮小 (Scale down)，下載完成後顯示一個綠色的勾勾圖示，2秒後恢復原狀。」

---

## 階段五：文案與最終檢查 (Copywriting & Review)

UI 再好，如果文案生硬就沒有靈魂。

### 步驟 5.1：UX Writing (使用者體驗文案)
**您的指令 (Prompt) 範例：**
> 「請將介面上所有的文字內容預設採用『一般常用且平易近人的繁體中文』。雖然整體的視覺風格是 Cyberpunk 的科技感，但文字說明應保持清晰直白，確保一般使用者都能直覺理解操作，不要使用過於生硬的字詞。」

---

## 階段七：部署到 GitHub Pages (Deployment)

完成開發後，我們將這個純前端專案託管到 GitHub Pages 上，作為您的作品集展示。

### 步驟 7.1：設定 Vite 與自動化部署
**您的指令 (Prompt) 範例：**
> 「我要將這個 Vue 專案部署到 GitHub Pages。請告訴我如何修改 `vite.config.ts` 的 `base` 設定 (假設我的 GitHub Repository 名稱是 'linksnap-promax')。另外，請幫我寫一個 `.github/workflows/deploy.yml` 檔案，讓我在 Push code 到 master 分支時，GitHub Actions 能自動打包並部署到 gh-pages 分支。」

---

##  Pro Max 技巧總結

1.  **Context is King**: 在 Prompt 中始終強調「現代風格」、「深色模式」、「互動性」。
2.  **Iterate (迭代)**: 不要期望一次完美。先生成結構，再要求「讓它更漂亮」、「加入動畫」。
3.  **Ask for "Why"**: 詢問 Agent 為什麼這樣設計，這能幫助您學習設計思考。

祝您建構愉快！