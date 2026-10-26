---
id: transfer-half
name: "🔉 ︱转述-半"
group: transfer
region: post
order: 3310
exclusive: transfer
default: true
vars:
  mandatoryQuestions:
    transferStart: "开头如何改编自 `读者回复` 并衔接上一段正文的结尾？改编到哪一句为止？"
---
# 正文创作边界（基于用户输入的创作范围）
<!-- @style dialogue -->
**本次正文开头必须改编自`<reader-response>`里的内容，使其无缝地、不重叠地、风格统一地衔接上上一个`<content>`里的正文尾部。**其余中间和结尾部分必须要是`<reader-response>`之外的剧情。
<!-- @style imperative -->
**本次正文开头必须改编自`<reader-response>`里的内容，使其无缝地、不重叠地、风格统一地衔接上上一段正文的尾部。**其余中间和结尾部分必须要是`<reader-response>`之外的剧情。
<!-- @end -->
