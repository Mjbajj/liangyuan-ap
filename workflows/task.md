---
id: task
name: 任务
kind: task
description: 元指令、代写、总结等非故事轮使用（可选，不影响正常写作）。
stages:
  - { id: thinking, actor: narrator, kind: text, tag: think, label: 思考 }
  - { id: content, actor: narrator, kind: content, optional: true, label: 输出 }
---
<!-- @section stage:thinking style=dialogue -->
先思考，思考写在 `<think>` 标签里。
<!-- @section stage:thinking style=imperative -->
先思考，思考写进 `think` 字段。
<!-- @section stage:content -->
只在被明确要求时给出结果。
