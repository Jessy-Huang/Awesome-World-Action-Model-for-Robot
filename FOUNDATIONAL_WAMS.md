# Foundational WAMs - 基础世界动作模型

> 具有强泛化能力的基础大模型，支持零样本任务迁移，覆盖从感知到动作的完整链路。

---

## 目录

- [OpenVLA - 开源视觉语言动作模型](#openvla---开源视觉语言动作模型)
- [CogACT - 认知协同VLA](#cogact---认知协同vla)
- [RT-2 - VLA先驱](#rt-2---vla先驱)
- [Pi0 - Physical Intelligence基础模型](#pi0---physical-intelligence基础模型)
- [GR00T - NVIDIA人形机器人基础模型](#gr00t---nvidia人形机器人基础模型)
- [Octo - 多任务机器人策略](#octo---多任务机器人策略)

---

## OpenVLA - 开源视觉语言动作模型

> 首个开源的大规模视觉语言动作模型，在 27B 参数规模上实现强大的零样本泛化能力。

![VLA](https://img.shields.io/badge/Paradigm-VLA-purple)
![Stars](https://img.shields.io/badge/Stars-8.2K-orange)

### 基本信息

| 属性 | 值 |
|------|-----|
| **Paper** | [arXiv:2409.16215](https://arxiv.org/abs/2409.16215) |
| **Code** | [openvla/openvla](https://github.com/openvla/openvla) |
| **机构** | HuggingFace + 洛桑理工 + NYU |
| **发布年份** | 2024 |

### 快速上手

```bash
# 安装
git clone https://github.com/openvla/openvla.git
cd openvla
pip install -e .

# 使用预训练模型
python -c "
from openvla import OpenVLA
model = OpenVLA.from_pretrained('openvla/openvla-7b')
action = model.predict(image, 'pick up the red block')
"
```

### 核心创新

1. **开源 VLA 先行者**: 首次将 27B 参数 VLA 模型开源，支持研究和微调
2. **多机器人适应**: 通过统一动作空间设计，支持 UR5、Franka、Panda 等多种机械臂
3. **开放词汇**: 基于 LLaMA 自然语言理解，支持复杂指令

### 技术规格

| 维度 | 规格 |
|------|------|
| **核心范式** | Transformer-VLA |
| **模型规模** | 7B / 27B (ViT + LLM) |
| **预训练数据** | Open X-Embodiment (700K 轨迹) |
| **状态表征** | Pixel (ViT-L) + Proprioception |
| **动作模式** | Multi-modal Tokenization |
| **动作频率** | 5-10 Hz |
| **动作空间** | 连续 (7-DOF 机械臂) |

### 架构图

```
┌─────────────────────────────────────────────────────────────┐
│                    OpenVLA Architecture                     │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   [Robot Image] ──→ ViT-L/14 ──┐                            │
│                                 ├──→ Vicuna-7B ──→ Action    │
│   [Language Instruction] ──→ Embed ──┘                       │
│                                                              │
│   [Proprioception] ──→ Linear ────────────────────────→     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Benchmark

| 基准 | 任务 | OpenVLA | 对比基线 |
|------|------|---------|----------|
| Open X-Embodiment | 真实机器人泛化 | SOTA | +15% vs RT-2 |
| LIBERO | 模拟任务 | 85% | 持平专用模型 |
| 零样本 | 训练外任务 | 45% | 显著领先 |

### 开发者指南

| 维度 | 评估 |
|------|------|
| **环境配置** | 中等 (需要 Docker) |
| **硬件需求** | 40GB GPU (A100) |
| **微调难度** | 低 (LoRA 支持) |
| **文档质量** | 完整 |

### 技术限制

- 需要大规模 GPU 进行推理
- 动作空间主要针对机械臂
- 长程任务规划能力有限

---

## CogACT - 认知协同VLA

> 微软研究院提出的 VLA 架构，通过认知模块与动作模块的协同增强机器人操作能力。

![VLA](https://img.shields.io/badge/Paradigm-VLA-purple)
![Stars](https://img.shields.io/badge/Stars-414-green)

### 基本信息

| 属性 | 值 |
|------|-----|
| **Paper** | [arXiv](https://arxiv.org/) |
| **Code** | [microsoft/CogACT](https://github.com/microsoft/CogACT) |
| **机构** | Microsoft Research |
| **发布年份** | 2024 |

### 核心创新

1. **认知-动作协同**: 引入专门的认知推理模块，指导动作生成
2. **分层架构**: 高层规划 + 低层控制分离
3. **多任务泛化**: 在复杂操作任务上展现优秀泛化能力

### 技术规格

| 维度 | 规格 |
|------|------|
| **核心范式** | Cognitive VLA |
| **模型规模** | ~13B |
| **状态表征** | Pixel + Semantic |
| **动作模式** | Hierarchical Action |

---

## RT-2 - VLA先驱

> Google DeepMind 提出的 Vision-Language-Action 模型，首次实现零样本机器人泛化。

### 基本信息

| 属性 | 值 |
|------|-----|
| **Paper** | [arXiv:2308.07930](https://arxiv.org/abs/2308.07930) |
| **机构** | Google DeepMind |
| **发布年份** | 2023 |
| **开源** | 否 |

### 核心创新

1. **VLA 概念提出**: 首次将视觉、语言、动作统一建模
2. **动作 Token 化**: 将连续动作离散化为 Token，类比 LLM 输出
3. **零样本泛化**: 在训练未见过的任务上展现泛化能力

### 技术规格

| 维度 | 规格 |
|------|------|
| **核心范式** | Transformer-VLA |
| **模型规模** | 55B (ViT + PaLM-E) |
| **预训练数据** | RT-1 数据 (130K episodes) |
| **动作频率** | 1-5 Hz |
| **动作空间** | 连续 (末端位置 + gripper) |

### Benchmark

| 测试集 | RT-2 | RT-1 | 基线 |
|--------|------|------|------|
| 分布内 | 62% | 67% | 33% |
| 分布外-视觉 | 39% | 0% | 0% |
| 分布外-语言 | 47% | 0% | 0% |

### 技术限制

- 计算资源需求极高 (64+ TPU)
- 动作频率受限于 LLM 推理速度
- 未开源

---

## Pi0 - Physical Intelligence基础模型

> Physical Intelligence 发布的通用物理技能基础模型，聚焦真实世界机器人操作。

### 基本信息

| 属性 | 值 |
|------|-----|
| **Paper** | 技术报告 |
| **机构** | Physical Intelligence |
| **发布年份** | 2024 |
| **开源** | 部分 (模型权重) |

### 核心创新

1. **通用物理技能**: 覆盖抓取、放置、折叠、清洁等日常操作
2. **快速适配**: 支持新任务的快速微调
3. **真实机器人**: 专注于真实机器人部署

### 技术规格

| 维度 | 规格 |
|------|------|
| **核心范式** | VLA + Diffusion |
| **模型规模** | 7B |
| **预训练数据** | 私有大规模数据 |
| **动作模式** | Diffusion-based |

---

## GR00T - NVIDIA人形机器人基础模型

> NVIDIA 推出的人形机器人基础模型，支持多种硬件配置。

### 基本信息

| 属性 | 值 |
|------|-----|
| **Paper** | 技术文档 |
| **机构** | NVIDIA |
| **发布年份** | 2024 |

### 核心创新

1. **人形机器人专注**: 针对双足/双臂人形设计
2. **Isaac 仿真集成**: 与 NVIDIA 仿真平台深度集成
3. **多模态输入**: 支持视觉、触觉、力矩等

### 支持硬件

- 宇树 H1
- Figure 01
- 1X Neo
- Agility Digit

---

## Octo - 多任务机器人策略

> Stanford 提出的开源多任务机器人策略模型。

### 基本信息

| 属性 | 值 |
|------|-----|
| **Code** | [StanfordVL/octo](https://github.com/stanfordvl/octo) |
| **机构** | Stanford Vision Lab |
| **Stars** | 2.5K+ |

### 核心创新

1. **开源**: 首个开源大规模机器人策略模型
2. **多任务**: 在 Open X-Embodiment 数据上预训练
3. **可扩展**: 支持新任务和机器人的适配

### 技术规格

| 维度 | 规格 |
|------|------|
| **核心范式** | Transformer Policy |
| **模型规模** | 93M / 270M |
| **动作模式** | Action Chunking |
| **支持机器人** | UR5, Panda, WidowX, Aloha |

---

## 分类对比

| 项目 | 机构 | 参数 | 开源 | 泛化能力 |
|------|------|------|------|----------|
| OpenVLA | HuggingFace | 7B | 是 | 强 |
| CogACT | Microsoft | 13B | 是 | 强 |
| RT-2 | Google | 55B | 否 | 强 |
| Pi0 | PI | 7B | 部分 | 强 |
| GR00T | NVIDIA | - | 是 | 中 |
| Octo | Stanford | 270M | 是 | 中 |

---

## 相关资源

- [Open X-Embodiment Dataset](https://robotics-transformer.github.io/)
- [LeRobot 框架](../SIMULATION_REAL2SIM.md#lerobot)
- [Action Controllers](../ACTION_CONTROLLERS.md)
