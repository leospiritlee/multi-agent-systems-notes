## 序：一份工作，为什么要交给一群 Agent

一个 Agent 接不住的任务，往往不是因为它不够聪明，而是因为一份工作里挤了太多不同的上下文：要查的东西各不相同，用到的工具互相冲突，结论还得有人挑错。多智能体系统的做法是把这份工作拆开，交给几个各司其职的执行者，再设计它们之间怎么说话、怎么汇总、出错怎么算。

这本专题精读写的是这条「协作链路」的十二道关：先判断该不该拆，再走角色分工、交互范式、通信拓扑、编排交接、共识裁判、共享状态、故障归因、算力账、评测与上线。每关回答同一个问题——多引入一个执行者，换来了什么，又带来了什么新代价。

**分工**：与两本 Agent 书不同，那两本讲单 Agent 的循环、工具调用与上下文工程，这本只讲多 Agent 之间的相处方式，单 Agent 的部分只在交接处引用。与《推理模型精读》不同，那本讲一次回答里多花算力，这本讲多个执行者之间怎么分工与互相校验。与《机器学习系统精读》不同，那本讲训练与推理怎么跑起来，这本不碰模型训练推导。

## 协作链路全景图

+----------------------------------------------------------------------+
| MULTI-AGENT SYSTEMS: WHERE DOES THE WORK GO                           |
|                                                                      |
| [1] boundary -> [2] role -> [3] interaction -> [4] topology           |
| [5] communication -> [6] orchestration -> [7] consensus               |
| [8] shared state -> [9] failure -> [10] compute                       |
| [11] eval -> [12] serving & protocol                                  |
|                                                                      |
| one task -> many roles -> structured talk -> verified joint result    |
+----------------------------------------------------------------------+

这张图不是调用顺序，而是「一份工作被拆开之后，多出来的问题依次出现在哪里」：先问该不该拆，再问谁来干、怎么说话、谁来看着、错了怎么办、账怎么算。越往后，代价越隐性，也越需要在上线前想清楚。

## 十二关速查表

| 关 | 主题 | 一句话 |
|---|---|---|
| 1 | 边界 | `multi-agent` 只在任务可拆、信息超窗口、并行有收益时才划算 |
| 2 | 分工 | `role prompt` 写清目标、格式、工具与禁区四段 |
| 3 | 交互 | `generator critic` 打破自己审自己的偏差 |
| 4 | 拓扑 | `centralized` 易调但要防中心瓶颈 |
| 5 | 通信 | `sparse communication topology` 压住 `communication overhead` |
| 6 | 编排 | `orchestrator worker` 配 `handoff` 与 `termination condition` |
| 7 | 共识 | `structured debate` 让错误在收敛前被打断 |
| 8 | 状态 | `checkpoint` 加共享状态，防写入冲突 |
| 9 | 故障 | `failure attribution` 定位真正出错的那一步 |
| 10 | 算力 | `capability saturation` 后协调收益转负 |
| 11 | 评测 | `outcome focused evaluation` 配 `LLM as judge` 与人工复核 |
| 12 | 上线 | `opaque execution` 的 `Agent Card` 划清协作边界 |

## 三层读法：机制、工程、判断

看到一个新方法，先问它属于哪一层。`cooperation`、`competition`、`coopetition` 与 `debate`、`majority voting` 是机制层，论文多半讨论这些；`StateGraph`、`checkpointer`、`Agent Card` 是工程层，官方文档给出接口与形态；`capability saturation`、`cost normalization` 是判断层，告诉你什么时候这些方法根本不该上。

三层不能互相替代。机制漂亮不等于工程上跑得稳，工程能跑也不等于这笔账划算。综合报告口径建议按「先单 Agent 基线、再按可拆性加执行者、最后才谈拓扑与协议」的顺序推进，并且每一步都用自己的题库做对照，而不是照搬别人的基准分数。

## 一个可验收的出口

读完十二关，出口标准不是记住某个框架名，而是能对着一个具体任务回答四句话：这份工作能不能真拆成互相独立的部分；两个执行者之间到底有什么数据越过边界；出错时能不能定位到具体那一步；多花的 `token budget` 换来的收益有没有超过单 Agent 大预算基线。四句都答得出，说明这条链路已经能用于判断，而不只是读过一篇综述。

对做系统的人，还要多一句：跨组织的 `Agent2Agent` 协作里，对方的内部状态对你是 `opaque execution`，你能依赖的只有 `Agent Card` 声明的能力与任务结果。这一句答不出来，前面四句答得再好也落不了地。

## 语料边界与口径

一手论文逐字留存，用于机制与术语的精确写法：AutoGen、CAMEL、MetaGPT、ChatDev、AgentVerse、Generative Agents、AgentScope、多智能体辩论、更多 Agent 是否更好、几份协作与 Agent 综述，以及一篇关于 Agent 系统缩放的量化研究。官方文档给出上线形态：LangGraph、AutoGen、CrewAI、CAMEL、AgentScope、Anthropic、OpenAI Agents SDK、Google ADK、Model Context Protocol、LangSmith 等。

综合报告只补工程判断，比例、成本与趋势一律标注「综合报告口径」。十二关的顺序、场景与速查表属于「延伸，非语料原文」，不加行内代码标记；行内代码只用语料中确实出现的标识符。安全内容只作工程提示，不构成法律意见。