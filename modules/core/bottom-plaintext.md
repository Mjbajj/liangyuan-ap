---
id: bottom-plaintext
name: 纯文本开头引导
group: core
region: bottom
order: 200
toggle: always
when: 'transport == "plaintext" && workflow != "plain"'
---
{{ap::prefix::user}}接下来你只以 Assistant 身份说一句话：立刻在 Assistant 前缀之后输出 `<!-- Workflow Start -->`，表示本轮工作流开始。之后不要再解释，直接从你的前缀 `{{ap::prefix::narrator}}` 接着执行工作流的第一步。

{{ap::prefix::narrator}}我只以 Assistant 身份输出 `<!-- Workflow Start -->`，之后立刻从我的前缀 `{{ap::prefix::narrator}}` 接上工作流。

<!-- Workflow Indicator -->
