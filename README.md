# XOR · MLP 参数实验室

一个**单文件、零依赖**的交互式教学网页：用一个 `2 → 2 → 1` 多层感知机（MLP）现场学习 XOR 逻辑门，并把"参数如何学习"这件事完整地可视化出来。

所有训练都在**浏览器本地**完成（全批量二元交叉熵 + Adam），不需要任何后端或 Python 环境。打开 `index.html` 即可运行。

## ✨ 功能

- **真实训练**：用 JavaScript 复现 BCE + Adam，训练 300 轮，两种框架（TensorFlow / PyTorch）各跑一次，得到几乎一致的参数。
- **网络权重可视化**：连线粗细 = 权重绝对值，蓝色为正、玫红为负；点选样本后展开逐层数值（z → h → logit → 概率）。
- **训练过程可回放**：拖动训练轮次滑块或点「从头播放」，实时联动 Loss / Accuracy / 最大权重与损失曲线上的圆点。
- **决策边界实时成形**：canvas 绘制整个输入空间的预测概率，随训练轮次变化；可调节分类阈值，观察分类区域与四个样本判定的变化。
- **两种写法对比**：并列展示 PyTorch 显式训练循环与 TensorFlow/Keras 标准流程。
- **参数对照**：展示两次真实训练得到的最终权重、偏置与最大参数差。
- 响应式布局，适配桌面与移动端。

## 📁 文件结构

```
.
├── index.html     # 全部内容（HTML + CSS + JS 单文件）
├── .nojekyll      # 让 GitHub Pages 直接托管，不经过 Jekyll
└── README.md
```

## 🚀 部署到 GitHub Pages

1. 在 GitHub 新建一个仓库，例如 `xor-mlp-lab`。
2. 把本目录的文件上传到该仓库（保持 `index.html` 在根目录）。
3. 进入仓库 **Settings → Pages**，Source 选择 `Deploy from a branch`，分支选 `main`、目录选 `/ (root)`，保存。
4. 等待几十秒后访问：

   ```
   https://<你的用户名>.github.io/xor-mlp-lab/
   ```

也可命令行一键推送（把 `<USER>`、`<REPO>` 换成自己的）：

```bash
git init
git add .
git commit -m "init: XOR MLP lab"
git branch -M main
git remote add origin https://github.com/<USER>/<REPO>.git
git push -u origin main
```

## 🧠 原理速览

网络：输入 `x=(x₁,x₂)`，隐藏层两个 sigmoid 神经元，输出一个 sigmoid 神经元。

```
h = σ(xW + b)
ŷ = σ(hv + c)
```

- 损失：四样本平均二元交叉熵 `L = -Σ[y·log(ŷ)+(1-y)·log(1-ŷ)] / 4`
- 优化：Adam（`lr=0.03, β₁=0.9, β₂=0.999`），全批量梯度下降
- XOR 线性不可分，因此单个隐藏层是必须的；训练结束后四个样本全部正确分类为 `[0,1,1,0]`。

> 不同框架/浮点实现的末位小数会略有差异，但参数结构、符号与分类结果一致。
