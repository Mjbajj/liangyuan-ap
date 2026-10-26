---
id: bottom-prefill
name: 预填充开头引导
group: core
region: bottom
order: 200
toggle: always
when: 'transport == "prefill" && workflow != "plain"'
---
{{ap::prefix::narrator}}我先把 {{ap::name::editor}} 请来，开始本轮工作流。

<!-- Workflow Indicator -->
