# 「一人設計團隊：高級動態網頁設計」  Prompt 整理

影片《從 0 到 1，打造高級動態網頁設計》裡用到的每一段 prompt，整理在這裡，直接複製拿去用

- 成品頁面：https://taonydesign.com/steam-machine-demo/ （桌機開，1440px 以上看得最完整）

一個人用四個步驟、七個 AI 工具，做完過去要一整個團隊才做得完的事。這四個步驟其實就是一間設計 agency 的四個角色

| 步驟 | 角色 | 工具 | Prompt |
|---|---|---|---|
| 1 | 產品經理 | Claude Code | [01-brief-and-prd.md](01-brief-and-prd.md) |
| 2 | 設計師 | Claude Code、Figma、Google Stitch | [02-design-direction.md](02-design-direction.md) |
| 3 | 素材團隊 | Meshy、Google Flow、Gemini、Higgsfield | [03-ai-assets.md](03-ai-assets.md) |
| 4 | 前端工程師 | Claude Code | [04-build-and-scroll.md](04-build-and-scroll.md) |


## 如何使用這份文檔

這裡的 prompt 全部是原版，沒有抽象化成填空模板。因為 prompt 的威力就在具體，`[你的產品] is a [形狀]` 這種模板誰都寫得出來，但它換不到能用的東西

所以每一段的用法是：**先看「為什麼這樣寫」搞懂哪些句子是骨架，再照「換成你的專案」把血肉換掉**。骨架是通用的，血肉是這台主機的

三個貫穿全部步驟的原則：

**一句 prompt 換不到一整個網頁。** 這一頁前後迭代了二十六輪，是「我發規格、AI 交東西、我驗收、不行就退件」的來回

**prompt 不用全部自己寫。** 卡住的時候，去問 Claude 或 ChatGPT「幫我寫一段給某某工具的 prompt」，複製過來再改

**AI 負責讓你少走冤枉路，設計還是你的手藝。** 所有 AI 產出的視覺都只是「方向」，最終成稿建議自己在 Figma 完成


## 授權與聲明

這些 prompt 你可以隨意取用、改寫、商用，註明出處即可

這是設計師視角的 concept redesign 演示，非 Valve 官方內容，與 Valve Corporation 無關
Steam、Steam Machine 及相關商標為 Valve Corporation 所有

---

更多 UIUX 設計資訊

- 免費電子報：https://taonydesign.beehiiv.com/
- Threads：https://www.threads.com/@uiux.taony
