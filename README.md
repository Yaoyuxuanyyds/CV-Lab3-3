<h1 align = "center">3D Reconstruction Based on NeRF</h1>

​	

​	**在计算机视觉领域，基于 NeRF（Neural Radiance Fields）的物体重建和新视图合成技术近年来取得了显著的进展。NeRF 利用神经网络学习物体的三维表示，从而能够生成高质量的新视图。本项目使用 Nerf-Pytorch 的项目框架，完成了对生活中物品的 3D 重建以及环绕视角视频渲染。**

***Nerf 模型训练框架来源：https://github.com/yenchenlin/nerf-pytorch.git***



### 项目目录：

**主要脚本文件：**

1. **run_nerf.py**:
   - 该脚本是NeRF模型的主要运行脚本，包含训练和渲染NeRF模型的核心代码。
   - 具体函数功能：
     - `batchify`：将函数应用于较小的批次以节省内存。
     - `run_network`：准备输入并应用神经网络。
     - `batchify_rays`：将光线分批渲染以避免内存不足。
     - `render`：渲染光线。
     - `render_path`：渲染路径上的视角。
     - `create_nerf`：实例化NeRF的MLP模型。
     - `raw2outputs`：将模型的预测转换为语义上有意义的值。
     - `render_rays`：体积渲染。
2. **run_nerf_helpers.py**:
   - 该脚本包含辅助NeRF模型训练和渲染的各种函数。
   - 具体函数功能：
     - `img2mse`、`mse2psnr`、`to8b`：图像处理相关的函数。
     - `Embedder`类及其方法：用于位置编码。
     - `get_embedder`：获取位置编码器。
     - `NeRF`类及其方法：NeRF模型的实现。
     - `get_rays`、`get_rays_np`：计算光线的方向和起点。
     - `ndc_rays`：将光线转换为规范设备坐标系。
     - `sample_pdf`：分层采样函数。
3. **load_llff.py**:
   - 用于加载LLFF（Local Light Field Fusion）数据集，该数据集包含用于3D重建的多视点图像。
4. **load_blender.py**:
   - 用于加载Blender数据集中的图像和相机姿态，主要用于训练NeRF模型。
5. **load_deepvoxels.py**:
   - 用于加载DeepVoxels数据集，该数据集包含从不同视点捕获的3D场景。
6. **load_LINEMOD.py**:
   - 用于加载LINEMOD数据集，该数据集常用于3D对象检测和姿态估计。

**子目录：**

1. `config\`: 包含了训练的配置文本文件，有一些基础的训练设置

2. `data\`：所拍摄的训练图片，格式为 llff 数据集。

3. `logs\`: 日志目录

   - `summary`: tensorboard 日志
   - `bag`: 对文具袋的训练与渲染结果

   - `test`: 对比实验中对易拉罐的训练与渲染结果

4. `LLFF-master`: 用于将相机位姿数据转化为llff格式的脚本。（来自repo：https://gitcode.com/Fyusion/LLFF.git）



### 训练：

#### （1）数据集准备

​	将自己制作的图片数据集通过 COLMAP 估计相机位姿后，使用 llff 脚本将其转化为 llff 格式。之后如果分辨率过高可能需要使用 `data/downsample.py` 文件对图片进行下采样。

#### （2） 训练模型

​	在 config 目录下加入自己的配置文件，注意数据集格式要与自己制作的数据集格式相同。配置后直接运行 `run_nerf.py` 即可

```cmd
python run_nerf.py --config configs/test.txt
```



### 渲染结果查看：

​	本项目中已对原本的 nerf-pytorch 中的 log 机制进行了修改，训练完成后，能够在 `logs\summaries` 中找到 tensorboard 的日志，并在 `logs` 中对应的子目录下找到不同训练轮数时的视频渲染结果，以及模型权重、训练日志等。





---

​								*本项目为 Fudan University Computer Vision 课程作业，作者：律己zZZ*





