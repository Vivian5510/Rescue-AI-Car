[English](README.md) | 简体中文
# PaddleDetection RKNPU2 Python部署示例

本目录下用于展示PaddleDetection系列模型在RKNPU2上的部署，以下的部署过程以PPYOLOE为例子。

## 1. 部署环境准备
在部署前，需确认以下步骤

- 1. 软硬件环境满足要求，RKNPU2环境部署等参考[FastDeploy环境要求](https://github.com/PaddlePaddle/FastDeploy/blob/develop/docs/cn/faq/rknpu2/rknpu2.md)

## 2. 部署模型准备

模型转换代码请参考[模型转换文档](../README.md)

## 3. 运行部署示例  

本目录下提供`infer.py`快速完成PPYOLOE在RKNPU上部署的示例。执行如下脚本即可完成

```bash
# 下载部署示例代码
git clone https://github.com/PaddlePaddle/PaddleDetection.git
cd PaddleDetection/deploy/fastdeploy/rockchip/rknpu2/python
# 注意：如果当前分支找不到下面的fastdeploy测试代码，请切换到develop分支
# git checkout develop

# 下载图片
wget https://bj.bcebos.com/paddlehub/fastdeploy/rknpu2/ppyoloe_plus_crn_s_80e_coco.zip
unzip ppyoloe_plus_crn_s_80e_coco.zip
wget https://gitee.com/paddlepaddle/PaddleDetection/raw/release/2.4/demo/000000014439.jpg

# 运行部署示例
python3 infer.py --model_file ./ppyoloe_plus_crn_s_80e_coco/ppyoloe_plus_crn_s_80e_coco_rk3588_quantized.rknn  \
                 --config_file ./ppyoloe_plus_crn_s_80e_coco/infer_cfg.yml \
                 --image_file 000000014439.jpg
```

# 4. 更多指南
RKNPU上对模型的输入要求是使用NHWC格式，且图片归一化操作会在转RKNN模型时，内嵌到模型中，因此我们在使用FastDeploy部署时，需要先调用DisableNormalizeAndPermute(C++)或`disable_normalize_and_permute(Python)，在预处理阶段禁用归一化以及数据格式的转换。

- [C++部署](../cpp)
- [转换PaddleDetection RKNN模型文档](../README.md)