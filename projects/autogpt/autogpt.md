---
marp: true
---

# Autogpt

**最火的AI Agent应用。Github 157k Star**

## 项目Github地址 https://github.com/Significant-Gravitas/AutoGPT

---

# 大纲

1. 安装使用
2. 演示
3. 代码原理讲解

---

# 安装使用

由于最新版包含了web应用，部署框架等更加庞大和复杂的功能，我们这里不会介绍。只介绍核心代码
> 下载
```
git clone https://github.com/Significant-Gravitas/AutoGPT.git
```
> 安装poetry

一个python dependency managing software,https://python-poetry.org/docs/#installing-with-the-official-installer

> 换poetry源：autogpts/autogpt/pyproject.toml
```
[[toos.poetry.source]]
name = "aliyun"
url = "http://mirrors.aliyun.com/pypi/simple"
default = true
```
---
> 安装各种依赖 
```
pip install -i https://mirrors.cloud.tencent.com/pypi/simple/ -e .
```
> 配置apikey
```
cp .env.template .env
# 修改内容
```
> 启动cli程序

```
python -m autogpt run --gpt4only -c
```
___

# 演示
通过一个例子来演示autogpt
> 写一个新闻门户前端app，不可以用前端框架，只能使用HTML+CSS+JavaScript

可以看到，AI Agent之于工具增强，更多体现在利用prmopt + 代码逻辑，让大模型自主规划完成复杂的任务。

---

# 代码&原理

## 核心prompt

## 代码原理/主流程

```
sequenceDiagram
		Main ->> Main: run_auto_gpt,入口
		Main ->> Main: run_auto_gpt，輸入task
    Main ->> Main: generate_agent_profile_for_task，調用大模型，返回任務的額外信息，通過openai function能力
    Main ->> Main: run_interaction_loop,入口
		loop cycles_remaining > 0
			Main ->> BaseAgent: autogpts/autogpt/autogpt/agents/base.py
			BaseAgent ->> BaseAgent: propose_action()
			BaseAgent ->> BaseAgent: build_prompt()
			BaseAgent ->> CommandRegistry: list_available_commands
			CommandRegistry ->> BaseAgent: commands
			BaseAgent ->> PromptStrategy: build_prompt(commands)
      PromptStrategy ->> BaseAgent: prompt
			BaseAgent ->> LlmProvider: create_chat_completion
			BaseAgent ->> BaseAgent: self.on_before_think(prompt, scratchpad=self._prompt_scratchpad)
			LlmProvider ->> Agent: raw_response
      BaseAgent ->> BaseAgent: on_response(llm_response,prompt,scratchpad)
			BaseAgent ->> BaseAgent: parse_and_process_response(llm_response,prompt,scratchpad)
			BaseAgent ->> Agent: parse_and_process_response(llm_response,prompt,scratchpad)
			alt cycles_remaining > 0
				Agent --> Main: command,command_args
				Main --> Agent: execute(command,command_args,input)
      end

		Main ->> Main: cycles_remaining--
    end

```


---
# 总结
AI Agent之于大模型工具增强，核心在于planning，通过提示词结合代码逻辑，让大模型有了规划的能力，能将一个复杂的任务进行拆分规划。

**但是，从演示可以看出，大模型真正要完成一个复杂的任务，结果还是不可控。在软件工程/系统架构领域，解决复杂问题的不变法则就是分治，如何将分治思想应用在大模型上呢?这就引出了新的大模型应用范式，multi-agent。就是将复杂任务分治成更加垂直的单一域任务，每个子agent更专注，然后可以通过DAG图或其他方式来管理agent的拓扑关系。**
> 举例，同样是写新闻门户应用。可以分成5个agent
* 产品经理agent：发布详细需求
* 设计agent：设计页面布局
* 开发agent：写应用
* 测试agent：review代码，测试应用
* 等等
