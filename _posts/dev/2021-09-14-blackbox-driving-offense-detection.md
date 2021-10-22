---
layout: page
title: "3차 프로젝트 시작"
description:
headline:
categories: deep-learning
tags: [DL]
comments: true
published: true
# typora-copy-images-to: ../images/2021-04-26 
---

[시연](#) Blackbox driving offense detection

## Dev-log
- [PR3-YOLOv4공부] YOLO v4 object detector 학습, 코랩
- [PR3-정리글] YOLOv3모델과 CRNN모델을 활용한 차량 번호판 인식 시스템
- [PR3-데이터확인] 자동차 차종/연식/번호판 인식용 영상
- [PR3-논문리뷰] 가상 데이터를 활용한 번호판 문자 인식 및 차종 인식 시스템 제안
- [PR3-YOLOv3공부] YOLO v3 object detector 학습, 주피터
- [PR3-성능평가지표] Object Detection 모델의 성능 평가 지표
- [PR3-성능평가지표] Python 구현
- [PR3-WSL2에서도커] 도커의 모든 것
- [PR3-WSL2에서도커] YOLOv5개발환경구축 - 코드만, 코랩
- [PR3-WSL2] Ubuntu18.04 설치
- [PR3-EffiecntNet] EffiecntNet object detector 학습

## 🚀 Algorithm

### 개발환경
1. Ubuntu 18.04
2. docker
3. GPU

### 중앙선 침범, 끼어들기 범칙행위 탐지
1. 차량 및 번호판 Detection
- yolov3, v4, v5, Resnet, EffiecntNet :mAP을 통한 성능비교
- tesseract ocr 활용한 번호판 인식(한글 포함) 

2. 차선 Detection
- mask r cnn, segmentation :mAP을 통한 성능비교

3. 최종 모델 결합

4. 신고 서비스 제안

<br>

## 🚀 Getting Started

### Install
#### npm 설치
```
cd frontend
npm install
```
> `frontend` 디렉토리에서 수행해야 합니다.

### Usage
#### webpack server 구동
```
npm run dev
```
#### application 구동
```
./gradlew bootRun
```
<br>

## ✏️ Code Review Process
[텍스트와 이미지로 살펴보는 온라인 코드 리뷰 과정](#)

<br>

## 🐞 Bug Report

버그를 발견한다면, [Issues](#) 에 등록해주세요 :)

<br>

## 📝 License

This project is [MIT](#) licensed.