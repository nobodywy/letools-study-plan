# 具身智能数据管线入门实录（LeTools · Kuavo）

> 目标：以**具身智能数据管线**为主线入门——用 **LeTools** 跑通「数据转换 → 训练消费 → 离线验证」，用**知乎 Qwen-RobotManip 拆解**建立工业级数据认知。训练/评测用于验证数据可用性，不是主线本身。
>
> 环境：WSL2 Ubuntu 20.04 + RTX 4080 SUPER（16G）｜ 实操 70% + 理论 30% ｜ 无仿真、无真机，评测用 **checkpoint 离线验证**（预测动作 vs 真值画图 + MSE）

## 📍 当前状态

- **阶段**：阶段二收尾 + 阶段三并行（阶段一 ✅；阶段二 M1 ✅，L2 自检待做）
- **下一步**：数据管线 L2 自检 → 具身数据入门 → 精读知乎拆解 → 进入 ACT 训练
- **里程碑**：M1 ✅ ｜ M2 ACT 离线验证 ｜ M3 VLA 微调/服务化 ｜ M4 完整闭环

## 🧭 学习路径

> 顺序逻辑：先跑通（管线）→ 再理解（数据认知）→ 再读论文（理论）→ 训练。

| 顺序 | 文档 | 内容 | 对应阶段 |
|---|---|---|---|
| 主线 | [学习路线](docs/学习路线.md) | 阶段清单 + 里程碑验收标准 | 全程 |
| 1 | [数据管线掌握清单](docs/数据管线掌握清单.md) | 工程侧：管线主线 / 六个必会主题 / 练习 / 自测 | 阶段二 |
| 2 | [具身数据入门](docs/具身数据入门.md) | 五类数据：种类 / 特点 / 阶段 / 价值 / 获取 / 流程 | 阶段三 |
| 3 | [Qwen-RobotManip精读指南](docs/Qwen-RobotManip精读指南.md) | 知乎拆解文的阅读顺序 + 全文骨架 + 自测题 | 阶段三 |
| 备用 | [参考资料](docs/参考资料.md) | 术语 Wiki + 外部链接汇总 | 随时查 |

## 🌟 两份核心资料

| 资料 | 角色 |
|---|---|
| **LeTools**：[代码仓库](https://github.com/LejuRobotics/LeTools-Learning) + [官方文档](https://www.letools.lejurobot.com/docs.html) | **实操主线**：真实工具链，数据管线的主要对象（本地 `~/work/LeTools-Learning`） |
| [知乎：Qwen-RobotManip 拆解](https://zhuanlan.zhihu.com/p/2053583019710190587) | **理论主线**：工业级数据管线、清洗与动作对齐的参照 |

## 🆘 求救渠道

- [LeTools-Learning Issues](https://github.com/LejuRobotics/LeTools-Learning/issues)（附环境信息 + 完整 traceback）
- 官网文档页右下角 AI 助手 / 社区群
- **原则**：环境问题别硬扛，及时求助；显存不够（VLA 阶段）退 SmolVLA 或 AutoDL 租 A100（约 ¥5–8/时）
