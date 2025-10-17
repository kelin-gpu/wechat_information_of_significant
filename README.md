# WeChat Information of Significant

> 一个基于微信聊天数据的“重要信息分析”项目。  
> 本项目最初来源于一次失败但有意义的尝试，通过多个数据集处理流程与机器学习模型，探索如何用 AI 和算法识别微信聊天中的重要信息。

---

## 🧠 项目简介

这个项目的核心目标是：
- **收集与处理微信聊天数据集**
- **通过 API 和机器学习模型识别消息的重要程度**
- **实现一个从数据采集 → 清洗 → 建模 → 应用的完整流程**

项目整体结构包括：
1. `dataset_initial`：最初的数据集尝试（非主要流程）  
2. `dataset_process`：数据处理与清洗  
3. `api_work`：API 调用与数据标注  
4. `sklearn_part`：机器学习分析与建模  

---

## 📁 目录结构


---

## 📂 dataset_initial

> ⚠️ 此部分可忽略，属于早期尝试。

最初的数据集来自另一个 GitHub 项目。由于原项目在正常途径下无法使用，只能通过“非常规手段”获取数据集。  
后来该数据集被替换，但这一部分依然保留作参考。  

**感谢：**  
特别感谢 **洪翌铭** 的文档与经验分享，他的内容对学习如何收集数据集、利用他人项目的思路非常有帮助：  
👉 [洪翌铭文档链接](https://nova.yuque.com/ph25ri/ua1c3q/dehu9no5m8fmfo89)

---

## 📂 dataset_process

这个模块主要负责数据集的处理与格式转换。

### 数据流转结构

#### 第一条流程（未最终使用）：
这个流程虽然最终未采用，但获取的数据更加丰富，也更难清洗。

#### 第二条流程（最终使用）：

在你重复这个流程时，只保留 `AMA_merged.json` 即可，后续脚本会重新生成其他文件。

---

### 📜 主要代码文件

#### `code_json_tranform_csv.ipynb`
- 从 `AMA_merged.json` 读取数据；
- 使用 `pandas` 筛选出主要字段（`wxid`, `Timestamp`, `Text`）；
- 输出为 `CSV` 文件，准备后续使用。

#### `code_group10.ipynb`
- 将多条聊天消息打包为一组（每 10 条为一组）；
- 为每组计算时间范围、参与者集合；
- 将文本用 `\n` 拼接；
- 输出新的 `CSV` 文件，用于后续模型输入。

---

## 📂 api_work

此模块用于调用 **阿里云 API** 来评估聊天内容的重要性。

### 数据输入
直接使用上一步生成的 `ama_group_message.csv`。

### 主要流程
1. 读取 CSV；
2. 调用阿里云 API；
3. 使用自定义 prompt（结合 DeepSeek、微信公众号 Nova 推广文案与个人理解）；
4. 输出分析结果至 `ana_all.csv`。

> ⚠️ 注意：
> - API Key 不提供，请自行购买。
> - 脚本会将字符串结果转为浮点数后存入 DataFrame。

---

## 📂 sklearn_part

该模块是模型训练与应用部分。

### `Normalization.ipynb`
- 由于 API 输出的结果 (`acc`) 并非严格处于 `[0, 1]` 区间；
- 需重新归一化，使结果映射到 0–1 范围。

### `linear_regression.ipynb`
- 使用归一化后的 `ama_all_norm.csv`；
- 处理非数值列（使用 `OneHotEncoder`）；
- 使用带正则化的线性回归模型；
- 输出模型评估结果与图表；
- 保存训练好的模型与编码器。

### `use_model.ipynb`
- 演示如何使用训练好的模型；
- 只需输入三个特征：`wxid`, `time`, `text`；
- 模型与编码器文件需放在同一目录；
- 若需增加输入特征，请联系作者。

---

## 🧩 环境依赖

- Python 3.8+
- pandas  
- numpy  
- scikit-learn  
- requests  
- jupyter  

安装依赖：
```bash
pip install -r requirements.txt
****
