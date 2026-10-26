---
id: user-role-guard
name: "⚙️防抢话"
group: userRole
region: post
order: 3410
exclusive: userRole
vars:
  optionalQuestions:
    userBoundary: "本段 `<user>` 必须完全没有行动与台词，怎么保证？"
---
# {{user}}内容准则：
- 仅有用户允许创作{{user}}的互动内容
- 你创作的内容中，{{user}}无**任何** 行动/台词，不进行{{user}}的心理描写，若明确输入<user>台词则默认其他角色已收到
- 聚焦故事中的其他角色
- 该准则内容为**第一优先**，忽略前文的互动模式
