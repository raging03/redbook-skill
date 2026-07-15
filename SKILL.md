---
name: redbook
description: 小红书 AI 图文一键工作流。触发词：今日小红书图文、小红书图文、小红书日报、今日ai日报、今日AI日报、AI知识、今日AI知识、ai知识、redbook。支持两种模式：AI日报（拉取今日热点/用户提供文章）和AI知识（讲解大模型相关概念）。生成白底大标题图文 HTML 并截图出图，附带可直接复制的小红书正文+标签。
---

# Redbook Skill — 小红书 AI 图文工作流

当用户说以下触发词时启动此 skill，完整走完对应流程，不要跳步、不要省略。

**触发词分类：**
- 「今日小红书图文」「小红书图文」「小红书日报」「今日ai日报」「今日AI日报」「帮我做小红书」「redbook」→ 进入 **AI日报模式**
- 「AI知识」「今日AI知识」「ai知识」以及用户表示要做知识讲解的 → 进入 **AI知识模式**

---

## AI日报模式

**默认使用图文设计型（A型）**：每张卡片有大标题 + badge标签，结构清晰。直接进入第一步。

---

## 格式B — 纯文字型规范

### 结构特征

- 顶部固定红色细条（8px，`#e8192c`）
- **仅第一张卡片有大标题**（`.h1` 54px + 可选 `.lead` 40px 导语）
- **其他所有卡片：无标题，只有纯段落文字**
- `.badge` 红色边框标签（可选，用于标注来源/日期，只出现在第一张）
- `.hl` 黄色高亮用于关键短语（`background: #fff3b0`）
- `.quote` 引用块（30px 加粗，适合金句）
- 进度点颜色：红色 `#e8192c`

### CSS 模板

```css
* { margin: 0; padding: 0; box-sizing: border-box; }
body {
  background: #e8e8e8; padding: 40px 20px;
  display: flex; flex-direction: column; align-items: center; gap: 32px;
  font-family: "PingFang SC", "Helvetica Neue", "Source Han Sans CN", "Microsoft YaHei", sans-serif;
}
.card { width: 750px; height: 1000px; background: #fff; display: flex; flex-direction: column; overflow: hidden; }
.bar { flex-shrink: 0; height: 8px; background: #e8192c; }
.card-header { flex-shrink: 0; padding: 14px 52px 12px; display: flex; justify-content: flex-end; align-items: center; }
.pager { font-size: 21px; color: #999; }
.card-inner { flex: 1; padding: 36px 52px 28px; overflow: hidden; }
.card-footer { flex-shrink: 0; padding: 8px 52px 16px; display: flex; justify-content: center; gap: 6px; align-items: center; }
.dot { width: 7px; height: 7px; border-radius: 50%; background: #ccc; flex-shrink: 0; }
.dot.active { background: #e8192c; width: 18px; border-radius: 3px; }
.badge { display: inline-block; font-size: 22px; font-weight: 700; color: #e8192c; border: 2px solid #e8192c; border-radius: 4px; padding: 3px 12px; margin-bottom: 20px; }
.lead { font-size: 40px; font-weight: 900; color: #111; line-height: 1.25; margin-bottom: 14px; }
.h1 { font-size: 54px; font-weight: 900; color: #111; line-height: 1.25; margin-bottom: 22px; }
p { font-size: 27px; color: #333; line-height: 1.8; margin-bottom: 18px; }
p:last-child { margin-bottom: 0; }
b { font-weight: 700; color: #111; }
.hl { background: #fff3b0; padding: 0 4px; }
.quote { font-size: 30px; font-weight: 700; color: #111; line-height: 1.7; margin-bottom: 18px; }
.gray { color: #888; font-size: 24px; }
```

### 卡片结构

**封面（第一张）**：
```html
<div class="card">
  <div class="bar"></div>
  <div class="card-header"><span class="pager">1/N</span></div>
  <div class="card-inner">
    <div class="badge">来源 · 日期</div>
    <div class="lead">副标题导语（可选）</div>
    <div class="h1">大标题<br>换行写</div>
    <p>正文段落...</p>
    <div class="quote">「金句引用」</div>
    <p class="gray">补充说明</p>
  </div>
  <div class="card-footer"><!-- dots --></div>
</div>
```

**其他卡片（第二张起）**：
```html
<div class="card">
  <div class="bar"></div>
  <div class="card-header"><span class="pager">N/N</span></div>
  <div class="card-inner">
    <!-- 无标题，直接段落 -->
    <p><b>关键词加粗</b>正文...</p>
    <p>第二段...</p>
    <div class="quote">「金句」</div>
  </div>
  <div class="card-footer"><!-- dots --></div>
</div>
```

### 高度估算（格式B）

card-inner 可用高度 = 1000 - 8（顶条）- 48（header）- 64（padding）- 40（footer）≈ **840px**
- `p` 每行 ≈ 27 × 1.8 = 48.6px，加 margin-bottom 18px ≈ **67px/行**
- 3行段落 ≈ 3×48.6 + 18 = 163px；4行段落 ≈ 213px
- `.quote` ≈ 2行 × 54px + 18 = **126px**
- 目标：每张卡内容 ≥ **672px**（80%）

---

## 历史作品参考 / 避免重复选题

以下为江总过往已生成的小红书 HTML，可作为风格、密度和选题去重参考。推荐选题时优先避开同主题重复；生成新图文时参考其白底大标题、彩色信息卡、4～5 张高密度卡片风格。

**AI日报已做：**
- 豆包AI误导用户损失600元：`/Users/jiangziyi/Downloads/豆包AI误导用户损失600元.html`
- 毕业典礼谈AI被嘘：`/Users/jiangziyi/Downloads/毕业典礼谈AI被嘘.html`
- Kimi Work投资大师Skill：`/Users/jiangziyi/Downloads/KimiWork投资大师Skill.html`
- iPhone一夜变成AiPhone：`/Users/jiangziyi/Downloads/iPhone一夜变成AiPhone.html`
- 周星驰电影AI二创：`/Users/jiangziyi/Downloads/周星驰电影AI二创.html`
- 中国2950亿美元AI基建计划：`/Users/jiangziyi/Downloads/中国2950亿美元AI基建计划.html`
- Claude Code动态工作流：`/Users/jiangziyi/Downloads/Claude Code动态工作流：让AI Agent像流水线一样协作.html`
- SpaceX、Anthropic、OpenAI拟扎堆上市：`/Users/jiangziyi/Downloads/SpaceX、Anthropic、OpenAI拟扎堆上市！加起来市值近4万亿美元.html`
- Claude Opus 4.8变安全了，但也更难用了：`/Users/jiangziyi/Downloads/Claude Opus 4.8变安全了，但也更难用了？.html`
- AI表现优于法学教授：`/Users/jiangziyi/Downloads/AI表现优于法学教授：斯坦福研究发现教授更偏好AI答案.html`
- 联合国报告警示AI数据中心水电消耗：`/Users/jiangziyi/Downloads/联合国报告警示：AI驱动下2030年数据中心水电消耗将翻倍.html`
- 佛罗里达起诉OpenAI：`/Users/jiangziyi/Downloads/佛罗里达起诉OpenAI！凶手行凶前先问了ChatGPT.html`
- OpenAI总裁亲述奥特曼被罢免：`/Users/jiangziyi/Downloads/OpenAI差点没了？总裁亲述：奥特曼被罢免当天，我就辞职了.html`
- DeepSeek V4价格打骨折：`/Users/jiangziyi/Downloads/DeepSeek V4价格打骨折，宁王京东网易抢着入场，梁文锋：目标是AGI.html`
- 微信读书Skill：`/Users/jiangziyi/Downloads/微信读书 Skill 冲上热搜，AI 终于让我的书不白读了.html`
- Cursor Composer 2.5性价比：`/Users/jiangziyi/Downloads/1／10成本、Opus 4.7级表现，Cursor甩出了性价比之王Composer 2.5.html`
- 从卖token到卖结果：`/Users/jiangziyi/Downloads/从卖 token 到卖结果，这些公司开始让 AI 背 KPI 了.html`
- ChatGPT打通银行API：`/Users/jiangziyi/Downloads/OpenAI把手伸进你的钱包了！ChatGPT 打通银行 API，全网炸锅：这是给黑客发年终奖？.html`
- Anthropic创业秘籍：`/Users/jiangziyi/Downloads/Anthropic 33页创业秘籍曝光！你和Claude就能撑起一家独角兽.html`
- 硅谷AI创业手册：`/Users/jiangziyi/Downloads/硅谷传疯了这份AI创业手册.html`
- AI数学能力争议：`/Users/jiangziyi/Downloads/2小时搞定博士论文，3天终结40年悬案：AI宣判数学「死刑」？.html`
- Claude Code隐藏玩法：`/Users/jiangziyi/Downloads/Claude Code隐藏玩法爆火！Anthropic大佬：不要再用Markdown了.html`
- AI裁员潮：`/Users/jiangziyi/Downloads/小红书_AI裁员潮来袭_图文.html`
- AI勒索：`/Users/jiangziyi/Downloads/小红书_AI勒索_图文.html`
- AI杀死自助书：`/Users/jiangziyi/Downloads/AI杀死了自助类书籍？畅销书作者销量暴跌80%.html`
- SpaceX收购Cursor：`/Users/jiangziyi/Downloads/SpaceX刚IPO，就600亿买下Cursor.html`
- 豆包专业版实测做题家风格：`/Users/jiangziyi/Downloads/AI日报｜豆包专业版实测：做题家风格，办公有余，生产不足.html`
- 特斯拉FSD夺命车祸（FSD杀人? vs 踏板日志）：`/Users/jiangziyi/Downloads/特斯拉FSD夺命车祸：司机甩锅，Tesla反手亮出100%油门.html`
- Meta投毒竞品AI（Cannes项目·伪装未成年压测）：`/Users/jiangziyi/Downloads/Meta被曝投毒竞品AI：假扮未成年发4万5千条高危提问.html`
- GPT-5.6 Sol自行删除用户文件与数据库：`/Users/jiangziyi/Downloads/AI日报｜OpenAI旗舰模型GPT-5.6 Sol自己删文件了，还删完不告诉你.html`

**AI知识已做：**
- ReAct：`/Users/jiangziyi/Downloads/AI Agent核心架构ReAct——LLM不思考就会胡说.html`
- Loop Engineering：`/Users/jiangziyi/Downloads/Loop Engineering：别只会写Prompt了.html`
- GUI vs CLI Agent：`/Users/jiangziyi/Downloads/GUI vs CLI Agent：AI该点屏幕还是敲命令.html`

---

## 第一步：拉取今日 AI 日报，推荐选题

用 aihot skill 的 API 拉取今日精选，筛选出**最适合小红书传播**的 3~5 篇文章推荐给用户。

```bash
UA="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36 aihot-skill/0.2.0"
since=$(date -u -v-24H +%Y-%m-%dT%H:%M:%SZ 2>/dev/null || date -u -d '24 hours ago' +%Y-%m-%dT%H:%M:%SZ)
curl -sH "User-Agent: $UA" "https://aihot.virxact.com/api/public/items?mode=selected&since=$since&take=50"
```

**筛选标准**（小红书传播性优先）：
- 有反常识/反直觉的信息（"AI 竟然……""没想到……"）
- 有数字冲击力（百亿融资、模型排名、数据对比）
- 贴近普通用户生活（不要纯学术论文）
- 故事性强，有人物、有转折
- **优先推荐完整文章**（博客长文、媒体报道、官方发布），**减少推荐 Twitter/X 上的帖子**；帖子信息碎、来源浅，做出来内容薄

**推荐时同步判断文章类型**（供第三步选模板用）：
- 📊 **数据冲击型**：文章核心是数字/排名/对比（融资额、准确率、成本降幅等）
- 📖 **故事叙事型**：文章有人物、事件经过、转折（人物决策、事件始末）
- 💡 **观点争议型**：文章提出反常识结论或行业争议（"大家都错了""没人提的真相"）

**输出格式**（给用户看的推荐列表）：

```
📰 今日 AI 热点，推荐以下 X 篇适合做小红书：

1. **标题** — 来源  [📊数据冲击型 / 📖故事叙事型 / 💡观点争议型]
   💡 推荐理由：一句话说明为什么适合小红书
   🔗 链接

2. ...

请告诉我你选哪篇？把文章 PDF 发给我，我来帮你做图文。
```

---

## 第二步：等待用户提供 PDF

用户会把文章 PDF 发给你。收到后：
1. 读取 PDF，判断文章类型（数据冲击型 / 故事叙事型 / 观点争议型）
2. 提取文章**英文短名**作为 slug（用于图片目录命名，如 `claude-blackmail`、`openai-hardware`）
3. 直接进入第三步，不要重复确认

---

## 第三步：读取 PDF，生成小红书图文 HTML

根据文章类型，选对应模板生成 HTML，输出到：
`/Users/jiangziyi/Downloads/<文章标题>.html`

文件名规则：
- 直接使用原文章标题作为文件名（中文标题完整保留）
- 标题要醒目、吸引人，若原标题太平淡可在括号内补充冲击性副标题
- 示例：`Claude Code隐藏玩法爆火！Anthropic大佬：不要再用Markdown了.html`
- AI日报模式文案标题前缀用 `AI日报｜`

---

## AI知识模式

### 第一步：推荐知识选题

提供 5～8 个 AI 相关知识选题，覆盖大模型原理、AI产品经理必知概念等。

**选题池方向**（优先选江总没做过的）：
- 大模型基础：Transformer、Attention机制、Token与上下文窗口、Temperature参数、Embedding向量
- Agent技术：Function Calling、MCP协议、Multi-Agent、Tool Use
- 应用技术：RAG检索增强生成、Prompt Engineering、Few-shot/Zero-shot、Chain-of-Thought
- 产品视角：AI幻觉的成因与缓解、模型评测Benchmark、微调vs提示词、模型量化与部署成本

**避免重复**：每次推荐前回顾「历史作品参考 / 避免重复选题」中的 AI知识已做主题。已知已做：ReAct。

**推荐格式**：

```
📚 AI知识选题推荐（以下都没做过）：

1. **RAG 检索增强生成** — 为什么 AI 能「有据可查」而不是瞎编
2. **Token 与上下文窗口** — 为什么 AI 会「忘事」，上下文到底是什么
3. **Temperature 参数** — 一个数字决定 AI 是「规矩」还是「发散」
4. **Function Calling** — AI 怎么真的「动手」操作外部系统
5. **Prompt Engineering** — 为什么同一个问题，不同问法结果差这么多
...

请选一个，我先整理内容大纲和文案思路给你确认，再做 HTML。
```

### 第二步：用户选题后，先输出内容大纲 + 文案思路

用户选定后，**先不要写 HTML**，输出以下内容让用户确认：

```
✅ 选题：[主题名]

📋 卡片大纲（5张）：
1. 封面：[一句话说封面核心冲击点]
2. [卡片主题]：[核心内容概述]
3. [卡片主题]：[核心内容概述]
4. [卡片主题]：[核心内容概述]
5. 结尾：[互动引导方向]

📝 文案思路：
- 标题方向：AI知识｜[标题草稿]
- 正文切入角度：[一句话说切入点]

确认大纲没问题后说「开始做」，我来生成 HTML。
```

### 第三步：生成 HTML

用户确认后，搜集整理该知识点的干货内容，生成 HTML。
- 参照下方「三种模板选择」和「HTML 样式规范」
- AI知识模式通常优先使用 💡观点争议型，也可按内容改用数据冲击型
- 文案标题前缀用 `AI知识｜`
- 生成 HTML 后，继续执行第四步预览、第五步截图、第六步文案、第七步同步

---

## 通用 HTML 生成规范

### 字数控制（重要）

**每张卡片正文不超过 150 字**。如果内容多，宁可拆成两张卡片，也不要在一张卡片里堆文字。正文字数过多会导致截图排版拥挤，影响阅读体验。

### 写前估算内容密度（先别急着写代码）

**动笔前先在脑子里过一遍每张卡的内容量**，确认能填满卡片，再开始写 HTML。

card-inner 可用高度约 **868px**。粗略估算方式：
- 正文每行 ≈ font-size × line-height（如 26px × 1.75 ≈ 45px/行）
- 加上所有标题、间距、padding，估算总高是否达到 **≥ 700px**

若某张卡预估内容不足，**在动笔前就从原文补充内容**，而不是写完再改：
1. 多提取一段信息、一个数据、一句人物引言
2. 把同类两张卡合并，内容合在一起才够撑满

> 教训：第一版每张卡片只放了一半内容就交稿，导致大段空白需要返工。估算是写前必做的一步，不是可选项。

### 三种模板选择

根据文章类型选对应卡片结构：

#### 📊 数据冲击型

适合：有明确数字、排名、对比的文章（融资、准确率、成本等）

| 卡片 | 内容 | 视觉重点 |
|------|------|----------|
| 01 封面 | **顺序固定**：badge徽标 → 大标题（≥54px） → 副标题说明 → 核心大数字 → 辅助数据/总结框 | 标题在数字前，让人先看懂主题再看数字 |
| 02 背景 | 这个数字从何而来，是谁做的 | 简洁段落 |
| 03 数据全貌 | 所有关键数据一览 | 多个数字卡片并排 |
| 04 和谁对比 | 和竞品/历史对比 | ❌ vs ✅ 对比卡 |
| 05 为什么能做到 | 核心原因/方法 | 步骤列表 |
| 06 对普通人意味着什么 | 实际影响/应用场景 | 场景卡 |
| 07 结尾追问 | 留给读者的思考 | 金句 + 互动引导 |

#### 📖 故事叙事型

适合：有人物、事件经过、转折的文章

| 卡片 | 内容 | 视觉重点 |
|------|------|----------|
| 01 封面 | 最戏剧性的那一刻 + 大标题 | 场景还原（邮件/对话/截图模拟） |
| 02 人物登场 | 谁，在什么背景下 | 角色介绍卡 |
| 03 事件经过 | 发生了什么（时间线） | 流程图或时间线 |
| 04 转折点 | 关键转折/意外发现 | 高亮引用块 |
| 05 调查/原因 | 为什么会这样 | 对比卡（❌排除 vs ✅成立） |
| 06 结果/影响 | 最终怎么了，影响是什么 | 数据 + 结论框 |
| 07 结尾追问 | 更大的问题留给读者 | 金句 + 互动引导 |

#### 💡 观点争议型

适合：提出反常识结论或行业争议的文章

| 卡片 | 内容 | 视觉重点 |
|------|------|----------|
| 01 封面 | 最反常识的那句话 | 大字引用，颜色强调 |
| 02 大家通常怎么认为 | 主流观点/常识 | 普通人认知卡（带❌） |
| 03 但实际上 | 文章的核心观点 | 反转卡（带✅，对比强烈） |
| 04 证据① | 第一个支撑论据 | 数据或案例 |
| 05 证据② | 第二个支撑论据 | 数据或案例 |
| 06 影响和启示 | 这个结论改变了什么 | 实用建议卡 |
| 07 结尾追问 | 你还相信原来的观点吗 | 金句 + 互动引导 |

### HTML 样式规范（三种模板通用）

- 卡片宽度 `width: 750px`，**固定高度 `height: 1000px`**（3:4 比例，上传小红书不出现白边）
- 卡片外壳用 flex 布局：`.card { display: flex; flex-direction: column; overflow: hidden; }`
- 顶部装饰条：`flex-shrink: 0`，高度 8px
- 内容区：`.card-inner { flex: 1; padding: 40px 52px 32px; overflow: hidden; }`
  - **card-inner 禁止设置 `display: flex`**，只用普通块流布局，内容用 `margin-bottom` 从顶部依次堆叠
  - 内容从顶部开始堆，底部留白 < 顶部留白，视觉上更自然；禁止 `justify-content: center/space-between`
- 底部页码：`.card-footer { flex-shrink: 0; }`，padding `12px 52px 16px`
- 白底 `background: #ffffff`
- **导语/副标题**（封面卡的第一行说明文字）`font-size: 38~42px; font-weight: 900; color: #111`
- 主标题 `font-size: 54px; font-weight: 900; color: #111; line-height: 1.3`
- 正文 `font-size: 27px; color: #333; line-height: 1.8`
- 顶部 8px 彩色装饰条（情绪色：红/橙/蓝/绿/紫）
- 黄色高亮 `.hl { background: #fff3b0; padding: 0 4px; }`
- 数据大字 `font-size: 46px; font-weight: 900; color: #e8192c`
- 每张卡片 `.card` class

### 生成后自检（写完 HTML 必做，不要跳过）

生成 HTML 后，**在告知用户之前**，逐张过一遍以下检查项：

**① 高度溢出检查**
card-inner 可用高度 = 1000 - 8（装饰条）- 52（footer）- 72（padding）= **868px**。
估算每张卡内容总高：每行 ≈ font-size × line-height，加上所有 margin/padding。
若估算超过 868px，必须删减内容或拆分卡片，否则 `overflow: hidden` 会静默截断末尾内容。

**② 空白检查**
脑补每张卡的视觉效果：内容区是否占满 868px 的 **80% 以上**（即内容估算 ≥ 695px）？
若某张明显稀疏，按优先级补救：
1. **加纯文字**：补充相关背景、人物介绍、延伸信息等纯文本段落，填满空白区域
2. **加大字号**：将正文从 27px 适当放大到 29~31px
3. **合并卡片**：与相邻卡片合并，仍不满足则重组内容结构

**③ 字体层次检查**
- 封面/结尾卡的导语性文字（如"Anthropic 工程师最新发文说"）≥ **38px 加粗**，不能用 28px 灰色小字
- 主标题 ≥ 42px，正文 27px，角标/来源 20px
- 同一张卡内至少有两个明显的字号层次，避免全卡平铺同样大小的文字

**④ 布局检查**
确认 card-inner 没有 `display: flex`，没有 `justify-content: center`。

**⑤ 封面卡专项检查**（最容易出问题，必查）

- **有没有大标题**：封面必须有 ≥54px 的大标题，让人一眼看出主题。不能直接上数字，数字没有标题就是一堆无意义的数字。
- **顺序对不对**：badge → 大标题 → 副标题/说明文字 → 核心数据。绝对不能把数字放在标题前面。
- **AI知识标题检查**：顶部 badge 标签保留；核心概念必须在标签下方的正文主视觉标题区突出，放第一行，标题最多两行；不要在核心概念前先写长导语或铺垫句。示例：顶部标签 → `RAG` + `到底是什么？`。
- **AI日报标题检查**：优先吸引人，用数字冲击、反常识、冲突、悬念；可以重写原标题，但必须保留事实准确。
- **标题选择**：AI日报优先使用原文里最有冲击力的标题/金句，如原标题太平则改成更抓人的小红书标题；AI知识优先突出概念名。
- **标题有没有吸引力**：用反常识措辞（"公司越赚裁员越狠"）、数字冲击或悬念，**不要用平铺直叙的描述**（"科技巨头用机器换人" ❌ → "公司越赚裁员越狠" ✅）。
- **标题换行方式**：多行标题必须用 `<br>` 按语义断行，不要依赖自然折行。AI知识标题不超过两行；断行点要符合中文语义，不能把一个短语切断。

**⑥ 内容来源检查**

- **卡片正文优先照抄原文**：不要自己重新组织语言。原文的表述往往更精准、更有细节，自己改写反而容易失真或变得平淡。
- 直接引用原文关键句、数据、人物说法，保留原文的措辞力度。
- 只在原文表达不适合小红书阅读节奏时（如太学术、段落太长）才做必要的精简，不要做超出必要的改写。

**⑦ Section 标题检查**

- **优先从原文里找金句作 section 标题**，不要自造平淡陈述句。原文作者打磨过的短句往往比临时自创的更有冲击力。
  - ❌ 「证据正在加速堆积」「连数学之神都变了」——陈述平淡，无吸引力
  - ✅ 「数学界的「深蓝时刻」来了」「嘴上说廉价，身体却异常诚实」——来自原文，有力度
- Section 标题**能一行写下就不换行**，不要用 `<br>` 强制分行。字数允许时默认单行，确实太长才换行，换行点必须符合语义。

**⑧ 全卡视觉风格统一检查**

- 同一套图文，所有卡片必须用**同一种内容组织方式**：要么全卡用盒子（背景色块 + 圆角），要么全卡用裸文字 + 标签。
- 禁止混用：有的卡片用盒子、有的卡片用裸文字段落，会让整套图文看起来像两套不同设计拼接，割裂感明显。
- 检查时逐张对比：每张卡的内容块样式（背景色、圆角、padding、标题颜色）是否一致。

### 卡片数量与内容密度（重要）

- **目标张数：4～5 张**，不要超过 6 张
- 每张卡片内容必须**视觉饱满**，不能有大段空白
- 如果某张内容撑不满，按顺序处理：①加纯文字补内容 → ②加大字号 → ③与相邻卡合并
- 合并原则：相关联的两张（如"对比"+"证据"，"技术拆解"+"回应"）合为一张
- 正文字数每张 **100～150 字**为佳

---

## 第四步：展示 HTML，等待用户确认

告知用户使用的模板类型，并附上预览提示：

```
HTML 已生成（📊数据冲击型 / 📖故事叙事型 / 💡观点争议型 模板）：
/Users/jiangziyi/Downloads/<文章标题>.html

用浏览器打开预览，确认没问题后告诉我「可以截图」或「需要修改」。
如需修改，请说明具体改哪张卡片的什么内容。
```

等待用户回复，不要自动进入第五步。

---

## 第五步：截图生成 PNG 图片

用户确认后，用第二步提取的 slug 执行截图脚本：

```bash
cd /Users/jiangziyi/Downloads && node screenshot_cards.mjs \
  "/Users/jiangziyi/Downloads/<文章标题>.html" \
  /Users/jiangziyi/Downloads/小红书图片 \
  <slug>
```

截图脚本参数说明（screenshot_cards.mjs）：
- `deviceScaleFactor: 1.71`（输出约 1280×1710px，卡在小红书最大尺寸，不触发压缩，图片最清晰）
- `width: 750px`，卡片固定高度 1000px → 物理输出恰好 1283×1710px，7 张全部尺寸一致

截图完成后告知用户：

```
✅ 图片已生成！

📁 路径：/Users/jiangziyi/Downloads/小红书图片/YYYYMMDD_<slug>/
共 X 张：
  📌 封面图 → card_01_封面.png   ← 小红书发布时选这张做封面
  📄 正文图 → card_02.png ~ card_0X.png
```

---

## 第六步：生成小红书配套文案

根据文章内容和模板类型，生成可直接复制的发布文案：

```
━━━━━━━━━━━━━━━
📌 小红书文案（直接复制）
━━━━━━━━━━━━━━━

【标题】
（20字以内，有冲击力，含核心关键词，可加 emoji）

【正文】
（150~300字，口语化，分段，有情绪起伏，结尾引导互动）

【标签】
#人工智能 #AI #（精准标签1）#（精准标签2）#（热点标签）
━━━━━━━━━━━━━━━
```

**文案写作要求**：
- 标题：AI日报模式必须以「AI日报｜」开头；AI知识模式必须以「AI知识｜」开头；总字数不超过 20 字（含前缀）
- 标题方向：数字/反常识/悬念三选一，前 10 字决定点击率
- 示例标题格式：`AI日报｜10分钟做投资助手`、`AI知识｜RAG到底是什么`
- 正文：从一个具体场景或反常识结论入手，不要开门见山讲背景
- 语气：像在跟朋友聊天，不要写成新闻稿
- 结尾：必须有一个互动问题（"你觉得……""有没有人……"）
- 标签：AI日报用 `#人工智能 #AI #精准标签1 #精准标签2`；AI知识用 `#AI知识 #人工智能 #大模型 #精准标签1 #精准标签2`

**按模板类型调整文案风格**：
- 📊 数据冲击型：正文先抛最震撼的数字，再解释背后含义
- 📖 故事叙事型：正文用"你知道吗……"开头，讲最戏剧性的转折
- 💡 观点争议型：正文先说"大家都以为……但其实……"，制造反转

---

## 第七步：同步历史参考到 GitHub

每次完成 HTML、截图和小红书文案后：
1. 将本次新生成的 HTML 路径补充进 `redbook/SKILL.md` 的「历史作品参考 / 避免重复选题」清单。
2. 在 `/Users/jiangziyi/.claude/skills/redbook` 仓库中只提交相关 skill 文件改动。
3. 提交信息使用中文，说明本次新增的历史参考主题。
4. 推送到当前远程 GitHub 仓库 `https://github.com/raging03/redbook-skill.git`；如果 `git push` 失败，必须如实告诉江总失败原因，不能说已同步。

---

## 注意事项

- 全程不要提 API 参数名、端点路径、HTML 技术细节
- 每一步完成后等用户回应，不要一次性全跑完
- 如果 aihot API 无响应，告知用户并请用户直接提供文章
- 生成 HTML 时严格控制每张卡片 ≤150 字，宁可多加一张卡片
- 截图脚本路径：`/Users/jiangziyi/Downloads/screenshot_cards.mjs`
- HTML 输出路径：`/Users/jiangziyi/Downloads/<文章标题>.html`（直接用文章标题命名，不要用 `小红书_图文.html` 这类通用名，存多了自己都分不清）
- 图片根目录：`/Users/jiangziyi/Downloads/小红书图片/`
- 每次截图自动建子目录 `YYYYMMDD_<slug>/`，不会覆盖历史图片
