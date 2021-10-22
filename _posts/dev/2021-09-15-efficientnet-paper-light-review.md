---
layout: page
title: "EfficientNet 논문 리뷰"
description:
headline:
categories: deep-learning
tags: [DL]
comments: true
published: true
# typora-copy-images-to: ../images/2021-04-26 
---


# EfficientNet(2019), Rethinking Model Scaling for Convolutional Neural Networks

**EfficientNet: 컨볼루션 신경망을 위한 모델 스케일링 재검토**

모델의 정확도를 높일 때, 일반적으로 
(1) 모델의 깊이 
(2) 너비
(3) 입력 이미지의 크기를 조절합니다.

기존에는 이 세가지를 수동으로 조절하였기 때문에, 최적의 성능과 효율을 얻지 못했습니다. 
EfficientNet은 3가지를 효율적으로 조절할 수 있는 compound scaling 방법을 제안합니다.
깊이, 너비, 입력 이미지 크기가 일정한 관계가 있다는 것을 실험적으로 찾아내고, 이 관계를 수식으로 만듭니다. 
compound scaling 방법으로 NAS(neural architecture search) 구조를 수정하여 SOTA를 달성합니다. 
ResNet이나 MobileNet 등 CNN 기반 구조에도 효과가 있다고 합니다.

일반적으로 모델을 scaling하는 방법은 너비, 깊이, 입력 해상도를 조절하는 것입니다. 각 w, d, r 이 상수들의 관계를 연구한 것이 efficientnet입니다.
w, d, r이 일정 값 이상이 되면 정확도가 빠르게 saturate합니다. 
그리고 w, d, r을 함께 조절하여 최고의 효율을 찾아내는 것이 compound scailing입니다.

efficientnet은 mnasnet과 동일한 search space를 사용해서 찾은 구조입니다.
따라서 mnasnet 구조와 비슷합니다.

* EfficientNet B6