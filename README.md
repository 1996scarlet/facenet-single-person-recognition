# Forked from insightface

## Deep Face Recognition

### Introduction

In this repository, we provide training data, network settings and loss designs for deep face recognition.
The training data includes the normalised MS1M, VGG2 and CASIA-Webface datasets, which were already packed in MXNet binary format.
The network backbones include ResNet, MobilefaceNet, MobileNet, InceptionResNet_v2, DenseNet, DPN.
The loss functions include Softmax, SphereFace, CosineFace, ArcFace and Triplet (Euclidean/Angular) Loss.


![margin penalty for target logit](https://github.com/deepinsight/insightface/raw/master/resources/arcface.png)

Our method, ArcFace, was initially described in an [arXiv technical report](https://arxiv.org/abs/1801.07698). By using this repository, you can simply achieve LFW 99.80%+ and Megaface 98%+ by a single model. This repository can help researcher/engineer to develop deep face recognition algorithms quickly by only two steps: download the binary dataset and run the training script.

### Training Data

All face images are aligned by [MTCNN](https://kpzhang93.github.io/MTCNN_face_detection_alignment/index.html) and cropped to 112x112:

Please check [Dataset-Zoo](https://github.com/deepinsight/insightface/wiki/Dataset-Zoo) for detail information and dataset downloading.


* Please check *src/data/face2rec2.py* on how to build a binary face dataset. Any public available *MTCNN* can be used to align the faces, and the performance should not change. We will improve the face normalisation step by full pose alignment methods recently.

### Train

5. Verification results.

*LResNet100E-IR* network trained on *MS1M-Arcface* dataset with ArcFace loss:

| Method | LFW(%) | CFP-FP(%) | AgeDB-30(%) |
| ------ | ------ | --------- | ----------- |
| Ours   | 99.80+ | 98.0+     | 98.20+      |



### Pretrained Models

You can use `$INSIGHTFACE/src/eval/verification.py` to test all the pre-trained models.

**Please check [Model-Zoo](https://github.com/deepinsight/insightface/wiki/Model-Zoo) for more pretrained models.**



### Verification Results on Combined Margin

A combined margin method was proposed as a function of target logits value and original `θ`:

```
COM(θ) = cos(m_1*θ+m_2) - m_3
```

For training with `m1=1.0, m2=0.3, m3=0.2`, run following command:
```
CUDA_VISIBLE_DEVICES='0,1,2,3' python -u train_softmax.py --network r100 --loss combined --dataset emore
```

Results by using ``MS1M-IBUG(MS1M-V1)``

| Method           | m1  | m2  | m3   | LFW   | CFP-FP | AgeDB-30 |
| ---------------- | --- | --- | ---- | ----- | ------ | -------- |
| W&F Norm Softmax | 1   | 0   | 0    | 99.28 | 88.50  | 95.13    |
| SphereFace       | 1.5 | 0   | 0    | 99.76 | 94.17  | 97.30    |
| CosineFace       | 1   | 0   | 0.35 | 99.80 | 94.4   | 97.91    |
| ArcFace          | 1   | 0.5 | 0    | 99.83 | 94.04  | 98.08    |
| Combined Margin  | 1.2 | 0.4 | 0    | 99.80 | 94.08  | 98.05    |
| Combined Margin  | 1.1 | 0   | 0.35 | 99.81 | 94.50  | 98.08    |
| Combined Margin  | 1   | 0.3 | 0.2  | 99.83 | 94.51  | 98.13    |
| Combined Margin  | 0.9 | 0.4 | 0.15 | 99.83 | 94.20  | 98.16    |


### 512-D Feature Embedding

In this part, we assume you are in the directory *`$INSIGHTFACE_ROOT/deploy/`*. The input face image should be generally centre cropped. We use *RNet+ONet* of *MTCNN* to further align the image before sending it to the feature embedding network.

1. Prepare a pre-trained model.
2. Put the model under *`$INSIGHTFACE_ROOT/models/`*. For example, *`$INSIGHTFACE_ROOT/models/model-r100-ii`*.
3. Run the test script *`$INSIGHTFACE_ROOT/deploy/test.py`*.

For single cropped face image(112x112), total inference time is only 17ms on our testing server(Intel E5-2660 @ 2.00GHz, Tesla M40, *LResNet34E-IR*).


## Face Alignment

Todo

## Face Detection

Todo

## Citation

If you find *InsightFace* useful in your research, please consider to cite the following related papers:

```
@inproceedings{deng2018arcface,
title={ArcFace: Additive Angular Margin Loss for Deep Face Recognition},
author={Deng, Jiankang and Guo, Jia and Niannan, Xue and Zafeiriou, Stefanos},
booktitle={CVPR},
year={2019}
}
```
