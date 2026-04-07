# 文档模板 - 项目贡献指南

> 标准格式规范，确保仓库文档的一致性与专业性

---

## 一、文档结构

```markdown
# [项目名称]

> [一句话核心定位]

## 快速上手
## 核心创新
## 技术规格
## Benchmark
## 开发者指南
## 技术限制
## 相关链接
```

---

## 二、快速上手段落模板

```markdown
### 环境配置

```bash
# 安装依赖
pip install [package]

# 克隆仓库
git clone https://github.com/[org]/[repo].git
cd [repo]

# 验证安装
python -c "import [module]; print([module].__version__)"
```

### 基础用法

```python
import [module]

model = [Class].from_pretrained("[model_name]")
result = model.predict(input_data)
```
```

---

## 三、技术规格表格

```markdown
| 维度 | 规格 |
|------|------|
| **核心范式** | [JEPA/RSSM/Diffusion/VLA] |
| **模型规模** | [参数数量] |
| **预训练数据** | [数据规模] |
| **状态表征** | [Pixel/Latent/PointCloud] |
| **动作模式** | [Chunking/Diffusion/Token] |
| **动作频率** | [Hz] |
| **开源** | [是/否/部分] |
```

---

## 四、Benchmark 表格

```markdown
| 基准 | 指标 | 结果 | 对比 |
|------|------|------|------|
| [数据集] | [指标] | [数值] | vs [基线] |
```

---

## 五、检查清单

提交前请确认：

- [ ] 包含项目链接（GitHub/arXiv）
- [ ] 包含快速上手机代码
- [ ] 包含技术规格表格
- [ ] 包含至少一个 Benchmark 数据
- [ ] 格式符合本模板
- [ ] 无主观评价词汇
