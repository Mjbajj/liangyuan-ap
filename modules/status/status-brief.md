---
id: status-brief
name: "➡️状态栏（Mj简要人物状态栏）"
structures: [status]
group: status
region: post
order: 3500
exclusive: status
vars:
  optionalQuestions:
    outputFormat: "简要状态栏要输出哪些字段？格式有什么硬性要求？"
---
# 状态栏

状态栏不写进正文，作为独立结构单独交回：{{ap::structure::status}}

上一轮的状态栏（有就据此更新，没有就忽略）：{{ap::structure::status::last}}

## 排版规则（务必遵守）

状态栏会渲染成消息下方的卡片，按 HTML 解析。**只允许使用这些标签**：`<p>`、`<p special>`（高亮条）、`<strong>`、`<em>`、`<mark>`、`<br>`、`<hr>`、`<ul>`、`<li>`、`<blockquote>`、`<code>`。

**禁止**使用 `<details>`、`<summary>`、`<div>`、`<table>`，也禁止任何 `style=` / `class=` 属性——它们会被直接剥离，导致排版塌成一片。

每个条目独占一行（`<li>` 或 `<p>`），字段名用 `<strong>` 包住。不要用缩进、全角空格或竖线对齐表格。

## 输出示例（结构不可更改，`<Status>` 标签本身也要保留）

<Status>
<p><strong>📅 日期</strong> 2026年10月27日（星期二）　<strong>⏰ 时间</strong> 深夜 1 时 20 分　<strong>📍 位置</strong> 港区·公寓卧室</p>
<hr>
<p special>👩 角色名 · 24岁 · E罩杯 · 168cm</p>
<ul>
<li><strong>身份</strong>：…</li>
<li><strong>年龄</strong>：…</li>
<li><strong>好感度</strong>：0/100</li>
<li><strong>性欲程度</strong>：0/100</li>
<li><strong>淫乱程度</strong>：0/100</li>
<li><strong>怀孕状态</strong>：…（只要剧情内射了就必然怀孕，直接显示已怀孕）</li>
<li><strong>当前状况</strong>：…</li>
</ul>
<hr>
<p><strong>💬 内心想法</strong><br>人物第一人称、情绪化口语化、完整连贯的内心真实想法，≥80字，禁止使用“……”和“…”，禁止直接摘抄前文内心戏，正常使用其他标点符号。</p>
<p><strong>😃 当前姿势/表情</strong><br>详尽粗俗下流地描绘人物整体身材外貌、姿势表情与性器状态，≥300字。</p>
</Status>

## 输出要求

- 状态栏整段完整输出，不要截断；所有时间地点和条目内容**务必**详尽输出，数值条目须随剧情/前文实时更新，不得直接照搬设定。数值变化时在条目末尾用括号标注，例如 `<strong>💗 好感度</strong>：73/100（+3）`；其他没特别要求的条目不必解释。
- **若用户输入“导出”则不进行状态栏输出**
- **只在当前剧情/场景内有 <user> 之外的角色时输出角色状态栏**
- 地点/日期栏只输出一次
- 上面示例里括号内是给你的生成要求，不要原样输出括号内容

<warnning>不要生成 <user> 的状态栏，不要生成男性角色的状态栏。当前如果有多个角色，全部依次分开生成，每个角色一套 `<p special>` 标题加一个 `<ul>` 列表。如果当前没有合适的女角色，则留空。不要使用引号。</warnning>
