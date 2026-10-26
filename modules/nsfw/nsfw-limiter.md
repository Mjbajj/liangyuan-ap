---
id: nsfw-limiter
name: "🥵NSFW限速器"
group: nsfw
region: post
order: 2100
vars:
  mjModules: |-
    # NSFW限速
    - 用户输入中没有明显nsfw指令时即便有诸多nsfw相关设定也不得直接进入nsfw互动场景。
    - 一切与<user>相关的nsfw互动都必须徐徐展开。
    - 用户没有写明<user>在nsfw场景高潮/射精时不得私自处理，保持在进行中的状态。
  optionalQuestions:
    mjLimiter: "本段是否必须守住 NSFW 限速？用户有没有给出明确的 NSFW 指令？"
---
