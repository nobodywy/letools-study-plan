# LeTools 十天入门计划（具身智能 / VLA）

> 目标：跑通 LeTools-Learning 工具链「数据 → 训练 → 离线评测」完整闭环，同步建立 VLA 理论视野。
>
> 环境：WSL2 Ubuntu + RTX 4080 SUPER（16G）｜ 节奏：实操 70% + 理论 30% ｜ 无仿真、无真机，评测用 **checkpoint 离线验证**（预测动作 vs 真值画图 + MSE）

## 文档索引

| 文档 | 内容 |
|---|---|
| 本文档 | 10 天日程 + 当前进度 |
| [具身数据入门](具身数据入门-种类与处理流程.md) | 五类数据的特点、训练阶段定位、获取与处理流程 |
| [参考资料](参考资料.md) | 具身智能术语 Wiki + 知乎 Qwen-RobotManip 拆解 + 全部链接 |

---

## 一、三份核心资料

| 资料 | 角色 | 怎么用 |
|---|---|---|
| [LeTools-Learning](https://github.com/LejuRobotics/LeTools-Learning) | **实操主线** | 「Rosbag 数据转换 → 策略训练 → 离线评测」工具链，代码在这里跑 |
| [LeTools 官方文档](https://www.letools.lejurobot.com/docs.html) | **主线说明书** | 核心是《零基础完整学习教程》八部分；页面右下角 AI 助手可随时提问 |
| [知乎：Qwen-RobotManip 拆解](https://zhuanlan.zhihu.com/p/2053583019710190587) | **理论线** | 数据工程 / 动作表示 / 架构 / 评测，按章节拆开晚上读 |

---

## 二、当前进度（2026-10-21）

- ✅ **环境搭建（Day 2）**：conda env `letools`（Python 3.12 + torch 2.9.1 cu128），GPU 直通正常，hydra / lerobot 可导入；ROS Noetic 装在 `/opt/ros/noetic` 供数据转换用
- ✅ **数据准备（Day 3，里程碑 M1）**：`task1_0001.bag` → LeRobot v3 → `~/work/data_out/lerobot`（1 episode / 153 帧 / 3 相机 / 16 维双臂+夹爪，eef_type=rq2f85）
- ⏳ **当前：Day 4 —— 训练第一个策略（ACT）**

---

## 三、学习日程

### Day 1 — 概念 + 全局认知

- [ ] 读 `beginner_tutorial.md` 第一部分：模仿学习 vs VLA、ACT / Diffusion / PI0 选型
- [ ] 读 `get_started/letools.md`，搞懂 `configs` / `kuavo_data` / `kuavo_model` / `kuavo_deploy` / `kuavo_server` 各自职责
- [ ] 快速浏览 `quick_start.md`，对「转换 → 训练 → 评测」三步建立整体印象

### Day 2 — 环境搭建 ✅

- [x] Miniforge + Python 3.12 环境（conda env `letools`）
- [x] 克隆仓库，安装 torch cu128，`nvidia-smi` / `torch.cuda.is_available()` 验证通过

### Day 3 — 数据准备 ✅ 里程碑 M1

- [x] 跑通 `CvtRosbag2Lerobot.py`：`task1_0001.bag` → LeRobot v3 数据集
- [x] 配置对齐：`eef_type=rq2f85`、640×480
- [ ] 🌙 知乎 §1.1.1–1.1.2：机器人数据 + 人类第一视角数据的构成
- [ ] 📖 延伸阅读：[具身数据入门](具身数据入门-种类与处理流程.md)（五类数据怎么分工）

### Day 4 — 训练第一个策略（ACT）⏳

- [ ] 按 `beginner_tutorial.md` 第六部分 + `lerobot_training.md`：读懂 `act.yaml` 每个字段
- [ ] 先 `--dry-run` 看解析结果，再小 steps（如 5000）跑通训练，盯 loss 曲线，找到 checkpoint 输出目录
- [ ] 🌙 ACT 核心思想 30 分钟科普（CVAE + action chunking）

### Day 5 — 推理链路 + 离线评测 ✅ 里程碑 M2

- [ ] 读 `inference.md` + `kuavo_deploy/eval.py` 源码，理清推理链路（只读代码，不跑仿真）
- [ ] 离线验证 Day 4 的 checkpoint：对数据集 episode 逐帧推理，预测动作与真值画在同一张图上看贴合程度
- [ ] 算量化指标（各关节 MSE / 误差曲线），写进笔记
- [ ] 🌙 知乎 §2.1：VLM backbone + DiT action expert 解耦架构

### Day 6 — 对比第二个策略

- [ ] 再训一个 `diffusion`，对比配置与效果差异
- [ ] 了解 `--launcher accelerate` 多卡训练用法
- [ ] 🌙 知乎 §2.2–2.3：80 维统一动作表示、camera-frame delta EEF

### Day 7 — VLA 模型实操 ✅ 里程碑 M3

- [ ] 按 `model_training.md` 下载预训练模型（PaliGemma + pi0_base / pi05_base）
- [ ] 微调 PI0 或 PI05，注意 quantile 统计文件的生成
- [ ] 显存不足的替代方案：改训 `smolvla`，或只跑通 `kuavo_server` 外部模型服务化流程

### Day 8 — 理论深化日（知乎精读）

- [ ] 白天精读知乎 §1 数据篇（全文最重要）：Human-to-Robot 数据合成、清洗五阶段、跨模态质检、ECoT
- [ ] 晚上读 §2 剩余（in-context policy adaptation）+ §3（post-training）
- [ ] 补 30 分钟 flow matching 直觉理解
- [ ] 对照 Day 3 实操，体会「工具链版」和「工业版」差距

### Day 9 — 综合项目 ✅ 里程碑 M4

- [ ] 独立完整走一遍：数据 → 转换 → 训练 → 离线评测 → 结果分析
- [ ] 🌙 知乎 §4：OOD 评测与消融，理解「**alignment first, then scale**」
- [ ] 有余力：ACT / Diffusion / SmolVLA 同数据离线指标对比表

### Day 10 — 收尾与进阶

- [ ] 选学 `bring_policies.md`，尝试接入一个新策略
- [ ] 写一篇学习笔记
- [ ] 自测：「模仿学习和 VLA 的区别」「为什么动作空间对齐比堆数据更重要」
- [ ] 后续路线：精读原论文（ACT → Diffusion Policy → PI0）、LeRobot 源码

---

## 四、里程碑总览

| 里程碑 | 时间 | 验收标准 |
|---|---|---|
| M1 | Day 3 ✅ | 独立完成 Rosbag → LeRobot 数据转换 |
| M2 | Day 5 | ACT checkpoint 离线验证：预测动作与真值基本贴合 |
| M3 | Day 7 | 完成一个 VLA 模型（PI0/PI05）的微调或服务化部署 |
| M4 | Day 9 | 独立完成「数据 → 训练 → 离线评测」完整闭环 |
| 输出 | Day 10 | 一篇学习笔记 + 两个自测问题答得上来 |

---

## 五、求救渠道

- [GitHub Issues](https://github.com/LejuRobotics/LeTools-Learning/issues)（附环境信息 + 完整 traceback）
- 官网文档页右下角 LeTools AI 助手 / 社区群（见 `community.md`）
- **节奏原则**：环境问题卡超过半天就求助，10 天版没有缓冲日
- **显存不够时**（主要是 Day 7）：退到 SmolVLA，或 AutoDL 租 A100（约 ¥5–8/时）
