# FreqFLIP-MMOERec  
**结合频域增强与语言模型初始化的多任务序列推荐系统**

## 📌 项目简介  
本项目基于 MovieLens-1M 数据集，构建了一个增强型序列推荐系统，结合以下三种技术：

- **FLIP**：利用预训练语言模型（如 BERT）对 item 的标题进行编码，初始化 MovieID 的语义表示，提升冷启动与稀疏场景下的表现。
- **频域对比学习**：将用户行为序列映射至频率域，建模兴趣演化趋势。
- **MMOE（Multi-gate Mixture of Experts）**：多任务学习结构，提升 CTR 与其他相关任务的联合优化能力。

---

## 🔍 背景动机  
传统 ID 向量的推荐模型存在以下问题：

- 冷启动与稀疏数据下效果差  
- 难以捕捉用户兴趣的动态变化  
- 多任务学习中特征共享不充分或干扰严重  

我们通过：
- 使用 **FLIP** 实现更具语义的 embedding 初始化  
- 融合 **频域对比学习** 处理兴趣演化  
- 构建 **MMOE 多任务结构** 实现信息共享与任务隔离  
来系统性提升推荐性能。

---

## 🛠️ 模型结构图  

```text
                   ┌────────────┐
                   │  Movie Title (文本) │
                   └────────────┘
                          │
                 [ FLIP 编码生成 Embedding ]
                          │
        ┌──────────────────────────────────┐
        │     频率域 Transformer 编码器     │ ← 用户行为序列
        └──────────────────────────────────┘
                          │
               用户兴趣表示（频率域）
                          │
                     [ MMOE 层 ]
                          │
           ┌──────────────┬───────────────┐
           │              │               │
         CTR         任务A（如影片类型）   任务B（如流行度预测）
     预测任务
```

---

## 📊 数据集信息

- 使用 [MovieLens-1M](https://grouplens.org/datasets/movielens/1m/) 数据集  
- 电影标题用于生成 FLIP 初始化 embedding，评分数据用于构建用户行为序列。

---

## 📦 项目特性

- ✅ 基于语言模型的 **Movie Embedding 语义初始化**  
- ✅ 频率域对比学习建模 **用户兴趣变化**  
- ✅ **MMOE 多任务结构** 提升多目标学习性能  
- ✅ **模块化结构设计**，可灵活替换 embedding 生成方式  
- ✅ 强化 **冷启动鲁棒性** 与 **数据稀疏性处理能力**

---

## 🚀 快速开始

### 1. 克隆项目  
```bash
git clone https://github.com/your_username/FreqFLIP-MMOERec.git
cd FreqFLIP-MMOERec
```

### 2. 安装依赖  
```bash
pip install -r requirements.txt
```

### 3. 准备数据  
将 `ratings.dat` 和 `movies.dat` 放入 `./data/` 文件夹下。

### 4. 运行训练脚本  
```bash
python train.py --model freqflip_mmoe --dataset movielens_1m
```

---

## 📈 评估指标

- **Hit@K**：命中率  
- **NDCG@K**：归一化折损累计增益  
- **AUC**：用于 CTR 子任务的分类效果评估

---

## 📚 参考文献

- [FLIP: Fine-grained Alignment between ID-based Models and Pretrained Language Models for CTR Prediction](https://arxiv.org/abs/2306.01997)  
- [Contrastive Learning with Frequency-Domain Interest Trends for Sequential Recommendation (RecSys 2023)](https://arxiv.org/abs/2306.00919)  
- [MMOE: Multi-gate Mixture-of-Experts for Multi-Task Learning](https://arxiv.org/abs/1808.07519)

---

## 📌 未来工作

- [ ] 引入用户侧语言建模，优化冷启动用户建模能力  
- [ ] 对比 BERT4Rec、FPMC 等模型的表现  
- [ ] 加入模块化 ablation 实验：仅 FLIP / 仅频域 / 全部融合  
- [ ] 发布预训练 embedding 模型与配置

---

## 🤝 致谢  
本项目参考了 FLIP、FrequencyRec 等开源实现，感谢原作者的分享精神。