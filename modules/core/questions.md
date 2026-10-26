---
id: questions
name: 要考虑的问题
group: core
region: post
order: 3800
toggle: always
when: 'workflow != "plain"'
vars:
  mandatoryQuestions:
    intent: "用户这次输入里，具体的情节动作、内容要素、情绪与节奏要求分别是什么？"
    world: "当前的世界观、类型、年代是什么？本段是维持还是切换？"
    boundary: "本次正文的创作边界到哪里——复述/改编用户输入多少，从哪里接上，往哪个方向推进多少？"
    scene: "当前场景：谁在场、在何处、各自的处境与关系状态如何？"
    style: "按 `<核心文字风格>`，本段要做哪些风格化处理？句式、节奏、意象与用词怎么统一？"
  optionalQuestions:
    impact: "这一段要给读者造成什么情绪冲击或张力？"
    repetition: "前文有哪些用滥的意象、句式、桥段必须避开？"
    twist: "有没有引入转折、意外或新元素的机会？"
---
## 要考虑的问题

### 必答问题
{{ap::var::mandatoryQuestions::values}}

### 选答问题
从下面这些里挑 2-3 条与本轮最相关的提问：
{{ap::var::optionalQuestions::values}}

### 决策纪律
每一个关键岔路口都要先摆出"非此即彼"的待决项和高水平可选项，再做出决定。决定必须同时满足：1）不降智、无逻辑 BUG；2）读者不会厌倦；3）相对最反预期。
