---
id: guided
name: 提问 → 规划 → 写作
kind: story
description: 写手先向责编提问，责编给出方案与决策，写手再动笔。默认工作流。
hint: （写手先问，责编给方案，写手再写）
stages:
  - id: inquire
    actor: narrator
    kind: text
    label: 提问
    field: { name: "step{{n}}_questions", description: "{{ap::name::narrator}} 向 {{ap::name::editor}} 提出本轮必须想清楚的问题。只列问题，不解释。" }
    tool:
      name: ask_questions
      description: "{{ap::name::narrator}} 向 {{ap::name::editor}} 提出本轮的问题。"
      param: { name: questions, description: "必答问题全部，加上选答问题里最相关的 2-3 条。只列问题，不解释。" }
  - id: plan
    actor: editor
    kind: text
    label: 规划
    field: { name: "step{{n}}_plan", description: "{{ap::name::editor}} 逐条回答、给出方案与决策，敲定梗概与开头结尾。" }
    tool:
      name: give_plan
      description: "{{ap::name::editor}} 回答问题，给出方案与决策。"
      param: { name: plan, description: "逐条建议 + 待决项与决定 + 梗概/开头/结尾 + 字数与禁用元素提醒。" }
  - id: draft
    actor: narrator
    kind: content
    label: 写作
    field: { name: "step{{n}}_story", description: "{{ap::name::narrator}} 按规划写出本段正文。" }
    tool:
      name: write_story
      description: "{{ap::name::narrator}} 写出本段正文。"
      param: { name: story, description: "完整的正文与附加格式（不要加任何包裹标签）。" }
---
<!-- @section intro style=dialogue -->
{{ap::prefix::narrator}}接着写这个故事。先把 {{ap::name::editor}} 请来，帮我把这轮要想清楚的事过一遍。

{{ap::prefix::editor}}我在。你问，我答。

{{ap::prefix::user}}那就按流程来，一步步确认。

<!-- @section intro style=imperative -->
按下面的阶段依次完成本轮：提问 → 规划 → 写作。每个阶段由指定角色完成，内容分别填进自己的字段，不要跳步、不要合并。

<!-- @section stage:inquire style=dialogue -->
{{ap::prefix::narrator}}我先向 {{ap::name::editor}} 提问：`要考虑的问题` 里「必答问题」逐条问出，「选答问题」里挑 2-3 条最相关的。**只列问题，不解释、不回答。**

<!-- @section stage:inquire style=imperative -->
作为 {{ap::name::narrator}}，把 `要考虑的问题` 中「必答问题」全部、「选答问题」中最相关的 2-3 条逐条提出来。只列问题，不解释。

<!-- @section stage:plan style=dialogue -->
{{ap::prefix::editor}}问题收到。我逐条给建议：先对照 `<基础写作指导>`、写作规则、`<history>` 和已经启用的模块，指出重复、用词过度、结构单调的地方；再对每个关键岔路口摆出待决项与可选项，然后做出决定——必须满足「不降智无 BUG、读者不厌倦、相对最反预期」，环环相扣。最后敲定本段梗概、开头衔接点与结尾落点，并提醒字数和禁用元素。**本阶段不要输出 `<content>` 标记，正文留到写作阶段。**

{{ap::prefix::narrator}}明白，我按这个来。

<!-- @section stage:plan style=imperative -->
作为 {{ap::name::editor}} 逐条回答上面的问题，给出可执行方案：对照 `<基础写作指导>`、写作规则与 `<history>` 指出重复、用词过度、结构单调之处；对每个关键岔路口给出待决项与可选项，并做出满足「不降智无 BUG、读者不厌倦、相对最反预期」的决定；最后敲定梗概、开头衔接点与结尾落点，提醒字数与禁用元素。本阶段不要输出 `<content>` 标记。

<!-- @section stage:draft style=dialogue -->
{{ap::prefix::narrator}}现在动笔。先输出 `<content>`，后面是正文与所有要求的附加格式。严格按上面的规划、`<基础写作指导>`、`<核心文字风格>` 写，约 {{ap::param::length}} 字，{{ap::param::pov}}。

<!-- @section stage:draft style=imperative -->
作为 {{ap::name::narrator}}，把本段正文完整写进「写作」阶段对应的正文字段：正文之后紧接所有被要求的附加格式（状态栏、选项等）。**不要输出 `<content>`、`</content>` 之类的包裹标签**。按上面的规划、`<基础写作指导>` 与 `<核心文字风格>` 写，约 {{ap::param::length}} 字，{{ap::param::pov}}。

<!-- @section outro style=dialogue -->
{{ap::prefix::user}}流程是这样：
```
{{ap::workflow::summary}}
```
没问题的话，现在开始。

<!-- @section outro style=imperative -->
流程：{{ap::workflow::summary}}。现在开始。
