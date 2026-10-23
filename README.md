# LeTools 具身智能入门实录（Kuavo / VLA）

> 目标：跑通 [LeTools-Learning](https://github.com/LejuRobotics/LeTools-Learning) 工具链「数据 → 训练 → 离线评测」完整闭环，同步建立 VLA 理论视野。
>
> 环境：WSL2 Ubuntu 20.04 + RTX 4080 SUPER（16G）｜ 实操 70% + 理论 30% ｜ 无仿真、无真机，评测用 **checkpoint 离线验证**（预测动作 vs 真值画图 + MSE）

## 进度

**▓▓▓░░░░░░░ 30%** ｜ 当前：📖 数据理论集中学习（按师兄路线）｜ 明细见[进度日志](logs/进度日志.md)

## 🧭 数据理论路线（师兄建议）

> 先认识数据本身，再看数据怎么来。

**种类 → 各类特点 → 模型阶段适用性 → 对模型的价值** ⏭️ **获取方法 → 处理流程**

落地文档：[具身数据入门](docs/具身数据入门.md)（上篇认知 / 下篇实操）

## 🗂 文档导航

| 文档 | 内容 |
|---|---|
| [学习路线](docs/学习路线.md) | 阶段清单 + 里程碑验收标准 |
| [具身数据入门](docs/具身数据入门.md) | 五类数据：种类 / 特点 / 阶段 / 价值 / 获取 / 流程 |
| [Qwen-RobotManip精读指南](docs/Qwen-RobotManip精读指南.md) | 知乎拆解文的精读路线 + 全文骨架 + 自测题 |
| [参考资料](docs/参考资料.md) | 具身智能术语 Wiki + 核心链接汇总 |
| [进度日志](logs/进度日志.md) | 按日期的学习记录 |

## 🌟 三份核心资料

| 资料 | 角色 |
|---|---|
| [LeTools-Learning](https://github.com/LejuRobotics/LeTools-Learning) | **实操主线**（本地 `~/work/LeTools-Learning`） |
| [LeTools 官方文档](https://www.letools.lejurobot.com/docs.html) | **主线说明书**（零基础教程 + 页面右下角 AI 助手） |
| [知乎：Qwen-RobotManip 拆解](https://zhuanlan.zhihu.com/p/2053583019710190587) | **理论线主文** |

## 🆘 求救渠道

- [LeTools-Learning Issues](https://github.com/LejuRobotics/LeTools-Learning/issues)（附环境信息 + 完整 traceback）
- 官网文档页右下角 AI 助手 / 社区群
- **原则**：环境问题卡超过半天就求助；显存不够（VLA 阶段）退 SmolVLA 或 AutoDL 租 A100（约 ¥5–8/时）
