# 第6章 编排与交接：流水线、主管与图

**一句话**：`pipeline` 确定性高、`orchestrator worker` 适合广度探索、`graph orchestration` 表达力最强也最难调试，每个环都要有 `termination condition`。

**为什么需要它**：通信规则定了，还需要一个调度者决定谁先干、何时换人、何时结束。编排是多智能体系统的骨架：同样一组角色，顺序流水线和带环状态图的行为天差地别。编排没设计好，系统会陷入互相谦让或无限循环，把预算烧在原地打转上。

**核心结构**：综合报告口径收敛出几种稳定形态。`pipeline` 是固定接力；`orchestrator worker` 由主执行者拆解、派工、汇总，`subagents` 各持独立上下文；`supervisor` 加 `specialist` 是生产中最常见的形态；`swarm` 向同质池广播后聚合；`blackboard` 经共享工作区中介；`graph orchestration` 用状态图加条件边加环。LangGraph 的 `StateGraph`、`MessagesState` 与 `checkpointer` 提供了图编排的运行时形态。

**关键设计**：`handoff` 是编排的核心接口。好的交接写清目标、格式、工具与禁区，坏的交接只丢一句「你来吧」。综合报告口径建议用确定性代码节点承担路由与格式化，把模型节点留给真正需要判断的部分；图里的每个环必须有 `termination condition` 与计数上限，否则「A 认为该 B 做、B 认为该 A 做」会烧穿预算。

**拆解**：
### 1. 流水线稳在不灵活
`pipeline` 每节点有明确契约，确定性与可测试性最好，适合固定格式的重复业务。代价是流程外任务无处安放，遇到例外只能整条重跑。

### 2. 主管模式防单点
`orchestrator worker` 与 `supervisor specialist` 收益大，但主管是瓶颈。用 `Command` 与 `Send` 这类显式路由替代隐式判断，能让派工可审计、可回放。

### 3. 图编排必须设环限
`graph orchestration` 的环带来迭代能力，也带来近似随机游走的执行路径。每个环配计数上限与退出条件，是调试成本可控的前提。

### 4. 中断要能续
`interrupt` 与 `human-in-the-loop` 让执行在关键点暂停、检查或改状态再继续；`durable execution` 与 `checkpointer` 保证长任务中断后能恢复。生产系统这两项不是可选项。

**今天怎么做**：给你系统里每个循环画一条退出条件，写明最多跑几轮、谁有权终止。再在关键交接点插一个 `interrupt`，人工看一次交接内容是否符合四段任务书。

<div class="chapend" markdown="1">

**容易误解的地方**：以为图编排越灵活越好。缺 `termination condition` 的环会无限打转；能用固定 `pipeline` 解决的流程，不要上状态图。

**记住这一句**：编排先定交接，再定拓扑；有环必设退出，关键处可暂停。

</div>
