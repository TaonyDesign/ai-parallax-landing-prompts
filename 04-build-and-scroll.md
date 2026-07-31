# Step 4｜寫成會動的頁面

工具：Claude Code、Figma API、ffmpeg

這一步你的角色從設計師變成驗收的人。技術棧是 React + TypeScript + Vite + Tailwind，捲動用 GSAP ScrollTrigger 搭 Lenis

## 4-1 用 Figma 留言當規格書

你平常怎麼跟工程師溝通，現在就怎麼跟 AI 溝通。直接在設計稿上留言：「這裡要爆炸」「這邊捲動的時候拆開」，然後讓 AI 一次讀出來

問題是預設的 Figma MCP 讀不到留言，要走 API

### 拿 token

Figma 右上角頭像 → Settings → Security → Personal access tokens → Generate new token

Scopes 至少勾這兩項：

- `file_comments:read`（讀留言）
- `file_content:read`（讀檔名和圖層，留言才對得回具體元件）

要讓 AI 幫你回留言的話，comments 那項也一起開寫入權限

### 給 Claude Code 的指令

Token 產生後複製，連同檔案連結交給 Claude：

```
這是我的 Figma personal access token：<貼上 token>
這是設計檔連結：<貼上 Figma 檔案連結>

Figma MCP 讀不到留言，請直接打 REST API 把這個檔案的所有留言抓下來：
GET https://api.figma.com/v1/files/<file key>/comments
header 帶 X-Figma-Token

然後把留言整理成一份「效果規格清單」：
每一條標出釘在哪個畫板／哪個圖層、要做什麼效果、觸發條件是什麼。
讀不出對應圖層的留言另外列一區，不要猜。
```

十幾條散在設計稿上的留言，一個指令就變成一份規格清單，後續的 coding 階段會方便很多

## 4-2 退件的寫法

這一頁不是一句 prompt 變出來的，是二十六輪的來回：我發規格、它交東西、我驗收，不行就退件

**退件的訣竅：不要只說「我不喜歡」，要講清楚哪裡不對**

實際退過的一句長這樣：

> 放大之後圓點裡像開了黑洞，看起來像兩張圖疊在一起

它就照這個方向重做出第二版。有效的退件描述有三個成分：**哪個狀態**（放大之後）、**看到什麼**（圓點裡像開了黑洞）、**像什麼**（兩張圖疊在一起）

第三個成分最有用，AI 需要一個具象的錯誤模型才知道要往哪修

這個過程就是設計師每天在做的事：出規格、看成品、給回饋。只是對象從工程師變成了 AI

## 4-3 捲動控制影片：拆幀

看起來最難的效果，原理意外簡單。捲動的時候零件一件一件拆開，**其實它就是一段影片**

用 ffmpeg 一行指令把影片切成一張張圖，捲動位置決定現在顯示第幾張。你捲它就播，你停它就停，你往回捲它就倒帶

```bash
ffmpeg -i explosion.mp4 -vf "scale=1600:-1:flags=lanczos" frames/frame_%03d.png
```

轉成 WebP 壓體積（大部分 Mac 上的 ffmpeg 沒編入 libwebp，所以分成兩段跑最保險）：

```bash
brew install webp
for f in frames/*.png; do cwebp -q 85 "$f" -o "${f%.png}.webp"; done
```

10 秒 30fps 大約 300 張，總量抓 10 到 30MB，需要 preload。太大就降到 24fps 或縮小寬度

網頁端把捲動進度映射到幀編號，畫在 canvas 上就完成了

## 4-4 停留點：讓拆解變成講解

拆解不要勻速。設幾個停留點，捲到那裡畫面就停在那個零件上，規格文字跟著進場

這一段的例子設了四個：風扇、CPU、GPU、SSD

作法是把「捲動進度 → 幀編號」的映射改成非線性：停留區間內幀號不變，讓文字有時間進場，過了才繼續推進

這一招把「拆開給你看」變成「一個零件一個零件講給你聽」。Apple 的產品頁用的也是同一個原理，所以捲動軸其實是在講故事

## 4-5 給 AI 的實作規則

發包實作時值得先講清楚的幾條：

```
實作規則：
- 所有捲動參數集中放在一個 constants 檔，不要把數字散進各個區塊
- 視覺數值一律從 Figma 逐值讀出來，不要看截圖臨摹
- 動畫函式庫走 npm 本地安裝，不要掛 CDN
- 每完成一段就給我截圖驗收，不要全部做完再一起看
```

最後一條最重要。動畫驗收要看連續幀不看靜幀，單張截圖看不出抖動和變形，所以要一段一段驗，不要等到最後
