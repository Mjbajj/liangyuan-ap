# 梁元 AP 迁移说明（v0.6 · 状态栏排版版）

来源：原生 SillyTavern 预设 **梁元 v4.1f 1017（Mj二创1.1）**。
v0.1 只做 1:1 搬家；**v0.2 按花瓣预设（Petals）的思路，把「思考」改造成「提问 → 规划 → 写作」三阶段工作流。**

## 0. v0.3：正文改成「按结构字段取」

- 默认传输方式改为 **structured**（`json_schema`）。回合 schema 只有三个字段：
  `step1_questions`（提问）/ `step2_plan`（规划）/ `step3_story`（正文）。
  正文从 `step3_story` 直接读，**不会再带 `</content>` 尾巴**，也不需要 contentMarker 解析。
- 模型看到的字段名和说明都是中文（`preset.yaml` 的 `tools` 段）。
- 所有提到 `<content>` 的模组都改成了 `@style` 双版本：
  - 对话式（plaintext / prefill）保留原措辞，标记照旧；
  - 指令式（structured / 工具）改成「写进本阶段正文，不要加包裹标签」。
  涉及：字数要求、输出模板、禁词表、状态栏×2、选项×2、转述×2。
- 配置四档：

  | 配置 | 传输 | 工作流 | 说明 |
  | --- | --- | --- | --- |
  | **结构化字段（推荐）** | structured | guided | 正文进字段，最干净 |
  | 工具调用 | tool-single | guided | 同样从字段取，适合工具调用强的模型 |
  | 纯文本 | plaintext | guided | 靠 `写手: ` 前缀 + `<content>` 标记解析 |
  | 快速模式 | plaintext | plain | 原预设行为（`<think>` + `<content>`） |

- ⚠️ **structured 需要模型或中转支持 `json_schema`**。如果报错或不返回 JSON，切「工具调用」；两者都不行就切「纯文本」。
- 状态栏 / 选项仍在正文内部（正文之后），还没拆成 AP 的卡片结构。

## 0.1 v0.4：状态栏与选项也改成结构卡片

| 东西 | 结构 id | 类型 | 指令式字段 | 对话式标签 | 界面表现 |
| --- | --- | --- | --- | --- | --- |
| 状态栏（简要/详细共用） | `status` | `custom`（text） | `z_status` | `<Status>…</Status>` | 消息下方可折叠卡片 |
| 结尾选项（三种共用） | `options` | `options`（8 项） | `z_options` | `<Options><option title="类别">描述</option>…</Options>` | 可点击的选项按钮（点击填入输入框，Ctrl/⌘/Shift 点击直接发送） |

改动要点：

- 状态栏不再塞在正文里：模组正文只讲「写什么」，输出位置由 `{{ap::structure::status}}` 决定；
  并加了 `{{ap::structure::status::last}}`，让模型能接着上一轮的状态更新（原预设只能从聊天记录里找）。
  原 `<状态栏>` 外层标签改成结构标签 `<Status>`；内部的 `<details>` 折叠保留。
- 三个结尾选项模组（剧情发展 / 续写 / 行动）**改成互斥组**（单选 `suggestions`，可一个都不选）。
  原预设可以同时开三个，会各输出一遍选项、互相打架；现在只能选一种风格。
- 选项数量统一成 **8**（`structures.options.count: 8`）。原「剧情发展选项」是 6 个，需要按 8 个来给，
  风格覆盖要求已同步改写。
- 结构字段自动加 `z_` 前缀排在工作流字段之后（`step1_questions` → `step2_plan` → `step3_story` → `z_status` → `z_options`），
  避免 Gemini 这类按字母序生成字段的接口把选项写在正文前面。
- 状态栏卡片可以直接编辑吗？不能（`custom` 卡片只读展示）；要改内容请重新生成。

## 0.1 v0.5：适配 DeepSeek 官方 API + 修复纯文本不跑工作流

### 问题一：结构化/工具配置报 `response_format` 错误

`This response_format type is unavailable now` 是 DeepSeek 官方 API 的真实限制：它只接受
`response_format: {"type":"json_object"}`，不接受 `json_schema`。AP 的 `structured` 传输会把 schema
交给 ST 的 `json_schema` 参数，ST 再转成 `response_format` 发给接口，所以在 DeepSeek 上必然失败。
**这不是预设写错，是接口能力不足。**

照花瓣预设的做法，给 DeepSeek 单独准备两套配置（并把默认配置切过去）：

| 配置 id | 传输 | 说明 |
| --- | --- | --- |
| `deepseek-tool`（默认） | `tool-single` | 只发工具列表 + `tool_choice`，**不发 response_format**；三个阶段作为提交工具的参数一次交回 |
| `deepseek-prefill` | `prefill` | 仅官方 DeepSeek API 可用，依赖前缀续写 |
| `structured` | `structured` | 保留给支持 JSON Schema 的接口；description 里已注明 DeepSeek 会报错 |
| `tools` | `tool-single` | 通用工具调用 |
| `plaintext` | `plaintext` | 对话式，靠前缀切分 |
| `plain` | `plaintext` + `plain` 工作流 | 快速模式，原预设行为 |

两套 DeepSeek 配置都带上 `sampling: { reasoning_effort: auto, show_thoughts: false }`（与花瓣一致）。

验证：`npx ap preview --config deepseek-tool --json` 里 `schema: null`、`tools` 只有
`start_workflow` / `submit_turn`，全串不含 `json_schema` / `response_format`。

### 问题二：纯文本配置跳过提问与规划，直接出正文

根因是**底部区域是空的**（预览里 `bottom (empty)`）：提示词最后一句是用户说「没问题的话，现在开始」，
后面没有任何要求模型以 `写手: ` 前缀开头的引导。模型于是直接写正文，AP 用 `finalFallback`
把原始文本兜成正文——表现就是「卡一下，然后直接出正文」。

照花瓣的 `bottom-plaintext` 修：

- `regions.bottom` 的角色从 `assistant` 改成 `system`（引导台词不能被当成模型已经说过的话）；
- 新增模组 `modules/core/bottom-plaintext.md`（`when: transport == "plaintext" && workflow != "plain"`）：
  要求模型先只输出 `<!-- Workflow Start -->`，然后**立刻从 `写手: ` 前缀接着执行工作流第一步**；
- 新增模组 `modules/core/bottom-prefill.md`（`when: transport == "prefill"`）：
  预填充模式下用 `写手: 我先把 责编 请来，开始本轮工作流。` 收尾。

同时补了三处解析隐患：

1. `actors` 的前缀改成 `{{ap::name::narrator}}: ` 这类动态写法，`user` 也补了前缀（对话式解析靠用户前缀截断「模型替用户说话」）；
2. `plaintextFormat: prefix` 显式声明；
3. `prefillGuide` 从空串改成 `<START_OF_STORY>`（空串时 AP 会插入 `<START>`，模型容易误解）；
4. 规划阶段的说明里明确「不要输出 `<content>` 标记」，避免模型在规划里提到 contentMarker 导致正文被提前切分。

## 0.2 v0.6：状态栏改白名单 HTML 排版 + 补回「风格化」必答问题

### 状态栏换行/排版塌陷的原因

AP 的状态栏是 `kind: custom, display: card` 卡片，渲染走 `src/ui/rich.ts` 的 `richText()`：

- 无 HTML 标签 → `plainToHtml()`：`\n\n` 分段落、单 `\n` 转 `<br>`，还支持 `**粗体**` / `` `代码` `` / `- 列表` / `> 引用`；
- 有 HTML 标签 → DOMParser 清洗，**只保留白名单**：
  - 块级 `P BLOCKQUOTE UL OL LI BR HR`，行内 `EM STRONG I B U S DEL CODE SPAN SMALL MARK`；
  - `<p special>` 会变成 `class="special"`（卡片里有高亮底色样式）；
  - 其他标签（`details` `summary` `div` `table`）**整层剥掉只留文字**，`style=` / `class=` 属性一律丢弃。

原状态栏用的 `<details><summary>` + 三反引号代码块，前者被剥、后者不转 `<pre>`，所以整块结构塌成一片——
这不是换行被吞，是承载结构的标签没了。

### 改法

两个状态栏模组整体重写为白名单 HTML：

- 日期/时间/位置：一行 `<p>` + `<strong>` 标签；
- 角色：`<p special>👩 名字 · 年龄 · 罩杯 · 身高</p>`（高亮条）；
- 所有条目：`<ul><li><strong>字段名</strong>：内容</li></ul>`，一条一行；
- 区块间用 `<hr>` 分隔；长段（内心想法、当前姿势）用 `<p><strong>标题</strong><br>正文</p>`；
- 模组里明确列出「允许的标签」与「禁止 `<details>`/`<div>`/`<table>`/`style=`」，并给出完整示例。

`structures.status.formatPrompt` 的 `tag` / `field` 两版说明同步写清白名单，避免指令式下模型又写回 `<details>`。

### 限制（改不了的部分）

- `style=` 会被清洗掉，所以**无法自定义颜色、边框、字号**，美化只能靠语义标签 + AP 自带主题配色；
- 卡片固定是折叠的（`CustomCard.vue` 写死 `collapsible :open="false"`），要默认展开得改扩展本身，预设层面做不到。

### 补回原思考步骤第 9 条

原「思考步骤」第 9 条「根据 `<核心风格>` 深入构思风格化」此前只在规划阶段提了一句，没进问题清单。
现在加为必答问题 `style`：`按 <核心文字风格>，本段要做哪些风格化处理？句式、节奏、意象与用词怎么统一？`，
必答问题从 7 条变 8 条。

## 1. 工作流设计（核心改动）

原预设是一段 565 字的「思考步骤」，让模型在 `<think>` 里一口气想完 11 件事。问题是：想什么、想到什么程度全靠模型自觉，而且思考内容混在正文里。

现在改成：

| 阶段 | 角色 | 做什么 | 对应原思考步骤 |
| --- | --- | --- | --- |
| **提问** `inquire` | 写手 | 把《要考虑的问题》里的必答问题逐条问出，选答问题挑 2-3 条。**只列问题，不解释** | 1/2/3/5 |
| **规划** `plan` | 责编 | 逐条回答并给方案；每个岔路口摆待决项→可选项→做决定（不降智无BUG / 不厌倦 / 最反预期）；敲定梗概、开头衔接、结尾落点；提醒字数与禁用元素 | 4/7/8/10 |
| **写作** `draft` | 写手 | 输出 `<content>` 后写正文与附加格式 | 6/9/11 |

两个角色：**写手**（narrator，写故事）、**责编**（editor，给建议和决策）。名字和前缀可以在面板「配置 → 身份」里改。

思考阶段（提问 + 规划）的内容不进聊天记录，只看得到正文——省下来的 token 比原版多。

## 2. 问题清单是动态的（花瓣的核心手法）

`preset.yaml` 声明两个字典变量：`mandatoryQuestions`、`optionalQuestions`。
**每个启用的模组自己往清单里加问题**，最后由 `modules/core/questions.md` 统一输出。关掉某个模组，它的问题就自动消失。

| 模组 | 贡献的问题 |
| --- | --- |
| questions（基础） | 意图 / 世界观 / 创作边界 / 当前场景 + 情绪冲击 / 避免重复 / 转折机会 |
| banned-words | 逐条核对禁用元素 |
| world-driven | 怎么推进剧情而不 OOC |
| prose-style | 排版与句式最容易违反哪条 |
| anti-omniscient | 要隐藏什么信息、避免哪些元词汇 |
| anti-flat / logic-consistency | 避免平淡造句 / 前后逻辑矛盾 |
| style-*（三个文风各自） | 按当前文风怎么校准 |
| dialogue-*（三档各自） | 对白比例与句子怎么拆 |
| transfer-*（三档各自） | 开头怎么衔接/改编读者回复 |
| user-role-*（四种各自） | `<user>` 的行动台词边界 |
| status-* / extras-* | 状态栏与选项的字段、数量、格式 |
| NSFW 各模块（11 个，各自独立键） | 限速、器官描写、反差、XP 落地、内心独白、母猪化、淫语等 |

> 键名故意各不相同（`mjLimiter`、`mjHeavy`、`mjInner`…），这样多个模块同时开时问题不会互相覆盖。

## 3. 两种配置

| 配置 | 工作流 | 适合 |
| --- | --- | --- |
| **提问→规划→写作（推荐）** | `guided` | 日常。思考分阶段，正文更稳 |
| **快速模式（不分阶段）** | `plain` | 赶时间/短对话。恢复原预设的 `<think>` 模板 + `<content>` 正文，单段思考 |

快速模式下 `旧版思考模板（快速模式用）` 和 `<prefix>` 预填充会自动生效（`when: workflow == "plain"`）；guided 模式下它们自动隐藏。

## 4. 参数（写作页）

| 参数 | 类型 | 默认 |
| --- | --- | --- |
| 正文字数 `length` | 数字 | 1500 |
| 人称 `pov` | 下拉（三/一/二） | 第三人称 |
| 角色外貌/外在类性癖 `xp_appearance` | 文本框 | 空 |
| 剧情描写/叙述类性癖 `xp_narrative` | 文本框 | 空 |

两个 XP 参数写进「涩情特化」模块的两处，留空时和原预设的空白行一致。

## 5. 安装 / 联调

1. ST 装 **Advanced Preset** 扩展 → AP 面板 → 预设 → 导入**文件** → `dist/liangyuan.apreset.json`；
2. 预设下拉选 `AP · 梁元`；
3. 边改边看：`npm run dev` → AP 面板 → 调试 → 开发源填 `http://localhost:4747`。

## 6. 想自己调的话

- **改阶段说的话**：`workflows/guided.md` 的 `@section stage:inquire/plan/draft`，`style=dialogue` 是对话式、`style=imperative` 是指令式，两种传输方式各用一份；
- **加一个问题**：在对应模组的 frontmatter 里加
  ```yaml
  vars:
    optionalQuestions:
      myQuestion: "你的问题文本？"
  ```
- **让用户自己加问题**：面板「我的模组」→ 变量部分 → 往 `mandatoryQuestions` / `optionalQuestions` 加键；
- **改角色名**：面板配置页，或 `preset.yaml` 的 `actors`；
- **改完必跑**：`npm run check`，再 `npm run build`。

## 7. 已知注意点

- `regions.history` 是空区域（挂 `historyStart` 锚点 + 插入点用），有一条无害警告；
- `contentMarker` 沿用 `<content>`，模型若把结尾 `</content>` 也输出，正文可能带尾巴——**需要实测**，必要时改成结构字段取正文；
- 对话式靠 `写手: ` / `责编: ` 前缀切分阶段；模型如果不带前缀，会被 `finalFallback` 兜底成纯正文；
- 状态栏/选项仍是普通提示词，没有改成 AP 的 cards（结构）；等稳定后再升级；
- SPreset 扩展的 RegexBinding 正则没搬进 AP，仍由 ST 侧负责美化显示。

## 8. 命令

```bash
npm run check     # 静态检查
npm run preview   # 看模型实际收到的提示词
npm run dev       # 开发服务器
npm run build     # 生成 dist/liangyuan.apreset.json
```
