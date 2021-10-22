---
layout: page
title: "EfficientDet 논문 리뷰"
description:
headline:
categories: deep-learning
tags: [DL]
comments: true
published: true
# typora-copy-images-to: ../images/2021-04-26 
---

# EfficientDet(2020), Scalable and Efficient Object Detection

**EfficientDet: scalable하고 효율적인 객체감지**

efficientdet은 backbone으로 efficientnet을 사용하며, weighted bi-directional feature pyramid network(BiFPN)과 compound scaling을 제안합니다.

BiFPN은 cross-scale connection과 weighted feature fusion을 사용합니다.

(1) cross-scale connection

기존의 FPN은 한 방향으로만 정보가 흐른다는 단점이 존재합니다. 이를 해결하기 위해 PANET은 bottom-up path를 추가합니다. NAS-FPN은 architecture search로 찾은 FPN의 구조를 사용하지만 일정한 규칙성이 없어 해석하기 어렵습니다. 저자는 PANET이 FPN과 NAS-FPN보다 좋은 성능을 보인다는 것을 발견하고 약간의 수정과 개선을 합니다.

PANet은 하나의 top-down과 하나의 bottom-up이 존재하지만, BiFPN은 양방향을 지닌 레이어를 여러개 쌓습니다. bidirectional한 레이어를 여러개 쌓음으로써 high-level feature fusion이 가능해집니다.

(2) weighted feature fusion

FPN내의 모든 input features는 output feature에 동일하지 않은 영향력을 갖습니다. 이 문제를 해결하기 위해 input feature에 가중치를 가합니다. 각 input feature의 중요도에 따라 가중치를 가하는 것입니다.

efficientnet은 resolution, width, depth 3가지 요소를 주어진 자원에 맞게 동시에 scale up 했습니다. object detection은 classification보다 고려해야 할 요소가 더 많습니다. 이 논문에서는 backbone, BiFPN, class/box network, resolution 4가지 요소를 동시에 scale up 합니다.

* EfficientDet D7