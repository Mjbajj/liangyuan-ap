---
id: status-detailed
name: "➡️状态栏（Mj详细人物状态栏）"
structures: [status]
group: status
region: post
order: 3510
exclusive: status
vars:
  optionalQuestions:
    outputFormat: "详细状态栏要输出哪些字段？格式有什么硬性要求？"
---
# 状态栏

状态栏不写进正文，作为独立结构单独交回：{{ap::structure::status}}

上一轮的状态栏（有就据此更新，没有就忽略）：{{ap::structure::status::last}}

## 排版规则（务必遵守）

状态栏会渲染成消息下方的卡片，按 HTML 解析。**只允许使用这些标签**：`<p>`、`<p special>`（高亮条）、`<strong>`、`<em>`、`<mark>`、`<br>`、`<hr>`、`<ul>`、`<li>`、`<blockquote>`、`<code>`。

**禁止**使用 `<details>`、`<summary>`、`<div>`、`<table>`，也禁止任何 `style=` / `class=` 属性——它们会被直接剥离，导致排版塌成一片。

分区块用 `<p><strong>区块名</strong></p>` 起头，条目一律用 `<li>` 独占一行；不要用缩进、全角空格或竖线摆表格。

## 输出示例（结构不可更改，`<Status>` 标签本身也要保留）

<Status>
<p><strong>📅 日期</strong> 2026年10月27日（星期二）　<strong>⏰ 时间</strong> 深夜 1 时 20 分　<strong>📍 位置</strong> 港区·公寓卧室</p>
<hr>
<p special>👩 角色名 · 24岁 · E罩杯 · 168cm　💗 对 <user> 的好感度 0/200</p>
<p><strong>👚 当前服装</strong></p>
<ul>
<li><strong>上衣与下衣</strong>：描写衣服特色与重要细节</li>
<li><strong>腿部</strong>：描写丝袜质感与透肉程度，没有丝袜则不写</li>
<li><strong>内衣</strong>：描写当前穿着的内衣裤特色与重要细节</li>
<li><strong>鞋子</strong>：…</li>
<li><strong>道具或配饰</strong>：…</li>
</ul>
<p><strong>🥵 身体情况</strong></p>
<ul>
<li><strong>身材</strong>：用直白色情的语言写整体身材，肤色体态、高矮胖瘦、熟幼色甜，突出身体曲线，≥40字</li>
<li><strong>骚浪表情</strong>：面部外貌与表情，性爱交合时的表现，眼睛/嘴巴/舌头等快感表现，≥40字</li>
<li><strong>骚逼</strong>：内外情况、交合时内部表现、扩张情况、内容物、快感、散发的气味，≥40字</li>
<li><strong>屁眼</strong>：内外情况、交合时内部表现、扩张情况、内容物、快感、散发的气味，≥40字</li>
<li><strong>玉足</strong>：足部情况与交合时的表现，突出散发的气味，≥40字</li>
<li><strong>骚乳</strong>：先判断是否有乳穴，没有则描述乳头；写乳穴的扩张、内容物、快感、气味，≥40字；注明是否避孕</li>
<li><strong>子宫</strong>：扩张情况、是否能自行脱出、内容物、快感、气味，≥40字；注明是否避孕</li>
</ul>
<p><strong>🧮 数值记录</strong></p>
<ul>
<li><strong>子宫内精液浓度</strong>：0 ml（初次生成结合人物身份背景给初始值）</li>
<li><strong>屁眼内精液浓度</strong>：0 ml</li>
<li><strong>做爱次数</strong>：0（必须精确记录）</li>
<li><strong>高潮次数</strong>：0（必须精确记录）</li>
<li><strong>乳穴受孕</strong>：未怀孕；受孕后写实时进程</li>
<li><strong>子宫受孕</strong>：未怀孕；受孕后写实时进程</li>
<li><strong>生产次数</strong>：0（写明生产的婴儿种族）</li>
<li><strong>性欲程度</strong>：0/100（须符合上文情节并反过来影响下文；与淫乱程度正相关，下降表示已泄欲）</li>
<li><strong>淫乱化程度</strong>：0/100（数值后加“形容词+名词”四字词组，如 清纯人妻、骚浪骚货、淫荡母猪）</li>
</ul>
<p><strong>🧿 特殊状态</strong><br>精神或身体的特殊状态、是否被催眠、被催眠后催眠 APP 的模式、是否服药等。</p>
<p><strong>💬 内心想法</strong><br>人物第一人称、情绪化口语化、完整连贯的内心真实想法，≥80字，禁止使用“……”和“…”，禁止直接摘抄前文内心戏。</p>
<p><strong>😃 当前姿势</strong><br>详尽粗俗下流地描绘人物姿势与状态，≥45字。</p>
</Status>

## 输出要求

- 状态栏整段完整输出，不要截断；所有时间地点和条目内容**务必**详尽输出，数值条目须随剧情/前文实时更新，不得直接照搬设定。数值变化时在条目末尾用括号标注，例如 `<strong>💗 好感度</strong>：73/200（+3）`；其他没特别要求的条目不必解释。
- **若用户输入“导出”则不进行状态栏输出**
- **只在当前剧情/场景内有 <user> 之外的角色时输出角色状态栏**
- 地点/日期栏只输出一次
- 上面示例里括号内是给你的生成要求，不要原样输出括号内容

<warnning>不要生成 <user> 的状态栏，不要生成男性角色的状态栏。当前如果有多个角色，全部依次分开生成，每个角色一套 `<p special>` 标题、各区块与 `<ul>` 列表。如果当前没有合适的女角色，则留空。不要使用引号。</warnning>
