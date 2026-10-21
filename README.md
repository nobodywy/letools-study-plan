# LeTools 十天入门计划（具身智能 / VLA）

> 制定日期：2026-10-19 ｜ 周期：10 天 ｜ 每日节奏：实操 70% + 理论 30%
>
> 目标：跑通 LeTools-Learning 工具链的「数据 → 训练 → 离线评测」完整闭环，同时建立对 VLA 具身操作基础模型的理论视野。
>
> 说明：本计划由 14 天版压缩而来——砍掉了独立的复盘日，VLA 实操（原 Day 10）提前到 Day 7，理论精读合并到 Day 8 集中进行。
>
> 🔄 更新 v1.1（2026-10-21）：按实际环境（WSL2 + 本地 4080S）调整，**去掉仿真（Gazebo）环节**，评测改为 checkpoint 离线验证。
>
> 📚 配套参考：[参考资料.md](参考资料.md)（具身智能术语 Wiki + Qwen-RobotManip 理论拆解 + 全部链接汇总）

---

## 一、三份资料的分工

| 资料 | 角色 | 怎么用 |
|---|---|---|
| [LeTools-Learning（GitHub）](https://github.com/LejuRobotics/LeTools-Learning) | **实操主线** | Kuavo 人形机器人的「Rosbag 数据转换 → 策略训练 → 仿真/真机部署」工具链，所有代码在这里跑 |
| [LeTools 官方文档](https://www.letools.lejurobot.com/docs.html) | **主线说明书** | 核心是《零基础完整学习教程》八部分（`#get_started/beginner_tutorial.md`）；页面右下角有 AI 助手可随时提问 |
| [知乎：Qwen-RobotManip 超详细拆解](https://zhuanlan.zhihu.com/p/2053583019710190587) | **理论线** | 工业级 VLA 基础模型怎么做数据、怎么设计动作表示、怎么训练和评测，按章节拆开晚上读 |

### 官方文档章节速查

| 分类 | 文档锚点 |
|---|---|
| GET STARTED | `#get_started/letools.md` · `#get_started/beginner_tutorial.md`（零基础主线教程）· `#get_started/installation.md` |
| TUTORIALS | `#tutorials/quick_start.md` · `#tutorials/data_preparation.md` · `#tutorials/model_training.md` · `#tutorials/lerobot_training.md` · `#tutorials/inference.md` · `#tutorials/roban_wholebody_vla.md` · `#tutorials/bring_policies.md` |
| Troubleshooting | `#troubleshooting/faq.md` · `#troubleshooting/community.md` |

---

## 二、环境与自查（已确定：WSL2 + 本地 4080S，不做仿真）

- [x] **环境**：Windows PC + WSL2 Ubuntu 26.04 + RTX 4080 SUPER（16G 显存），Mac 通过 `ssh myubuntu` 外网接入
- [ ] **GPU 验证**：WSL 里 `nvidia-smi` 正常即可（GPU 由 Windows 驱动直通，**不要**在 WSL 内装 Linux 显卡驱动）
- [ ] **不做仿真**：不装 Gazebo / ROS Noetic 桌面环境（26.04 也装不了 Noetic）；数据转换若依赖 ROS，走 Docker 容器化方式
- [ ] **评测方式**：没有仿真和真机，用 **checkpoint 离线验证**代替——拿数据集做推理，对比预测动作和真值
- [ ] **基础**：会 Python、知道神经网络是什么即可；ROS 和模仿学习零基础没关系

> 10 天版节奏偏紧，环境（Day 2）一定不要拖过第二天；卡住超过半天就直接上 [GitHub Issues](https://github.com/LejuRobotics/LeTools-Learning/issues) 或官网 AI 助手提问。

---

## 三、学习日程

### Day 1 — 概念 + 全局认知

- [ ] 读 `beginner_tutorial.md` 第一部分：模仿学习 vs VLA、ACT / Diffusion / PI0 选型
- [ ] 读 `get_started/letools.md`，对照 README 项目结构图，搞懂 `configs` / `kuavo_data` / `kuavo_model` / `kuavo_deploy` / `kuavo_server` 各自职责
- [ ] 快速浏览 `quick_start.md`，对「转换 → 训练 → 部署」三步流程建立整体印象

### Day 2 — 环境搭建

- [ ] 按 `beginner_tutorial.md` 第二~四部分 + `installation.md`：装 Miniforge、建 Python 3.12 环境
- [ ] 克隆仓库并初始化子模块，跑 `setup_env.sh`
- [ ] 不装 ROS Noetic；如数据转换脚本依赖 ROS，用 Docker 容器化方式跑（见 `installation.md` 的 Docker 部分）
- [ ] 验证 GPU：`nvidia-smi` 能看到 RTX 4080 SUPER，`python -c "import torch; print(torch.cuda.is_available())"` 输出 `True`
- [ ] 验证：`python kuavo_model/train.py --policy act --dry-run` 能正常打印配置

> ⚠️ 这步最容易卡，且 10 天版没有缓冲日，今天必须打通。报错先查 FAQ / Issues / 官网 AI 助手。

### Day 3 — 数据准备 ✅ 里程碑 M1

- [ ] 按 `beginner_tutorial.md` 第五部分 + `data_preparation.md`：下载赛事数据（或用已有 Rosbag）
- [ ] 配置 `configs/data/KuavoRosbag2Lerobot.yaml`（`platform_type` / `eef_type` / `which_arm` 必须和数据匹配）
- [ ] 跑 `python kuavo_data/CvtRosbag2Lerobot.py`，检查转换出的 LeRobot v3 数据集
- [ ] 🌙 知乎 §1.1.1–1.1.2：机器人数据 11000 小时 + 人类第一视角数据的构成

### Day 4 — 训练第一个策略（ACT）

- [ ] 按 `beginner_tutorial.md` 第六部分 + `lerobot_training.md`：读懂 `act.yaml` 每个字段
- [ ] 先 `--dry-run` 看解析结果，再小 steps（如 5000）跑通训练，盯 loss 曲线，找到 checkpoint 输出目录
- [ ] 🌙 ACT 核心思想 30 分钟科普（CVAE + action chunking）

### Day 5 — 推理链路 + 离线评测 ✅ 里程碑 M2

- [ ] 读 `inference.md` + `kuavo_deploy/eval.py` 源码，理清推理链路：checkpoint 加载 → 观测预处理 → 策略推理 → 动作输出（只读代码，不跑仿真）
- [ ] 离线验证 Day 4 的 ACT checkpoint：加载模型，对数据集里几条 episode 逐帧推理，把**预测动作序列和真值动作**画在同一张图上看贴合程度
- [ ] 算一个量化指标（如各关节 MSE / 误差曲线），形成「模型学到没有」的判断，写进笔记
- [ ] 🌙 知乎 §2.1：VLM backbone + DiT action expert 解耦架构——理解 ACT 和 VLA 差在哪

### Day 6 — 对比第二个策略

- [ ] 再训一个 `diffusion`（无需下载预训练模型），对比配置与效果差异
- [ ] 了解 `--launcher accelerate` 多卡训练用法
- [ ] 🌙 知乎 §2.2–2.3：80 维统一动作表示、camera-frame delta EEF

### Day 7 — VLA 模型实操 ✅ 里程碑 M3

- [ ] 按 `model_training.md` 下载 HuggingFace 预训练模型（PaliGemma + pi0_base / pi05_base，零基础教程 3.4 节有步骤）
- [ ] 微调 PI0 或 PI05，注意 quantile 统计文件的生成
- [ ] 了解外部模型走 `kuavo_server` 服务化部署（`policy_type: client`）
- [ ] 显存不足的替代方案：改训 `smolvla`，或只跑通外部模型服务化流程

### Day 8 — 理论深化日（知乎精读）

- [ ] 白天精读知乎 §1 数据篇（全文最重要的一章）：Human-to-Robot 数据合成（virtual finger、SAM3 分割、ProPainter 修复、IK 求 base pose）、清洗五阶段、跨模态质量检查、ECoT
- [ ] 晚上读 §2 剩余（in-context policy adaptation）+ §3（post-training、VLA-to-VA 退化）
- [ ] 补 30 分钟 flow matching 直觉理解
- [ ] 对照 Day 3 的数据转换实操，体会同一问题的「工具链版」和「工业版」差距

### Day 9 — 综合项目 ✅ 里程碑 M4

- [ ] 独立完整走一遍：数据 → 转换 → 训练 → 离线评测 → 结果分析（误差指标、预测偏差大的样本分析）
- [ ] 🌙 知乎 §4：OOD vs in-domain 评测（LIBERO-PLUS、RoboTwin-IF/XE）、五组消融；理解核心论点「**alignment first, then scale**」
- [ ] 有余力：把 ACT / Diffusion / SmolVLA 在同一份数据上的离线指标做成对比表，写一段小结

### Day 10 — 收尾与进阶

- [ ] 进阶选学：读 `bring_policies.md`，尝试接入一个新策略（深入代码结构）
- [ ] 写一篇学习笔记或博客
- [ ] 自测两问：「模仿学习和 VLA 的区别是什么」「为什么动作空间对齐比堆数据更重要」
- [ ] 规划后续路线：精读原论文（ACT → Diffusion Policy → PI0）、LeRobot 源码；以后有仿真/真机条件时，再回来补 `kuavo_deploy` 的 sim / real 部署流程

---

## 四、里程碑总览

| 里程碑 | 时间 | 验收标准 |
|---|---|---|
| M1 | Day 3 | 能独立完成 Rosbag → LeRobot 数据转换 |
| M2 | Day 5 | ACT checkpoint 完成离线验证：预测动作与真值基本贴合 |
| M3 | Day 7 | 完成一个 VLA 模型（PI0/PI05）的微调或服务化部署 |
| M4 | Day 9 | 独立完成「数据 → 训练 → 离线评测」的完整闭环 |
| 输出 | Day 10 | 一篇学习笔记 + 两个自测问题答得上来 |

---

## 五、提醒与求救渠道

1. **最容易卡的点**：Python 3.12 依赖安装，以及 WSL2 的小毛病（虚拟盘 I/O error 时用 `wsl.exe --shutdown` 重启即可）；严格按 `installation.md` 来，不要自由发挥
2. **求救渠道**：
   - [GitHub Issues](https://github.com/LejuRobotics/LeTools-Learning/issues)（附环境信息 + 完整 traceback）
   - 官网文档页右下角 LeTools AI 助手
   - QQ / 微信群（见 `community.md`）
3. **节奏原则**：环境问题卡超过半天就求助，不要自己硬耗——10 天版没有缓冲日
4. **显存不够时**（主要是 Day 7 的 PI0/PI05 微调）：退到 SmolVLA，或临时在 AutoDL 租几小时 A100（约 ¥5–8/时）；其余环节 4080S 足够

---

## 附：关键参考链接

- 仓库：https://github.com/LejuRobotics/LeTools-Learning
- 官方文档：https://www.letools.lejurobot.com/docs.html
- 零基础教程：https://www.letools.lejurobot.com/docs.html#get_started/beginner_tutorial.md
- 知乎拆解：https://zhuanlan.zhihu.com/p/2053583019710190587
- LeRobot 框架：https://github.com/huggingface/lerobot
- OpenPI：https://github.com/Physical-Intelligence/openpi
- NVIDIA Isaac GR00T：https://github.com/NVIDIA/Isaac-GR00T
- Kuavo-VLA-1.0 模型页：https://model.lejurobot.com/kuavo-vla-1
