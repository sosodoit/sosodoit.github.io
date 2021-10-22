---
layout: page
title: "우분투 18.04에서 주피터노트북 사용"
description:
headline:
categories: yolo
tags: [DL]
comments: true
published: true
# typora-copy-images-to: ../images/2021-04-26 
---

# Object Detection
## YOLOv5

무엇이: Classification
어디에: Localization-Bounding Box

기본구조
- Object Detection을 수행할 모델: yolov5
- 차량, 차종, 번호판 이미지 학습 -> Weights 생성
- 생성된 Weights를 적용한 yolov5 모델에 차량, 차종, 번호판 이미지를 넣어 Prediction

여기서
- yolov5에서 제공하는 학습된 Weights 사용
- 차량, 차종, 번호판이 있는 이미지를 넣어 Prediction
- 준비되어 있는 환경에서 파이썬 코드 파일을 실행하고 결과 확인

<br>

[주피터노트북 설치](https://surprisecomputer.tistory.com/46)
```
sudo apt-get update
# 우분투 18.04에는 python3가 설치되어있음
sudo apt-get install python3-pip
sudo pip3 install notebook
python3
>>> from notebook.auth import passwd
>>> passwd()
Enter password:
Verify password:
'해쉬값나옴 복사해두고'
>>> exit()
# 주피터 환경설정 파일 생성
jupyter notebook --generate-config
# <경로나옴>
sudo vi <경로>
# 제일 아래쪽으로 가서 a 눌러서 수정모드
c = get_config()
c.NotebookApp.password = u'sha1:복사한해쉬값'
c.NotebookApp.ip = '내부ip' # $ip addr show or $hostname -I
c.NotebookApp.notebook_dir = '/' # 경로설정가능
# esc :wq! enter
sudo jupyter-notebook --allow-root
# 백그라운드 모드로 실행
ctrl + z
bg
disown -h
```

라이브러리 설치
```
sudo apt-get update
sudo apte install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.7
python3.7 -V
```

<br>

1. 사전 준비 및 기본 실행 명령어
- 도커 설치 및 실행 환경: Ubuntu18.04
- 도커 이미지: ultralytics/yolov5:latest
```
docker pull ultralytics/yolov5:latest
```
- 결과 저장 폴더 생성 및 경로 확인: "C:\docker_results"
- 기본 컨테이너로 메모리와 폴더를 공유하여 이미지 실행
    - 메모리 부족을 방지하기 위해 컨테이너에 메모리 공유 "--shm-size=8G"
    - "C:\docker_results" 폴더를 컨테이너의 /home/workspace에 공유 "-vC:\docker_results:/home/workspace"
```
docker run -it --shm-size=8G -v C:\docker_results:/home/workspace ultralytics/yolov5:latest
```
- /DL/yolov5 디렉토리로 이동
- 파일 목록 조회 
```
ls data/images
```
- 몇 가지 옵션을 주고 detect.py를 실행하여 Prediction
```
python detect.py --source data/images --weights yolov5s.pt --conf 0.25 --device cpu
# data/images의 파일들을 yolov5s.pt weights를 통해 prediction
```
- 결과가 runs/detect/exp2 경로에 저장된 것을 확인
```
ls runs/detect/exp2
```
- 결과를 /home/workspace로 복사하여 공유한 디렉토리에서 확인
```
cp -r runs/detect/exp2 /home/workspace/yolov5_prediction
```

# Instance Segmentation
## YOLACT(사물경계인식모델)

Semantic Segmentation
- 모양 그 자체를 인식
- Road:0, Grass:1, Sheep:2

Instance Segentation
- Road:0, Grass:1, Sheep_1:2, Sheep_2:3, Sheep_3:4

기본구조
- Instance Segmentation을 수행할 모델: YOLACT
- 차량, 차종, 번호판 이미지 학습 -> Weights 생성
- 생성된 Weights를 적용한 yolact 모델에 차량, 차종, 번호판 이미지를 넣어 Prediction

여기서
- YOLACT에서 제공하는 학습된 Weights 사용
- 차량, 차종, 번호판이 있는 이미지 넣어 Prediction
- 준비되어 있는 환경에서 파이썬 코드 파일을 실행하고 결과 확인

- /DL/yolact 디렉토리로 이동
- 결과 폴더 생성
```
mkdir /home/workspace/yolact_prediction
```
- 파일 목록 조회 
```
ls data/sample
```
- 몇 가지 옵션을 주고 eval.py를 실행하여 Prediction
```
python eval.py --trained_model=weights/yolact_base_54_800000.pth --score_threshold=0.15 --top_k=15 --cuda=False --image=data/sample/sample.png:/home/workspace/yolact_prediction/result.png
# data/sample/sample.png를 weights/yolact_base_54_800000.pth를 사용하여 Prediction한 결과를 /home/workspace/yolact_prediction/result.png로 저장
```