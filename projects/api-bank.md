<img width="461" alt="image" src="https://github.com/fackee/llm_based_app_develop/assets/16185950/5ae24114-fc56-4bd7-b9ce-aec654f57c28"># api-bank
项目github：https://github.com/AlibabaResearch/DAMO-ConvAI/tree/main/api-bank

### 代码实现原理

#### 核心prompt


#### 总结

api-bank主要是设计了一套框架对大模型使用工具/API的能力做了基准测试。
启发：既然大模型能够理解api,那系统的架构方式，集成调用模式是不是可以有新的创新？
![]()

下一期：autogpt

## 部署补充事项
有几个点可能需要改一下：
1. tool_search.py的best_match_api方法，最好把embedding模型下载下来，可能会因为墙的原因卡住
<img width="799" alt="image" src="https://github.com/fackee/llm_based_app_develop/assets/16185950/e40376fa-2abe-42ea-b7f0-d6975d9dcafa">
2. 被墙了需要开启本地代理
<img width="461" alt="image" src="https://github.com/fackee/llm_based_app_develop/assets/16185950/3202d11d-1fd5-45ed-9696-0de00bbc321e">
3. 有些本地部署web访问权限问题，server_name改成0.0.0.0
<img width="1121" alt="image" src="https://github.com/fackee/llm_based_app_develop/assets/16185950/3a003849-acb5-4f9c-bf26-8decd58e93a2">
4. 可能因为openai/request库版本过低问题报错，修改utils.py的ChatGPTWrapper的init方法以及call方法
<img width="1375" alt="image" src="https://github.com/fackee/llm_based_app_develop/assets/16185950/92223f30-188d-4c87-a934-e4a5c48375de">
