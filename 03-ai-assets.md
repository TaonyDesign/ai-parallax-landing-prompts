# Step 3｜AI 生素材：3D 模型、爆炸拆解、產品影片

工具：Figma AI、Meshy、Google Flow、Gemini、Higgsfield

這一步最容易翻車，也最花時間。整段最重要的一招寫在最後面的「首尾幀錨定」，卡住的時候直接跳去看

## 3-1 圖片轉 3D 模型（Meshy）

Hero 那台會跟著滑鼠轉動的 CRT 主機，是從一張圖生成的 3D 模型

### 先改圖，再丟 Meshy

**把螢幕裡的畫面塗掉，改成純深藍黑的空白螢幕。** 兩個理由：

1. 畫面烤進貼圖之後就是一張死圖，做不了發光效果，之後也換不掉。空白螢幕生出來會是乾淨的平面，可以在網頁裡貼任何內容並讓它發光
2. 版權。原圖螢幕裡如果是別人的遊戲畫面，那是有著作權的素材，要公開發布就塗掉最省事

塗 `#1a2b4a` 左右的深藍或純黑都行，保留螢幕外框那圈深色邊，那是機身的一部分

想要更好的 3D 品質，用 Figma 內建的 AI 功能把頂部、底部、左右四個角度的圖都生出來匯出，Meshy 選「多檢視」模式一次餵四張

### Meshy 設定

| 設定 | 選 | 為什麼 |
|---|---|---|
| Symmetry | Off | 機殼左右對稱，但搖桿在左、按鈕在右。開對稱會把搖桿鏡射到兩邊 |
| Topology | Quad | 硬表面產品，四邊面的邊角比三角面乾淨 |
| Target Polycount | 先拉高（100k 以上） | 生成階段留細節，之後在網頁端減面。一開始就壓低，倒角和按鈕會糊掉 |
| PBR Maps | 開 | 要 roughness 貼圖才有塑膠的霧面質感，關掉會像玩具 |

貼圖解析度 4K 就夠用。下載選 **GLB**，這是讓 Claude Code 做 3D 動畫最好用的格式

### 提示詞（英文，直接貼）

```
Retro 1980s tabletop arcade console, single molded beige ABS plastic body
in warm ivory tone, matte finish with fine surface grain. Boxy CRT monitor
housing tilted slightly upward, deep recessed rectangular screen with dark
navy inner bezel, blank dark screen. Grey accent panel on the rear right of
the monitor shell. Circular perforated speaker grille on the right side
panel. Front control deck with a white ball-top joystick on the left, two
large round arcade buttons in white and cream on the right, and a row of
four small rectangular buttons (one red, three grey) above them. Recessed
cartridge slot at the front base with a cartridge inserted. Embossed text
labels on the housing. Clean hard-surface edges, chamfered corners, product
render quality, neutral studio lighting, plain background.
```

### 拿到模型後檢查四個地方

硬表面模型最容易翻車的就是這四處，轉一圈看：

1. **搖桿的桿子**：最細的部件，容易斷或黏成一坨。這裡糊掉就重生成
2. **四顆小方鈕**：容易糊成一條長方形
3. **喇叭孔**：可能整片變成平面貼圖而不是真的凹孔，這其實可以接受
4. **背面**：參考圖是四分之三視角，AI 要用猜的，通常猜得還行

第 1 點以外的三點，在 Hero 的尺寸下多半看不出來

## 3-2 爆炸拆解靜態圖（Google Flow）

先給一張產品參考圖，再下指令。開頭那句「以附圖為準」是防止 AI 過度發揮想像力的關鍵

```
Use the attached photos as the exact reference for the machine's exterior design, proportions and materials. Generate an exploded view of THIS machine.
```

接主體：

```
Create a photorealistic exploded view of a compact matte-black cube gaming console, fully disassembled with every hardware component floating in mid-air, arranged along a single straight diagonal axis from the lower-left to the upper-right of the frame.

Along the axis: the front panel with its thin green LED light bar still glowing, a large cooling fan with visible blades, a copper heat-pipe heatsink with thin vertical fins, the mainboard carrying a CPU with its silicon die exposed and a GPU surrounded by VRAM chips, and an M.2 NVMe SSD stick, all wrapped by the outer cube chassis shell split into top and bottom halves.

Each component hovers evenly spaced with subtle depth of field, as if frozen mid-assembly. Studio product photography, soft even top-down key light with gentle fill from camera-left, subtle green accent glow from the LEDs on nearby components, #111111 dark charcoal background, extreme detail on PCB traces, fan blades and heatsink fins, centered composition with generous negative space, no text, no labels, no watermark, 8K render.
```

出圖後再用對話微調，例如「把 IO 面板移除」「散熱器改成銀色」，一次改一件事

## 3-3 爆炸拆解影片（Higgsfield）

靜態圖沒問題不代表動態沒問題。第一次跑的時候盒子打開後機殼變形了，這是這類生成最常見的翻車點

修法是把「物理限制」寫進 prompt：每個零件都是剛體、只位移不變形、立方體從頭到尾都是立方體。下面這版就是修過的

```text
Scene: A matte-black compact gaming PC disassembles into an exploded view with brisk, precise mechanical motion, energetic but always controlled. Every internal component separates one after another along a single straight diagonal axis running from the lower-left to the upper-right of the frame, matching the arrangement of the end image. Each component slides outward along this axis and decelerates crisply into its final floating position.

First frame: The fully assembled cube-shaped PC, matte black finish, thin green LED light bar along the bottom front panel, two USB ports and a circular power button with green indicator below — centered in frame against a #111111 dark charcoal background, identical to the start image.

Disassembly sequence (quick and rhythmic — each step takes roughly one second and flows into the next without pause; all components travel along the same diagonal axis, with deliberate exceptions in steps 1 and 2):
1. Four small black screws at the corners of the chassis unscrew themselves, spinning rapidly as they back out of their holes, then drift away and exit the frame. Each screw keeps its exact shape and size while spinning — no melting or flickering.
2. The outer chassis shell splits along its seams — the top half lifts straight upward and the bottom half drops straight down, both moving in pure straight-line translation as perfectly rigid bodies. This step stays smooth and deliberate, slightly slower than the rest. Both halves keep their exact cubic proportions throughout: every face stays flat, every edge stays straight, and the rear face remains a perfect square at all times. No bending, squashing, stretching, or warping as the shell opens. Steps 1 and 2 are the only motions not on the diagonal axis; everything after moves strictly along it.
3. The front LED panel snaps free toward the lower-left end of the axis, green LEDs still glowing, exposing the cooling fan behind it.
4. The black cooling fan (exactly as shown in the end image) detaches and slides briskly along the axis into its floating position. The camera holds on the fan for a beat.
5. The copper heatsink (thin vertical fins) detaches and slides along the axis into place behind the fan.
6. The camera micro-pans quickly to feature the CPU — a large square chip seated on the motherboard.
7. The camera micro-pans quickly to feature the GPU with its VRAM chips arrayed around it on the board.
8. The camera settles on the M.2 NVMe SSD as it slides into its final position at the upper-right end of the axis.

Last frame: The fully exploded view — all components arranged in a diagonal line from bottom-left to top-right, each floating in its final position — identical to the end image.

Optics: 47° normal field of view, medium distance, slightly elevated high-angle studio perspective. The camera remains perfectly stable throughout — only smooth slow micro-pans when featuring each component, never shaking.

Lighting: Consistent studio lighting locked throughout — soft even top-down key light with gentle fill from camera-left, no shifts or flickers during the disassembly. The #111111 dark charcoal background stays constant. The green LEDs cast a subtle green accent glow on nearby components.

Physics: Every component is completely rigid — each retains its exact shape, material, texture, and color from start to finish. Components separate cleanly with mechanical weight and smooth deceleration into their resting positions. The chassis halves in particular behave like solid machined metal: their geometry, proportions, and right angles never change during separation — they only translate through space, never deform. The cube silhouette stays a true cube from the first frame to the last.

Audio: Subtle mechanical ambiance — a rapid ratcheting whir as the screws back out, soft servo whirring as each component separates and slides, gentle metallic click at each detachment point, quiet low ambient hum throughout.
```

開著 smart mode 的話，生成前它會把每一步列出來讓你確認品質、比例、長度，確認完再 Approve。長度控制在 10 秒

## 3-4 首尾幀錨定：這一步最重要的一招

改 prompt 只是把翻車機率降低，真正解掉變形的是換方法

**把影片的第一格和最後一格用圖片直接指定死，AI 只負責把中間補起來，變形的空間就小很多**

用法是先生好首圖（組裝完整）和尾圖（爆炸完成），再開首尾幀模式（Frames to Video）生影片。只把圖當一般參考圖附上是鎖不住的，一定要用首尾幀模式

### 實例：頂視轉正面加點亮 LED

想做「捲動的時候從產品頂部視角慢慢轉為正面，然後點亮綠色燈帶」，事先生三張參考圖：①頂視純平面 ②正面燈滅 ③正面燈亮，然後拆成兩段生成，在「正面燈滅」那一幀無縫相接

**Shot A｜頂視轉正面**（首幀＝圖①，尾幀＝圖②）

```
Strictly follow the provided reference images for the product only. Ignore the white background in the reference images — they define the product design, not the scene.

CRITICAL PROPORTIONS: The console is a PERFECT CUBE, like a large die — its height, width, and depth are all exactly equal. It is NOT a flat, wide, or slim set-top box. If the body appears wider than it is tall, the result is wrong.

Matte black, softly rounded edges, completely plain featureless top surface. The front face is one large flat square panel; the only details are inside the recessed strip at the very bottom, from left to right: two identical rectangular USB-A ports side by side, one small thin slot in the center, a tiny unlit status dot, and a plain blank circular power button with NO icon printed on it. There are absolutely no letters, labels, logos, symbols, or HDMI ports anywhere on the device. All lights are OFF.

BACKGROUND: a perfectly uniform, solid, very dark charcoal background, exact hex color #111111 — near black, slightly lighter than the product. No gradient, no vignette, no visible floor line, no texture, no spotlight pool. A subtle soft rim light traces the edges of the console so its black silhouette stays clearly separated from the dark background.

The camera starts directly overhead in a top-down view, then in one slow, smooth, continuous move orbits down and forward, ending in a perfectly straight-on front view matching the second reference image exactly. The product never moves. Low-key studio lighting, no cuts, no zoom, photorealistic. 4 to 5 seconds.
```

負面提示詞（工具有這個欄位才貼）：

```
flat box, wide box, slim set-top box, HDMI port, letter A, printed labels, icons on buttons, power symbol, extra ports, text, logo, rubber feet, white background, bright background
```

**Shot B｜點亮 LED**（首幀＝圖②，尾幀＝圖③）

```
Strictly follow the provided reference images. Do not change the product design, proportions, materials, or port layout in any way.

Locked camera, straight-on front view of the matte black console, identical framing to the reference images. The product is perfectly still and the camera does not move at all.

The shot starts with all lights OFF, matching the first reference image. Then the horizontal green LED light bar, located in the groove between the main body and the bottom strip, smoothly glows to life across its full width with a soft green bloom, and the tiny status dot next to the power button also lights up green, ending in the exact state of the second reference image. The power button is a plain blank circle with no icon; there are no letters or labels anywhere on the device.

BACKGROUND: a perfectly uniform, solid, very dark charcoal background, exact hex color #111111 — near black, slightly lighter than the product. No gradient, no vignette, no visible floor line, no texture, no spotlight pool. A subtle soft rim light traces the edges of the console so its black silhouette stays clearly separated from the dark background. Ignore the white background in the reference images.

Static camera, no cuts, no zoom, photorealistic, no text, no logo. 3 to 4 seconds.
```

## 3-5 素材驗收標準

**看連續幀，不看單幀。** 零件變形、風扇葉片閃爍、PCB 細節漂移，單張截圖都看不出來

捲動式的頁面會讓使用者停在任意一幀細看，破綻容忍度比正常播放更低，所以驗收標準要比一般影片嚴

另外三條硬檢查：

- 燈條必須轉完視角才亮，中途亮起就重跑
- 前面板的孔位順序不得增減或搬位
- 背景必須是均勻的 `#111111`，偏亮、偏藍、有 vignette 都算 fail

Prompt 裡的具體描述（葉片數、零件形狀）只能從實際的首尾圖逐值抄，不能腦補
