---
layout: post
title:  "Awesome HTMLtoWord"
categories: Word
tags:  Word
author: FTX
---

* content
{:toc}

# 使用 HTML 导出 Word / Excel 文档完整指南

> 本文记录如何借助大模型生成 HTML 格式代码，变相导出符合 Word 或 Excel 格式要求的文档。

---
![HTMLtoWord](/images/HTMLtoWord.png)





## 核心思路
通过 HTML 格式代码，在浏览器中渲染页面后下载为 `.doc` 或 `.xls` 文件，用 Office 打开即可保持格式。

---

## 推荐模型

**DeepSeek** 或其他支持代码生成且上下文窗口较大的模型，要求能：
- 理解复杂的格式要求（字号、字体、行距）
- 生成完整的、可直接运行的 HTML 代码
- 支持长输出，不截断内容

---

## 提示词示例

### 示例一：导出幼儿园综合素质试卷

```
请根据历年《综合素质》（幼儿园）考试试卷内容，模拟帮我一套试题，满分150分，29个单选，3个材料分析题，一个写作题，单选要有ABCD四个选项，简答题和作文题分别要留出答题位置，作文要有正式的作文页面，匹配答案和解析。

保留原格式，一级标题用四号宋体，加粗，二级标题用小四号黑体，加粗显示，正文用五号宋体，行距设置为1.5倍，将以上信息以html格式输出，要能直接在页面成功下载word格式文件，并且有下载word格式文档的按钮，保留所有信息，不要压缩和漏掉任何细节。
```

---

## 格式对照表

| 格式要求(这部分可以交给大模型处理) | CSS 对应写法 |
|--------------------|-------------|
| 四号宋体加粗             | `font-family: SimSun; font-size: 14pt; font-weight: bold;` |
| 小四号黑体加粗            | `font-family: SimHei; font-size: 12pt; font-weight: bold;` |
| 五号宋体               | `font-family: SimSun; font-size: 10.5pt;` |
| 1.5倍行距             | `line-height: 1.5;` |

> 💡 Word 字号对应：四号=14pt，小四=12pt，五号=10.5pt

---

## 使用步骤

1. **编写提示词** → 明确内容、结构、格式、下载要求
2. **发送模型** → 生成完整 HTML 代码
3. **浏览器运行** → 复制代码保存为 `.html` 并打开
4. **点击下载** → 获得 `.doc` / `.xls` 文件

---

## 常见问题

### Q1: 下载的 Word 文件用 WPS 打开格式错乱？

确保使用 Windows 系统字体（`SimSun`、`SimHei`），建议用 Microsoft Word 打开。

### Q2: 代码被截断？

提示词强调 **"一次性输出完整代码"**，或让模型续写。

### Q3: Excel 乱码？

添加 BOM 头 `\ufeff` 并设置 `charset=utf-8`。

---

## 总结
![效果图](/images/HTMLtoWord02.png)

使用 HTML 导出有格式要求的文档，无需安装额外软件，纯浏览器操作，格式控制精确，适合快速生成标准化文档。