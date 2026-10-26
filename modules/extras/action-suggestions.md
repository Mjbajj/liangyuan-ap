---
id: action-suggestions
name: "<行动:suggestion>"
structures: [options]
group: suggestions
region: post
order: 3620
exclusive: suggestions
vars:
  optionalQuestions:
    actionOptions: "8 个行动的行为类别怎么分？描述要遵守什么？"
---
# 行动选项

- 本轮结束时另外交回 8 个供 {{ap::name::user}} 选择的行动选项。
- label 写「行为类别」（5 字以内）；content 写「行为描述」。
- 每个行为要符合：1）第一人称，主观的描述；2）合理连贯、与前文无逻辑矛盾；3）读者更可能感兴趣，不导向死胡同。
- 所有「行为描述」都**不确定**该行为发生后的影响、结果或后续。

{{ap::structure::options}}
