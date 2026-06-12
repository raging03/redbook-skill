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

### 第一步：拉取今日 AI 日报，推荐选题

用 aihot API 拉取今日精选，筛选出**最适合小红书传播**的 3~5 篇文章推荐给用户。

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
- **优先推荐完整文章**（博客长文、媒体报道、官方发布），**减少推荐 Twitter/X 上的帖子**

**推荐时同步判断文章类型**：
- 📊 **数据冲击型**：文章核心是数字/排名/对比
- 📖 **故事叙事型**：文章有人物、事件经过、转折
- 💡 **观点争议型**：文章提出反常识结论或行业争议

**输出格式**：

```
📰 今日 AI 热点，推荐以下 X 篇适合做小红书：

1. **标题** — 来源  [📊数据冲击型 / 📖故事叙事型 / 💡观点争议型]
   💡 推荐理由：一句话说明为什么适合小红书
   🔗 链接

请告诉我你选哪篇？把文章 PDF 发给我，我来帮你做图文。
```

### 第二步：等待用户提供 PDF

用户会把文章 PDF 发给你。收到后：
1. 读取 PDF，判断文章类型（数据冲击型 / 故事叙事型 / 观点争议型）
2. 提取文章**英文短名**作为 slug（用于图片目录命名）
3. 直接进入第三步，不要重复确认

### 第三步：生成 HTML

→ 参见下方「**通用 HTML 生成规范**」，文案标题前缀用 `AI日报｜`

---

## AI知识模式

### 第一步：推荐知识选题

提供 5~8 个 AI 相关知识选题，覆盖大模型原理、AI产品经理必知概念等。

**选题池方向**（优先选江总**没做过**的）：
- 大模型基础：Transformer、Attention机制、Token与上下文窗口、Temperature参数、Embedding向量
- Agent技术：ReAct（已做）、Function Calling、MCP协议、Multi-Agent、Tool Use
- 应用技术：RAG检索增强生成、Prompt Engineering、Few-shot/Zero-shot、Chain-of-Thought
- 产品视角：AI幻觉的成因与缓解、模型评测Benchmark、微调vs提示词、模型量化与部署成本

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

**避免重复**：每次推荐前回顾已生成过的知识主题，不要重复推荐。已知已做：ReAct。

### 历史作品参考 / 避免重复选题

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

**AI知识已做：**
- ReAct：`/Users/jiangziyi/Downloads/AI Agent核心架构ReAct——LLM不思考就会胡说.html`

**非小红书/测试文件，通常不作为选题参考：**
- `spotlight-modal*.html`
- `fonts_preview (1).html`
- `ai_studio_code*.html`
- `五年级水循环小测验.html`

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
→ 参见下方「**通用 HTML 生成规范**」，文案标题前缀用 `AI知识｜`

---

## 通用 HTML 生成规范

### 卡片类型选择

根据内容选对应结构：

#### 📊 数据冲击型
适合：有明确数字、排名、对比的内容

| 卡片 | 内容 | 视觉重点 |
|------|------|----------|
| 01 封面 | badge → 大标题 → 副标题 → 核心数字 → 总结框 | 标题在数字前 |
| 02 背景 | 这个数字从何而来 | 简洁段落 |
| 03 数据全貌 | 所有关键数据一览 | 多个数字卡片并排 |
| 04 和谁对比 | 和竞品/历史对比 | ❌ vs ✅ 对比卡 |
| 05 为什么能做到 | 核心原因/方法 | 步骤列表 |
| 06 对普通人意味着什么 | 实际影响/应用场景 | 场景卡 |
| 07 结尾追问 | 留给读者的思考 | 金句 + 互动引导 |

#### 📖 故事叙事型
适合：有人物、事件经过、转折的内容

| 卡片 | 内容 | 视觉重点 |
|------|------|----------|
| 01 封面 | 最戏剧性的那一刻 + 大标题 | 场景还原 |
| 02 人物登场 | 谁，在什么背景下 | 角色介绍卡 |
| 03 事件经过 | 发生了什么（时间线） | 流程图或时间线 |
| 04 转折点 | 关键转折/意外发现 | 高亮引用块 |
| 05 调查/原因 | 为什么会这样 | 对比卡 |
| 06 结果/影响 | 最终怎么了 | 数据 + 结论框 |
| 07 结尾追问 | 更大的问题留给读者 | 金句 + 互动引导 |

#### 💡 观点争议型
适合：反常识结论或行业争议内容（AI知识模式常用此类）

| 卡片 | 内容 | 视觉重点 |
|------|------|----------|
| 01 封面 | 最反常识的那句话 | 大字引用，颜色强调 |
| 02 大家通常怎么认为 | 主流观点/常识 | 普通人认知卡（带❌） |
| 03 但实际上 | 核心观点 | 反转卡（带✅，对比强烈） |
| 04 证据① | 第一个支撑论据 | 数据或案例 |
| 05 证据② | 第二个支撑论据 | 数据或案例 |
| 06 影响和启示 | 这个结论改变了什么 | 实用建议卡 |
| 07 结尾追问 | 互动引导 | 金句 + 互动 |

### HTML 样式规范

- 卡片宽度 `width: 750px`，**固定高度 `height: 1000px`**
- 卡片外壳：`.card { display: flex; flex-direction: column; overflow: hidden; }`
- 顶部装饰条：`flex-shrink: 0`，高度 8px，彩色（情绪色：红/橙/蓝/绿/紫）
- 内容区：`.card-inner { flex: 1; padding: 40px 52px 32px; overflow: hidden; }`
  - **禁止设置 `display: flex`**，只用普通块流布局，内容用 `margin-bottom` 从顶部依次堆叠
  - 禁止 `justify-content: center/space-between`
- 底部页码：`.card-footer { flex-shrink: 0; padding: 12px 52px 16px; }`
- 白底 `background: #ffffff`
- 导语/副标题：`font-size: 38~42px; font-weight: 900; color: #111`
- 主标题：`font-size: 54px; font-weight: 900; color: #111; line-height: 1.3`
- 正文：`font-size: 27px; color: #333; line-height: 1.8`
- 黄色高亮：`.hl { background: #fff3b0; padding: 0 4px; }`
- 数据大字：`font-size: 46px; font-weight: 900; color: #e8192c`

### 写前必做：高度估算

card-inner 可用高度约 **868px**（= 1000 - 8顶条 - 52footer - 72padding）。

动笔前逐张估算：
- 正文每行 ≈ font-size × line-height（27px × 1.8 ≈ 49px/行）
- 加上所有标题、间距，估算是否达到 **≥ 700px**
- 不足则补充内容，超过则删减，**不要写完再改**

### 生成后自检（必做，不要跳过）

**① 高度溢出检查**：估算每张卡内容总高，超过 868px 必须删减，否则末尾内容被 overflow:hidden 截断。

**② 空白检查**：内容估算 ≥ 695px（80%），不足则按顺序：①加纯文字 → ②加大字号 → ③合并卡片。

**③ 字体层次检查**：封面导语性文字 ≥ 38px 加粗；同一卡内至少两个明显字号层次。

**④ 布局检查**：card-inner 没有 `display: flex`，没有 `justify-content: center`。

**⑤ 封面卡专项检查**：
- 有 ≥54px 大标题
- 顺序：badge → 大标题 → 副标题 → 数据
- 标题有吸引力：反常识/数字冲击/悬念，不用平铺直叙

**⑥ 视觉风格统一**：所有卡片同一种组织方式（全用色块盒子，或全用裸文字），禁止混用。

### 卡片数量与内容密度

- **目标张数：4～5 张**，不要超过 6 张
- 每张卡片正文 **100～150 字**为佳
- 内容不足先合并卡片，不要用大量空白凑数

---

## 第四步：展示 HTML，等待用户确认

```
HTML 已生成（模板类型）：
/Users/jiangziyi/Downloads/<标题>.html

用浏览器打开预览，确认没问题后告诉我「可以截图」或「需要修改」。
```

等待用户回复，不要自动进入第五步。

---

## 第五步：截图生成 PNG 图片

```bash
cd /Users/jiangziyi/Downloads && node screenshot_cards.mjs \
  "/Users/jiangziyi/Downloads/<文章标题>.html" \
  /Users/jiangziyi/Downloads/小红书图片 \
  <slug>
```

截图完成后告知：

```
✅ 图片已生成！

📁 路径：/Users/jiangziyi/Downloads/小红书图片/YYYYMMDD_<slug>/
共 X 张：
  📌 封面图 → card_01_封面.png   ← 小红书发布时选这张做封面
  📄 正文图 → card_02.png ~ card_0X.png
```

---

## 第六步：生成小红书配套文案

```
━━━━━━━━━━━━━━━
📌 小红书文案（直接复制）
━━━━━━━━━━━━━━━

【标题】
（20字以内，AI日报模式前缀「AI日报｜」，AI知识模式前缀「AI知识｜」）

【正文】
（150~300字，口语化，分段，有情绪起伏，结尾引导互动）

【标签】
AI日报模式：#人工智能 #AI #精准标签1 #精准标签2
AI知识模式：#AI知识 #人工智能 #大模型 #精准标签1 #精准标签2
━━━━━━━━━━━━━━━
```

**文案要求**：
- 正文从具体场景或反常识结论入手，不要开门见山讲背景
- 语气像跟朋友聊天，不要写成新闻稿
- 结尾必须有互动问题（"你觉得……""有没有人……"）

**AI知识模式**额外要求：
- 正文先抛「大家以为……但其实……」的反转
- 解释要有类比：用日常生活类比技术概念，降低理解门槛

---

## 第七步：同步历史参考到 GitHub

每次完成 HTML、截图和小红书文案后：
1. 将本次新生成的 HTML 路径补充进本文件的「历史作品参考 / 避免重复选题」清单，并同步更新 `redbook/SKILL.md` 中的同名清单。
2. 在 `/Users/jiangziyi/.claude/skills/redbook` 仓库中只提交相关 skill 文件改动。
3. 提交信息使用中文，说明本次新增的历史参考主题。
4. 推送到当前远程 GitHub 仓库；如果 `git push` 失败，必须如实告诉江总失败原因，不能说已同步。

---

## 注意事项

- 全程不要提 API 参数名、端点路径、HTML 技术细节
- 每一步完成后等用户回应，不要一次性全跑完
- 如果 aihot API 无响应，告知用户并请用户直接提供文章
- 截图脚本路径：`/Users/jiangziyi/Downloads/screenshot_cards.mjs`
- HTML 输出路径：`/Users/jiangziyi/Downloads/<标题>.html`
- 图片根目录：`/Users/jiangziyi/Downloads/小红书图片/`
- 每次截图自动建子目录 `YYYYMMDD_<slug>/`，不会覆盖历史图片
