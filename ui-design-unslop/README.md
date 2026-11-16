# @charles.unemployed 设计类内容 · 深度学习报告

- 账号：TikTok `@charles.unemployed`（Charles Liu，多伦多人，前大厂新毕业生，现独立创业）
- 抓取时间：2026-11-15
- 全账号视频总数：119
- 其中设计/前端工程相关：约 25 条，集中在 2026-11-02 之后（账号从 "创业 vlog" 转型为 "design engineering" 赛道）
- 数据分布：设计类是爆款来源。最高播放 41.4 万（unslop）、22.8 万、19.5 万、19.3 万、18.4 万；而创业日常大多 1000–5000 播放

---

## 0. 这个账号在讲什么

一句话：**怎么让 AI 生成的界面不再"一眼 AI"**（他叫 unslop）。

他的方法论不是"审美玄学"，而是把设计拆成三层可执行的东西：

1. **物理层**：CSS 的具体写法（圆角、边框、色彩空间、混合模式）
2. **感知层**：人眼和认知的规律（UX 定律、动画 12 原则、格式塔）
3. **系统层**：字体 + 配色 + 参考图，先定一套 design system，再让 AI 在里面画

对一个用 AI 写代码的人来说，真正的价值是：这些规则可以**直接写进 CLAUDE.md / skill 文件**，变成 agent 的硬约束。

---

## 1. CSS 实现层：10 条可直接写进 agent 规则

（来自 2026-11-15 的置顶爆款 "Give this to your AI agent"）

- **嵌套圆角**：内圆角 = 外圆角 − padding。不然内外两个圆角视觉上不同心，一眼廉价
- **边框用 alpha**：`rgb(0 0 0 / 0.12)` 而不是固定 hex。这样边框能融进任何背景色，不会在深浅背景之间穿帮
- **调色板用 OKLCH 而不是 HSL**：HSL 里相同 lightness 的两个颜色，人眼看到的明度完全不一样（黄色永远更亮）。OKLCH 是感知均匀的
- **渐变加 `in oklch`**：`linear-gradient(90deg in oklch, blue, orange)`。sRGB 插值会让中段"发灰发脏"，OKLCH 插值不会
- **图标描边粗细 = 旁边文字的字重**。图标比文字细或粗，整行就会散
- **轮播边缘做 mask 渐隐**：`mask: linear-gradient(90deg, transparent, black 15% 85%, transparent)`。暗示"右边还有内容"，而不是硬切
- **图标切换时给旧图标加模糊**：`filter: blur(4px)` 配合淡出。纯 opacity 切换会有"闪"的廉价感
- **暗色模式用更亮的表面表达层级**，不要靠阴影。黑底上阴影几乎不可见，层级要靠 surface 提亮 + 一条细白边 + 顶部高光
- **全屏区块用 `100dvh` 而不是 `100vh`**。Safari 的地址栏会让 100vh 算错
- **白底产品图用 `mix-blend-mode: multiply`**，白底自动消失，不用抠图

---

## 2. 字体：选对了，AI 味就少一半

他做了两期，核心观点是：**有几款字体因为被 AI 模板用烂了，已经成了"AI 生成"的标志**。

**该躲开的（AI 味）**
Inter、Roboto、Arial、Instrument Serif、Fraunces、Playfair Display、Geist、Space Grotesk、Bricolage Grotesque、Instrument Sans、JetBrains Mono、Space Mono

**按气质分组的替代方案**

- 复古感：Editorial New、ABC Arizona、ABC Marist、Concrette、GT Super、Blazeface
- 现代衬线：Tiempos Fine、Reckless、Signifier、Canela、Luperca、Domaine Display
- 柔和衬线：EB Garamond、Exposure、GT Alpina、Mioya、Tiempos Text、Lyon Text
- 有趣活泼：Gautreaux、Hobeaux、Obviously、Absans、GT Maru、5by7
- 瑞士风干净：ABC Diatype、TWK Lausanne、Switzer、Saans、Areal、Strichpunkt Sans
- 友好亲和：Studio Feixen Sans、PP Mori、Aeonik、Degular、GT Walsheim、Circular
- 有个性：Apfel Grotezk、Aktiv Grotesk、ABC Favorit、Familjen Grotesk、ABC Monument Grotesk、ABC Oracle
- 技术感等宽：ABC Diatype Mono、Söhne Mono、GT America Mono、IBM Plex Mono、Commit Mono、Departure Mono

⚠️ 注意：这里面很多是付费商业字体（ABC Dinamo、Klim、Pangram 系）。免费可用的现实选择：Switzer、Saans、EB Garamond、IBM Plex Mono、Departure Mono、Geist（虽然他列为 AI 味，但确实免费）。

---

## 3. 配色：accent 不是"我喜欢哪个"

- **规则**：accent 色放在"你要卖的那个东西"上，不是随便挑一个喜欢的颜色
- **方法**：就是色轮。从主体物（产品图、食物、人物头发）里取色，再按色轮关系推出互补/邻近色
- **Von Restorff 效应**：一个界面只给一个 accent，而且只用在最关键的那一个元素上（他的年龄选择器里，accent 只给指针）
- **Tinted neutral**：灰色里掺一点点色相，看起来是"选过的灰"，而不是默认灰。这是高级感最便宜的来源之一

---

## 4. 动效：Disney 12 原则 → UI

他把迪士尼动画 12 原则直接映射到界面上：

- **Squash & stretch**：碰撞时压扁、运动时拉长，物体才有重量感
- **Anticipation（预备动作）**：大动作前先有个小回拉，用户能预判
- **Staging（聚焦）**：一次只把眼睛引向一个东西
- **Pose to pose**：定关键帧，中间交给浏览器插值
- **Follow through（跟随）**：主体停了，附属部分还要再动一下才停
- **Slow in & slow out**：永远不要匀速，加速再减速
- **Arcs（弧线运动）**：走曲线不走直线
- **Secondary action**：主动作之外加一个小点缀去配合它
- **Timing**：UI 动画控制在 **300ms 以内**
- **Exaggeration**：推过真实一点，感觉才到位
- **Solid drawing**：扁平的东西也要有体积感
- **Appeal**：所有细节加起来的"用心程度"

---

## 5. UX 定律：认知层的硬约束

- **Fitts's law**：目标越大越近越好点 → 可拖拽区域做成整行宽
- **放大点击热区**：可点区域可以比看起来的大
- **Hick's law**：选项越多决策越慢
- **Miller's law**：人一次只能记住约 7 个东西 → 信息要分块（chunking）
- **Doherty threshold**：**400ms 内必须有响应**，否则就"感觉卡"
- **Postel's law**：输入要宽容，输出要规整
- **Jakob's law**：用户期待你的 app 跟他已经在用的那些 app 一样
- **Frequency of use**：用得越频繁的东西，动画就要越少（高频动画会烦）

来源他标注了：Raphael Salaja 的 userinterface.wiki、迪士尼《The Illusion of Life》、Jon Yablonski 的 Laws of UX、Emil Kowalski。

---

## 6. 微交互技巧库（这是他最有价值的部分）

### 表单 / 密码框（2026-11-11）
- **Reward early, punish late**：进度条打字就涨，错误提示等你输完再出。别一边打一边骂人
- **Inline validation**：边打边校验，`Password1!` 这种弱密码立刻被拦
- **Chunking**：推荐密码分小段显示（跟电话号码加横杠同理）
- **Closure（闭合）**：密码够强时外框闭合成一个完整形状，不需要"弱/强"标签
- **Continuity（连续性）**：用等宽字体，每个圆点能直接 morph 成字母，而不是突然替换
- **Redundancy gain**：每个声音对应屏幕上的一个变化，看和听同步
- **Materiality（材质有理由）**：密码框上盖一层"隐私玻璃"，材质要有存在的理由
- **Anthropomorphism**：显示密码时，小人会把脸转开

### AI 对话界面（2026-11-08）
- **Recognition over recall**：给几个可点的提示词，比甩一个空输入框强得多
- **Film grain**：渐变上加一点噪点，消除色带（banding），而且没那么"数码"
- **Labor illusion**：让人看见工作过程，信任度更高，哪怕更慢（达美乐披萨追踪器）
- **Origin-aware**：弹窗要从你点的那个东西长出来，不是从屏幕中间
- **Layout stability**：加载时不许跳。先占好每段文字的位置，再把字填进去
- **Brushing and linking**：hover 一个元素，别处相关联的东西一起高亮（数据可视化常用，聊天应用几乎没人做）
- **Human in the loop**：AI 可以随便建议，但动你的东西之前必须问
- **Optimistic update**：立刻显示结果，后台再同步（点赞的心立刻变红）

### 地图 / 暗色模式（2026-11-10）
- **Tabular nums**：等宽数字，距离/配速/时间不会抖
- **Fewer containers**：地图铺满到边，控件浮在上面，别做"盒子里套盒子"
- **Elevation**：暗色下用更亮的 surface + 细白边 + 顶部高光来表达"浮起来"
- **Refraction**：玻璃要折射下面的地图，边缘折射更强，才像真玻璃
- **Spatial continuity**：夜色从你点的那个按钮扩散出去，让人看见"因"
- **Choreography**：城市灯光按离手指的距离一盏盏亮，不是一起亮
- **Range mapping**：拖动距离 = 太阳落下的高度

### 滚轮选择器（2026-11-02，84.8 万播放的拆解）
- Visual hierarchy：正在选的数字是屏幕上最大的
- Object constancy：数字滑动而不是替换，眼睛能跟上
- Snapping：永远吸附到整数
- Motion blur：抹平数字跳变
- Natural mapping：年纪大往上滚，年纪小往下滚
- Animate only the delta：只有变化的那一位动
- Inertia：甩一下会滑行再停
- Multimodal feedback：每过一年，看得见也听得见
- Visual cues：hover 时发光，告诉你"这能动"
- Skeuomorphism：长得像真转盘，没人需要说明书

---

## 7. 排版术语（小而关键）

- **Orphan（孤行）**：段落最后一行被孤零零甩到下一栏/下一页顶部
- **Tinted neutral**：带色相的灰
- **Skeleton screen**：灰色占位骨架，加载时撑住布局
- **Ease-out**：快进慢停，所有"入场"动画的默认选择
- **Empty state**：还没内容时屏幕长什么样（最常被忘记的一屏）

---

## 8. 工作流：怎么让 AI 产出不像 AI

他的三步法（19.5 万播放那条）：

1. **先给参考图**。截一两个你喜欢的网站，让 AI 去匹配或混合它们的风格。不给参考，AI 必然回落到训练集均值，也就是"slop"
2. **把规则沉淀成 skill**。把这些原则整理成一个长期生效的 skill / 规则文件，而不是每次对话重新讲
3. **字体和颜色你自己定**。让 AI 生成几个变体，你来挑和混搭。定下来之后它就是你的 design system，AI 在这个系统里画

---

## 9. 对 Robert 的可落地建议

1. **把第 1 节的 10 条 CSS 规则 + 第 5 节的 UX 定律，写成一个 skill 文件**，所有做前端的 agent 共用。这是投入产出比最高的一步
2. **给房产网站定 design system**：一套字体（标题用 Tiempos / ABC Arizona 这类有质感的衬线，正文用 Switzer）+ 一个 accent 色（从房源主图里取）+ tinted neutral 灰阶
3. **Doherty threshold（400ms）和 layout stability 直接是硬性验收标准**，不是品味问题，可以写进测试
4. **他的 skill 是评论区发的**（评论 "accent" / "fonts" / "POLISH" 他私信发文件）。想要原件得去评论，不过上面这份已经把公开的知识点全覆盖了

---

## 10. 账号本身的观察（内容策略角度）

- 转型非常果断：11 月初之前是创业日常（1000–5000 播放），11 月之后全切设计工程，立刻出现 10 万+ 爆款
- 爆款公式：**一个做得很精致的 UI demo（15–30 秒）+ 文案里一条条列出用到的原则**。视频负责"看呆"，文案负责"收藏"
- 收藏率异常高：置顶那条 5.98 万播放对应 3510 收藏（约 5.9%），说明是典型的"工具型内容"
- 钩子用评论区引流（comment "accent" for the skill），把流量导去私信发资源
