---
timezone: UTC+8
---

# 王谦(Joy Wang)

**GitHub ID:** JoyWQ

**Telegram:** @JoyWQQ

## Self-introduction

目前在一家传统物联网公司做数据预警平台的后端研发工作 主要涉及流式数据处理和复杂规则预警 工作中会用到一些数据挖掘和自然语言学习的小模型 对于Web3、AI、复杂DeFi策略交汇地带抱有浓厚的学习兴趣 希望能通过此次活动更大幅度地提升自己 并结识到优秀、有意思的新朋友 也希望在未来能从事更cool的工作或者事业

## Notes

<!-- Content_START -->
# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->
-   通过FaucetMe网站领取ZetaChain的测试币，并在小狐狸钱包里手动配置ZetaChain的网络
    
-   学习Universal合约模式及核心的on call方法
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->

-   QWEN-AI控制台账号注册（资料里提供的是国际版，国内用户感觉可以自己搜索网页，不然电话验证会很麻烦）
    
-   QWEN API简单调用
    

```
curl -X POST "https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation" -H "Content-Type: application/json" -H "Authorization: Bearer sk-021ed70a929f43fa9b049e8dc6dcf689" -d "{\"model\": \"qwen-turbo\", \"input\": {\"messages\": [{\"role\": \"user\", \"content\": \"请用一句话介绍你自己。\"}]}, \"parameters\": {}}"
{"output":{"finish_reason":"stop","text":"我是一个大型语言模型，能够帮助用户回答问题、创作文字、逻辑推理、编程等多种任务。"},"usage":{"input_tokens":18,"output_tokens":22,"prompt_tokens_details":{"cached_tokens":0},"total_tokens":40},"request_id":"8ed95b94-3c6d-4a2d-a168-c92ce670e82c"}
```

-   Zetachain本地安装
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
