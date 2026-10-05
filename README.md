# 蓝底标志识别数据集

面向 **2025 年智能车竞赛 · 室外赛 5G 赛道** 的蓝底标志图像分类数据集，覆盖 **左右转标志** 与 **AB 标志** 四类目标，可用于车载视觉中的标志识别与分类训练。

仓库名：`Blue-background-logo-recognition-dataset`

---

## 项目简介

本仓库提供一组已按类别整理好的 JPG 图像，画面主体为蓝底白图案标志（左转、右转、字母 A、字母 B）。数据以压缩包形式发布，解压后即可按文件夹标签直接用于图像分类任务。

| 项目 | 说明 |
| --- | --- |
| 任务类型 | 图像分类（按目录作为类别标签） |
| 类别数量 | 4 |
| 图像格式 | JPG |
| 图像尺寸 | 224 × 224 |
| 图像总数 | 1605 |
| 数据打包 | [`dataset.zip`](./dataset.zip)（约 15 MB） |

---

## 适用场景

- 2025 年智能车竞赛室外赛 5G 赛道相关视觉任务
- 蓝底左右转标志、AB 标志的识别与分类模型训练 / 验证
- 轻量分类网络、嵌入式 / 车载部署前的快速原型验证

> 说明：仓库内未提供检测框或分割标注，当前组织方式更适合 **分类** 而非目标检测。

---

## 数据集内容

| 类别 | 目录 | 含义 | 图像数量 |
| --- | --- | ---: | ---: |
| A | `A/` | AB 标志中的 **A** | 352 |
| B | `B/` | AB 标志中的 **B** | 479 |
| left | `left/` | **左转** 标志 | 404 |
| right | `right/` | **右转** 标志 | 370 |
| **合计** | | | **1605** |

### 样例预览

| A | B | left | right |
| :---: | :---: | :---: | :---: |
| ![A](assets/samples/A.jpg) | ![B](assets/samples/B.jpg) | ![left](assets/samples/left.jpg) | ![right](assets/samples/right.jpg) |

样例特征（据解压后图像观察）：标志多为 **深蓝底 + 白色图案**，图像为裁剪后的近景小图；部分样本存在模糊、透视倾斜等采集差异。

---

## 目录结构

仓库根目录：

```text
.
├── README.md
├── dataset.zip          # 完整数据集
└── assets/
    └── samples/         # README 预览用样例图（从 dataset.zip 抽取）
        ├── A.jpg
        ├── B.jpg
        ├── left.jpg
        └── right.jpg
```

解压 `dataset.zip` 后（忽略 macOS 产生的 `__MACOSX/`）：

```text
dataset/
├── A/          # A_1.jpg, A_2.jpg, ...
├── B/          # B_1.jpg, B_2.jpg, ...
├── left/       # left_1.jpg, left_2.jpg, ...
└── right/      # right_1.jpg, right_2.jpg, ...
```

文件命名规则：`{类别名}_{编号}.jpg`（编号大致连续，个别编号可能缺失）。

---

## 标注格式

本数据集采用 **文件夹即标签** 的组织方式，**没有** 单独的标注文件（如 YOLO txt、COCO json、VOC xml 等）。

- 类别标签 = 图像所在目录名：`A` / `B` / `left` / `right`
- 一张图像对应一个类别
- 可直接对接常见分类数据加载方式（如按子目录读取）

若需用于检测任务，需自行补充边界框等标注。

---

## 使用方法

### 1. 获取数据

```bash
git clone https://github.com/Sleepless-cloud/Blue-background-logo-recognition-dataset.git
cd Blue-background-logo-recognition-dataset
unzip dataset.zip -d dataset
```

解压后建议忽略或删除 `__MACOSX/` 目录（为打包时的系统元数据，不是训练数据）。

### 2. 快速检查

```bash
# 各类别数量
find dataset/A dataset/B dataset/left dataset/right -type f -name '*.jpg' | wc -l

# 按类别统计
for d in A B left right; do
  echo -n "$d: "
  find "dataset/$d" -type f -name '*.jpg' | wc -l
done
```

### 3. 接入训练（示例）

以 PyTorch `ImageFolder` 为例（需自行安装依赖并划分训练/验证集）：

```python
from torchvision import datasets, transforms
from torch.utils.data import DataLoader

transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
])

dataset = datasets.ImageFolder(root="dataset", transform=transform)
loader = DataLoader(dataset, batch_size=32, shuffle=True)

print(dataset.classes)  # 预期类似 ['A', 'B', 'left', 'right']
```

类别顺序以实际目录名为准。

---

## 统计信息

以下统计来自对仓库内 `dataset.zip` 的直接清点（已排除 `__MACOSX/`）：

| 统计项 | 数值 |
| --- | --- |
| 类别数 | 4（`A`, `B`, `left`, `right`） |
| 图像总数 | 1605 |
| 单类数量 | A: 352 · B: 479 · left: 404 · right: 370 |
| 分辨率 | 全部为 **224 × 224** |
| 格式 | JPG |
| 压缩包体积 | 约 15 MB |

类别分布不完全均衡（`B` 最多，`right` 相对较少），划分训练/验证集时可按需做分层抽样或类别平衡。

---

## 竞赛背景

本数据集面向 **2025 年智能车竞赛室外赛 5G 赛道** 中常见的蓝底指示类标志识别需求，重点覆盖：

1. **左右转标志**：引导车辆转向决策  
2. **AB 标志**：赛道中的字母标识识别  

仓库本身未附带官方竞赛规则原文或成绩说明；具体赛题细节请以当年竞赛组委会公布材料为准。

---

## 许可证

当前仓库 **未附带** `LICENSE` 文件，GitHub 仓库元数据中亦未声明开源许可证。

在补充明确许可证之前，使用、再分发或商用前请先联系仓库维护者确认授权范围。

---

## 维护信息

- GitHub：https://github.com/Sleepless-cloud/Blue-background-logo-recognition-dataset
- 数据文件：[`dataset.zip`](./dataset.zip)
- 预览样例：[`assets/samples/`](./assets/samples/)

如有问题或补充说明，欢迎通过 GitHub Issues 反馈。
