---
layout: post
title:  "AI领域四大件"
categories: AI
tags:  AI
author: FTX
---

* content
{:toc}
# RAG 与 Agent 实战入门指南

> 本文整理自个人学习笔记，系统梳理 RAG 与 Agent 的核心概念、API 调用技巧、提示词工程及 LangChain 实战开发，助你快速上手 LLM 应用开发。

---

## 一、什么是 RAG 与 Agent？

在正式动手之前，先理解两个核心概念：

| 概念 | 说明 |
|------|------|
| **RAG**（检索增强生成） | 将企业私域数据与大模型结合，让大模型能够"看懂"私有知识，回答更准确、更专业 |
| **Agent**（智能体） | 相当于给大模型装上"手脚"，使其能够自主规划、总结、市场调研、处理复杂流程等 |

简而言之：**RAG 解决"大模型不知道"的问题，Agent 解决"大模型不会做"的问题。**

---

## 二、API Key 与环境配置

### 2.1 获取阿里云百炼 API Key

注册阿里云百炼平台，创建 API Key（免费额度有效期 3 个月）。拿到 Key 后第一时间配置到**系统环境变量**中，而不是硬编码在代码里。

### 2.2 环境变量配置（Windows）

```
搜索「高级系统设置」→「环境变量」→ 新建用户变量
变量名：DASHSCOPE_API_KEY
变量值：你的API Key
```

> 重启终端后，环境变量生效，代码中无需再暴露任何敏感信息。

### 2.3 快速验证连接

```python
from openai import OpenAI

client = OpenAI(
    api_key=os.environ["DASHSCOPE_API_KEY"],  # 从环境变量读取，安全！
    base_url="https://dashscope.aliyuncs.com/compatible-mode/v1",
)

response = client.chat.completions.create(
    model="deepseek-v3",
    messages=[{"role": "user", "content": "你是谁？"}],
    extra_body={"enable_thinking": True},
    stream=True
)

# 流式打印思考过程 + 回复
is_answering = False
for chunk in response:
    delta = chunk.choices[0].delta
    if hasattr(delta, "reasoning_content") and delta.reasoning_content:
        if not is_answering:
            print("\n====== 思考过程 ======")
            is_answering = True
        print(delta.reasoning_content, end="", flush=True)
    if hasattr(delta, "content") and delta.content:
        if is_answering:
            print(delta.content, end="", flush=True)
```

---

## 三、本地模型部署（Ollama）

如果阿里云免费额度用尽，或对数据隐私有严格要求，可以利用 Ollama 在本地部署开源大模型。

**Windows 安装步骤：**

1. 官网下载 Ollama 并安装
2. 命令行拉取模型：
   ```bash
   ollama run qwen3:14b      # 14B 参数模型，需 16GB+ 显存
   ollama run llama3.2:3b    # 3B 参数模型，8GB 内存即可运行
   ```
3. 本地访问地址：`http://localhost:11434/v1`

> 本地模型不消耗 Token，但受硬件性能限制。

---

## 四、OpenAI 库高效调用指南

阿里云百炼、Ollama 均兼容 OpenAI SDK 接口，以下是核心用法。

### 4.1 基本调用

```python
from openai import OpenAI

client = OpenAI(base_url="https://dashscope.aliyuncs.com/compatible-mode/v1")

response = client.chat.completions.create(
    model="qwen3-max",
    messages=[
        {"role": "system", "content": "你是Python编程专家，回答简洁"},
        {"role": "assistant", "content": "好的，我是Python专家，你要问什么？"},
        {"role": "user", "content": "用Python输出1-10的数字"}
    ]
)

print(response.choices[0].message.content)
```

> `messages` 列表的三种角色：`system` 定规则、`assistant` 模拟历史、`user` 用户提问。

### 4.2 流式输出

流式输出让用户看到逐字生成，体验更流畅：

```python
response = client.chat.completions.create(
    model="qwen3-max",
    messages=[{"role": "user", "content": "写一首诗"}],
    stream=True
)

for chunk in response:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end=" ", flush=True)
```

### 4.3 带历史上下文的对话

```python
messages = [
    {"role": "system", "content": "你是专业金融顾问"},
    {"role": "user", "content": "小红有两条宠物狗"},
    {"role": "assistant", "content": "好的，已记录"},
    {"role": "user", "content": "小明有3只猫"},
    {"role": "assistant", "content": "好的，已记录"},
    # 模型能"记住"前面对话，一共有5只宠物
    {"role": "user", "content": "一共有几只宠物？"}
]
```

---

## 五、提示词工程（Prompt Engineering）

提示词是调用大模型最核心的技能，以下是经过验证的六大技巧：

### 5.1 六大黄金技巧

| 技巧 | 说明 | 示例 |
|------|------|------|
| **描述详细** | 提供充足背景信息 | ❌ 帮我写文案 → ✅ 为25-35岁程序员写一篇Vue3技术文章，800字 |
| **设定角色** | 让模型扮演专家角色 | "你是一位资深金融分析师..." |
| **分隔符标注** | 用 `##` 或 ````json```` 等标记输入边界 | `##输入##` ... `##结束##` |
| **分步引导** | 指导模型按步骤执行 | "第一步...第二步...第三步..." |
| **提供示例** | Few-shot Learning，让模型模仿 | 输入→输出的示例对 |
| **参考文本** | 提供知识库，抑制幻觉 | 将合同文本作为上下文传入 |

### 5.2 实战案例：金融文本分类

让大模型对金融文本进行四分类：**新闻报道、财务报告、公司公告、分析师报告**。

```python
from openai import OpenAI

client = OpenAI(base_url="https://dashscope.aliyuncs.com/compatible-mode/v1")

# 示例数据
examples = {
    "新闻报道": "今日，股市经历震荡，受宏观经济数据和全球贸易局势影响...",
    "财务报告": "公司年度财务报告显示盈利稳步增长，资产负债表表现强劲...",
    "公司公告": "本公司高兴宣布完成最新一轮并购，收购了AI领域领先的公司...",
    "分析师报告": "科技公司创新将成为未来增长主要推动力，云计算和AI是关键..."
}

questions = [
    "央行宣布降息以刺激经济增长...",
    "ABC公司成功完成股权收购...",
    "无法分类的一句话...",
]

messages = [
    {"role": "system", "content": "你是金融专家，分类为['新闻报道','财务报告','公司公告','分析师报告']，不清楚的返回'不清楚类别'"}
]

# 注入示例学习
for key, value in examples.items():
    messages.append({"role": "user", "content": value})
    messages.append({"role": "assistant", "content": key})

# 批量分类
for q in questions:
    response = client.chat.completions.create(
        model="qwen3-max",
        messages=messages + [{"role": "user", "content": f"分类：{q}"}]
    )
    print(response.choices[0].message.content)
```

### 5.3 实战案例：金融文本信息抽取

从文本中结构化抽取：日期、股票名称、开盘价、收盘价、成交量。

```python
import json
from openai import OpenAI

client = OpenAI(base_url="https://dashscope.aliyuncs.com/compatible-mode/v1")

schema = ["日期", "股票名称", "开盘价", "收盘价", "成交量"]

examples = [
    {
        "content": "2023-01-10，股票强大科技A股开盘价100元，收盘102元，成交量52万",
        "answers": {"日期": "2023-01-10", "股票名称": "强大科技A股", "开盘价": "100元",
                    "收盘价": "102元", "成交量": "52万"}
    }
]

messages = [
    {"role": "system", "content": f"从文本中抽取{schema}，以JSON格式输出，未提及的填'原文未提及'"}
]

for ex in examples:
    messages.append({"role": "user", "content": ex["content"]})
    messages.append({"role": "assistant", "content": json.dumps(ex["answers"], ensure_ascii=False)})

for q in ["2025-06-16，股票传智教育开盘66元，收盘68元..."]:
    resp = client.chat.completions.create(
        model="qwen3-max",
        messages=messages + [{"role": "user", "content": q}]
    )
    print(resp.choices[0].message.content)
```

### 5.4 实战案例：文本语义匹配

判断两个金融句子是否语义一致：

```python
client = OpenAI(base_url="https://dashscope.aliyuncs.com/compatible-mode/v1")

examples = {
    "是": [
        ("公司ABC发布季度财报，显示盈利增长。", "财报披露，公司ABC利润上升。"),
        ("央行降息，刺激经济增长。", "利率下调以提振经济。")
    ],
    "不是": [
        ("黄金价格下跌，投资者抛售。", "外汇市场交易额创新高。"),
        ("油价下跌，能源公司面临挑战。", "新能源技术持续创新。")
    ]
}

questions = [
    ("利率上升，影响房地产市场。", "高利率对房地产有一定冲击。"),   # 语义一致 → 是
    ("油价大跌，能源公司面临挑战。", "智能城市建设趋势明显。"),    # 语义无关 → 不是
]

messages = [
    {"role": "system", "content": "判断两个句子语义是否一致，回答'是'或'不是'"}
]

for label, pairs in examples.items():
    for s1, s2 in pairs:
        messages.append({"role": "user", "content": f"句子1:[{s1}]，句子2:[{s2}]"})
        messages.append({"role": "assistant", "content": label})

for q1, q2 in questions:
    resp = client.chat.completions.create(
        model="qwen3-max",
        messages=messages + [{"role": "system", "content": f"句子1:[{q1}]，句子2:[{q2}]"}]
    )
    print(resp.choices[0].message.content)
```

---

## 六、JSON 数据格式速查

JSON 是程序间数据交换的主流格式，需要熟练掌握。

| 格式 | Python 对应 | 示例 |
|------|------------|------|
| JSON 对象 `{}` | 字典 `dict` | `{"name": "周杰伦", "age": 31}` |
| JSON 数组 `[]` | 列表 `list[dict]` | `[{"name":"A"},{"name":"B"}]` |

```python
import json

# 字典 → JSON 字符串
d = {"name": "周杰伦", "age": 31, "gender": "男"}
s = json.dumps(d, ensure_ascii=False)
print(s)  # {"name": "周杰伦", "age": 31, "gender": "男"}

# JSON 字符串 → 字典
res = json.loads('{"name": "周杰伦", "age": 31}')
print(res, type(res))
```

---

## 七、LangChain 入门：从 0 到 1

LangChain 是构建 LLM 应用的核心框架，封装了大量实用组件，大幅提升开发效率。

### 7.1 核心组件一览

```
LangChain 核心组件
├── Models        — 大语言模型调用（ChatGPT、Llama 等）
├── Prompts       — 提示词模板管理
├── Chains        — 链式调用，将多个组件串联
├── Memory        — 对话记忆（临时 / 长期）
├── Document Loaders — 文档加载器（PDF、TXT、JSON 等）
├── Embeddings    — 文本向量化
├── Vector Stores — 向量数据库存储与检索
└── Retrievers   — 知识检索
```

### 7.2 模型调用

```python
from langchain_ollama import OllamaLLM

model = OllamaLLM(model="gemma3:1b")
result = model.invoke(input="你是谁？能做什么？")
print(result)
```

### 7.3 提示词模板

```python
from langchain_core.prompts import ChatPromptTemplate

# Few-shot 模板：带示例的提示词
prompt = ChatPromptTemplate.from_template(
    "你是金融专家，将文本分类为{categories}，参考示例：{examples}"
)
# format() 或 invoke() 传参
prompt.invoke({"categories": ["新闻", "公告"], "examples": "..."})
```

### 7.4 链（Chain）：串联调用

```python
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough

# 核心模式：Prompt → Model → OutputParser
chain = prompt | model | StrOutputParser()
result = chain.invoke({"question": "什么是RAG？"})
```

### 7.5 记忆（Memory）

| 类型 | 说明 |
|------|------|
| **临时记忆** | 当前会话内的对话历史 |
| **长期记忆** | 持久化存储，跨会话使用 |

### 7.6 文档加载与分割

```python
from langchain_community.document_loaders import PyPDFLoader, TextLoader

# PDF 文档
loader = PyPDFLoader("report.pdf")
pages = loader.load()

# 文本文件
loader = TextLoader("policy.txt")
docs = loader.load()

# 分割长文档（为 RAG 做准备）
from langchain.text_splitter import RecursiveCharacterTextSplitter
splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
chunks = splitter.split_documents(docs)
```

### 7.7 RAG 实战：向量检索 + 生成

完整 RAG 流程如下：

```
用户提问 → 检索相似文档 → 组装提示词 → 调用大模型 → 返回答案
```

```python
# 1. 加载文档并分割
loader = PyPDFLoader("知识库.pdf")
docs = loader.load()
chunks = splitter.split_documents(docs)

# 2. 向量化存入向量数据库（示例伪代码）
vectorstore = FAISS.from_documents(chunks, embeddings)
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})  # 取 Top-3 相关文档

# 3. 构造 RAG 链
rag_chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | model
    | StrOutputParser()
)

# 4. 提问
answer = rag_chain.invoke("公司的退款政策是什么？")
print(answer)
```

---

## 八、总结与展望

本文涵盖了以下核心知识点：

- **API 调用**：阿里云百炼 + Ollama，环境变量安全配置
- **OpenAI SDK**：模型调用、流式输出、上下文记忆
- **提示词工程**：六大技巧 + 金融领域三个实战案例
- **JSON 数据格式**：结构化数据处理基础
- **LangChain**：组件体系、RAG 全流程开发

后续可深入探索的方向：
- 向量数据库进阶（Milvus、ChromaDB、Pinecone）
- Agent 架构（ReAct、Plan-and-Execute 模式）
- 多模态 RAG（支持 PDF 图片、表格理解）
- 生产级部署（LangServe、LangSmith 监控）

---

*本文基于个人学习笔记整理，供学习参考。如有疏漏，欢迎指正。*