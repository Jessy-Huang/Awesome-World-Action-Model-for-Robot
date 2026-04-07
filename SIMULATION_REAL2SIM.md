# Simulation & Sim2Real - 仿真与域迁移

> 侧重于利用世界模型进行大规模合成数据生成，或实现高保真Sim2Real迁移。

---

## 目录

- [LeRobot](#lerobot)
- [SimplerEnv](#simplerenv)
- [Squint - Sim2Real](#squint---sim2real)
- [RoboGen](#robogen)

---

## 为什么需要仿真？

| 方面 | 真实机器人 | 仿真环境 |
|------|------------|----------|
| **数据采集** | 慢 (物理限制) | 快 (可并行) |
| **成本** | 高 (硬件磨损) | 低 (云计算) |
| **安全性** | 有风险 | 无风险 |
| **可控性** | 难复现 | 完全可控 |
| **并行** | 受限 | 100+ 实例 |

---

## LeRobot

> HuggingFace 的机器人学习框架，提供端到端的数据集、训练和仿真集成。

![Stars](https://img.shields.io/badge/Stars-23K-red)

### 基本信息

| 属性 | 值 |
|------|-----|
| **Code** | [huggingface/lerobot](https://github.com/huggingface/lerobot) |
| **机构** | HuggingFace |
| **发布年份** | 2024 |

### 核心功能

1. **数据集**: 统一的机器人数据集格式
2. **仿真环境**: 集成 PyBullet / Isaac Sim
3. **训练**: 多种策略模型支持
4. **评估**: 标准化评估流程

### 数据集支持

| 数据集 | 规模 | 类型 | 机器人 |
|--------|------|------|--------|
| so101_bc | 10K | 演示 | SO-101 |
| aloha | 15K | 遥操作 | Aloha |
| libero | 50K | 仿真 | Franka |
| calvin | 24K | 仿真 | KUKA |

### 仿真示例

```python
from lerobot import simulate

# 创建仿真环境
env = simulate(
    robot_type="so101",
    task="pick_cube",
    backend="pybullet"
)

# 收集数据
env.record(n_episodes=100)

# 训练策略
from lerobot import train
policy = train(dataset="so101_bc", policy="act")
```

---

## SimplerEnv

> 简化机器人策略仿真环境，支持 RT-1、Octo、OpenVLA 等主流模型。

### 基本信息

| 属性 | 值 |
|------|-----|
| **Code** | [trannguyenle95/SimplerEnv](https://github.com/trannguyenle95/SimplerEnv) |
| **机构** | - |
| **Stars** | 2K |

### 支持的策略

| 策略 | 支持程度 |
|------|----------|
| RT-1 | 官方支持 |
| Octo | 官方支持 |
| OpenVLA | 官方支持 |
| SpatialVLA | 社区支持 |
| COGAct | 社区支持 |

### 支持的机器人

- Google Robot
- WidowX + Bridge
- WidowX 250
- UR5

---

## Squint - 快速视觉RL Sim2Real

> 快速视觉强化学习 Sim2Real 框架。

![Stars](https://img.shields.io/badge/Stars-45-blue)

### 基本信息

| 属性 | 值 |
|------|-----|
| **Paper** | [arXiv](https://arxiv.org/) |
| **Code** | [aalmuzairee/squint](https://github.com/aalmuzairee/squint) |
| **发布年份** | 2024 |

### 核心创新

1. **快速 Sim2Real**: 减少仿真到真实迁移的 gap
2. **视觉 RL**: 端到端视觉策略
3. **SO-101 支持**: 适配主流机械臂

### 技术栈

- SO-101 Robot Arm
- ManiSkill3 仿真
- PyTorch
- CUDA 加速

### 快速上手

```bash
git clone https://github.com/aalmuzairee/squint.git
cd squint
pip install -r requirements.txt

# 训练
python train.py --env maniskill --task pick_cube

# Sim2Real
python deploy.py --real_robot
```

---

## RoboGen

> 生成式仿真平台，利用 GenAI 生成多样化训练数据。

### 基本信息

| 属性 | 值 |
|------|-----|
| **Paper** | [RoboGen](https://arxiv.org/) |
| **机构** | MIT / CMU |
| **发布年份** | 2024 |

### 核心创新

1. **程序化生成**: 自动生成多样化任务
2. **资产生成**: 自动生成 3D 场景资产
3. **物理仿真**: 集成多种物理引擎

### 工作流程

```
┌─────────────────────────────────────┐
│         RoboGen Pipeline            │
├─────────────────────────────────────┤
│                                      │
│  LLM 任务描述 ──→ 任务分解            │
│       ↓                              │
│  3D资产生成 ──→ 场景构建             │
│       ↓                              │
│  物理仿真 ──→ 数据收集               │
│       ↓                              │
│  策略训练 ──→ 仿真评估               │
│                                      │
└─────────────────────────────────────┘
```

---

## Sim2Real Gap 分析

### Gap 来源

```
Sim2Real Gap = 感知差距 + 动力学差距 + 动作差距

感知差距:
  仿真渲染 ≠ 真实相机
  完美分割 ≠ 噪声检测

动力学差距:
  标称质量 ≠ 真实质量
  理想摩擦 ≠ 现实摩擦

动作差距:
  精确执行 ≠ 死区/延迟
```

### 解决策略

| 策略 | 方法 | 适用场景 |
|------|------|----------|
| Domain Randomization | 随机化仿真参数 | 视觉差异 |
| System ID | 精确测量物理参数 | 动力学差异 |
| Domain Adaptation | 图像风格迁移 | 感知差异 |
| 在线适配 | 真实数据微调 | 全面 gap |

---

## Benchmark 工具

### 标准仿真基准

| 基准 | 任务数 | 评估指标 | 适用策略 |
|------|--------|----------|----------|
| CALVIN | 6 | 长程成功率 | 模仿学习 |
| LIBERO | 4套 | 任务完成率 | 多种 |
| ManiSkill3 | 100+ | 成功率 | 视觉 RL |
| RLBench | 100 | 成功率 | 多种 |

### 评估指标

| 指标 | 说明 | 阈值 |
|------|------|------|
| SR | 成功率 | >80% |
| SPL | 路径效率加权成功率 | >0.7 |
| ASR | 平均成功率 | >75% |

---

## 工具对比

| 工具 | 类型 | 开源 | 难度 | 适用场景 |
|------|------|------|------|----------|
| LeRobot | 框架 | 是 | 低 | 端到端学习 |
| SimplerEnv | 环境 | 是 | 中 | 策略评估 |
| Squint | 框架 | 是 | 中 | Sim2Real |
| RoboGen | 平台 | 是 | 高 | 数据生成 |

---

## 相关链接

- [LeRobot GitHub](https://github.com/huggingface/lerobot)
- [CALVIN 基准](https://github.com/Meesemo/calvin)
- [LIBERO 基准](https://github.com/LIBERO-robot/LIBERO)
- [ManiSkill3](https://github.com/Meesemo/maniskill3)
