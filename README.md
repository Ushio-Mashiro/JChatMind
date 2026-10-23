
# AI智能体助手-JChatMind

这是一个智能 AI Agent 系统，基于 Spring AI 框架构建，实现了自主决策、工具调用和知识库检索等核心能力。

系统采用 **Think-Execute 循环机制，能够理解复杂任务、规划执行步骤、调用外部工具，并基于 RAG 技术从知识库中检索相关信息，完成多步骤的复杂任务**。

### 项目亮点

1、**Agent Loop（Think-Execute 循环）

* 多轮规划
* 多轮工具调用
* 状态管理（THINKING / EXECUTING / DONE / ERROR）
* 错误处理与最大步数控制（防止无限循环）


2、**工具系统（固定工具 + 可选工具，可扩展、可治理**）

很多人做工具调用只是“写几个 if else”，JChatMind 的工具系统是“框架化”的：

* 工具自动注册
* 固定工具 / 可选工具分类管理
* 可扩展：新增工具不改核心流程
* 可控：禁用 Spring AI 自动执行，改为手动管理 ToolCalling 流程

这里的技术点：“工具调用怎么做扩展？工具失败怎么处理？工具返回结果怎么进入对话历史？”

这就是讲“系统设计”的地方。

3、**RAG 知识库（PostgreSQL + pgvector**）

RAG 不是 PPT 概念，JChatMind 是完整链路：

* Markdown 文档解析、分块
* Embedding 生成并落库
* pgvector 相似度检索（<->）
* ivfflat 索引优化，支持 10 万+向量

而且最关键的点是：用 PostgreSQL 一套体系把结构化数据和向量数据都管了（部署简单、成本低、事务一致性好）

4、**多模型支持（注册表模式 ChatClientRegistry**）

项目不是“绑定一个模型”，而是：

* DeepSeek / 智谱 AI 可切换
* 统一 ChatClient 接口
* 注册表模式管理模型实例（解耦创建与使用）
* 便于未来扩展更多模型

这里也涉及到：如果要加一个新模型要改哪些代码？怎么做到无侵入？

5、**SSE 实时通信（执行过程实时可视化**）

很多 Agent 项目体验很差：用户不知道系统在干嘛。

JChatMind 用 SSE 做了：

* 状态实时推送：THINKING / EXECUTING / DONE
* 前端能实时看到“Agent 正在干啥”
* 比 WebSocket 更简单，适合单向推送


