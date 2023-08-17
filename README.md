# 环境噪声分类模型部署教程(基于ACDNet)

## A. 模型训练
#### A.1 包下载
1. 创建 `python 3.7+` 的支持环境.
2. Install `torch`.
3. Install `wavio`.
4. Install `FFmpeg` for downsampling and upsampling audio recordings.

#### A.2 数据预处理
1. 进入 pro_dataset 的根目录
2. 数据集文件构成如下：
* class_file
  * class0
    * *.wav
  * class1
  * class2
  * ...
3. 运行```python pre_dataset.py```来将数据集语音文件重新命名并放到 datasets/dataset 中, 命名规则为 {fold}-{index}-{ab}-{i}.wav.
* fold: 该数据集被分到的验证集组号*
* index 和 ab: 该数据集的唯一编号
* i: 标签label
4. 进入 pro_dataset/ACDNet 的根目录
5. 运行 ```python common/prepare_dataset.py``` 进行数据处理.
6. 运行 ```python common/val_generator.py``` 准备验证集.
* 现在所有的数据被处理成 `44.1kHz` 和 `20kHz`, 并且保存在`datasets/`目录中*.

#### A.3 训练模型 (PyTorch)
训练一个新模型, 运行: ```python torch/trainer.py```.
##### Notes
* 根据屏幕显示的步骤进行操作.
* 训练一个新模型, 请选择 `training from scratch` 并且在下一步 model path 选项中不输入任何路径.
* 训练后的模型权重会保存在 `torch/trained_models`.
* 模型权重以你输入的名字命名.
* 对于 5 重交叉验证，将有 5 个相应的模型.

## B. 测试
#### B.1 数据预处理
1. 进入 pro_dataset/ACDNet 的根目录.
2. 测试的数据集文件构成如下：
* test_dataset
  * *.wav
3. 运行  ```python common/prepare_test_dataset.py``` 进行测试数据集预处理.
4. 运行  ```python common/test_generator.py``` 进行测试数据集预处理, 在运行前要设置 `opt.batchSize=测试数据集的数量 * 10`.

#### B.2 测试
运行: ```python torch/test_tester.py```.
##### Notes
* 根据屏幕显示的步骤进行操作.
* 输入训练模型权重路径：训练后的模型权重会保存在 `torch/trained_models`.

