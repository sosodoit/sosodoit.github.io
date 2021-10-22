---
layout: page
title: "Pytorch 기반 EfficientNet 구현"
description:
headline:
categories: deep-learning
tags: [DL]
comments: true
published: true
# typora-copy-images-to: ../images/2021-04-26 
---

# 중앙선 침범, 끼어들기 범칙행위 감지 
(신고 서비스 제안)

```
# 개발환경
Ubuntu 18.04, docker
tensorflow, pytorch, GPU
```

1. 차량, 번호판 Detection
- 사용모델: yolov3, 4, 5, EfficientDet, RetinaNet
- 성능비교: mAP
- 학습데이터: 2318개

2. 번호판 recognition
- 사용모델: tesseract ocr
- 성능향상필요
- 모델찾기

3. 차선 Detection
- 사용모델: mask r cnn, segmentation
- 성능비교: mAP

4. 최종 모델 결합
1 + 2 + 3 = make rule = 범칙행위 감지 통합모델

5. 신고 서비스 제안

---

## ERD

감지할 영상 넣음 -> 모델돌림 -> 결과나옴 -> detection 결과 -> 신고함(자동)

- 감지할 영상에 일자, 시간, 차량번호, 장소, 상세위치가 있어야함?
- 결과에 위 사항을 db에 저장하고 detecition 결과 좌표, 이미지경로 등 저장
- 정보테이블, 조회테이블, 신고테이블

---

<br><br>

# 🚀 TODAY GOAL
차량검출: EfficientDet, RetinaNet
번호판인식: tesseract 성능향상(이미지처리), 다른모델탐색
> 담당: EfficientDet(소희), RetinaNet(태경)

## 🚀 Getting Started 
우분투 18.04에서 도커 cuda10, 파이토치, 주피터 노트북 환경 만들기
tensorflow-gpu, python, cuDNN, CUDA 버전 [확인](https://www.tensorflow.org/install/source#gpu) 

``` powershell ubuntu
$ sudo service docker start
$ sudo chmod 666 /var/run/docker.sock # 도커 명령어를 입력할 때마다 sudo를 추가적으로 입력하기 귀찮으므로 권한 부여

# 1. Nvidia 도커 설치
# 도커에서 gpu를 사용하기 위해서는 nvidia 도커를 설치 해야한다.
# 먼저 nvidia-driver가 설치 된 이후 아래를 입력한다.
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
$ curl -s -L https://nvidia.github.io/nvidia-docker/ubuntu16.04/amd64/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
$ sudo apt-get update
$ sudo apt-get install -y nvidia-docker2
$ sudo systemctl restart docker
# 설치가 정상적으로 이루어졌으면 아래 명령어를 통해 접속이후
$ nvidia-doker run -it 이미지명:태그
# 도커 컨테이너 안에서 아래를 입력후 정보가 나오면 된다. 명령어는 docker와 동일하고 앞에 nvidia-라는 접두어만 추가하면 된다.
$ nvidia-smi

# 2. pytorch 도커 내려받기 https://hub.docker.com/r/pytorch/pytorch/tags
# 필요한 버전에 맞게 내려받고
$ docker pull pytorch/pytorch:1.1.0-cuda10.0-cudnn7.5-devel
# 접속 명령어
$ docker run -it docker run -it pytorch/pytorch:1.1.0-cuda10.0-cudnn7.5-devel

# 3. 도커 push
$ docker login
$ docker ps -a
$ docker commit -m "메시지" 컨테이너명 이미지명:태그
$ docker push 컨테이너명

# 도커에서 많이 쓰는 명령어
$ docker run 이미지명:태그 # 실행만 (바로 종료될 수 있음)
$ docker run -it 이미지명:태그 # 실행이후 컨테이너 접속
$ docker run -d 이미지명:태그 # 이미지 background에서 실행
$ docker run --name 컨테이너명 이미지명:태그 # 해당 이미지의 이름을 "컨테이너명"으로 실행
$ docker run -p 28888:8888 이미지명:태그 # 해당 이미지를 실행하고 8888을 로컬 포트 28888에 연결
$ docker ps # 실행 중인 컨테이너 확인
$ docker ps -a # 모든 컨테이너 상태 확인
$ docker start 컨테이너명 # exited 된 컨테이너 실행
$ docker stop 컨테이너명 # 실행중인 컨테이너 중지
$ docker attach # 실행중인 컨테이너 bash에 접속 (run -it 느낌)
$ docker rm 컨테이너명 # 컨테이너 삭제
$ docker images # 현재 내려받은 이미지들 확인
$ docker rmi 이미지명:태그 # 해당 이미지 삭제
$ docker -v host_directory:container_directory # 내 디렉토리와 컨테이너 디렉토리 연결
$ docker --shm-size=8G # 공유 메모리를 8G로 설정

# 4. jupyter notebook
# 도커 컨테이너에 접속할 때 -p로 컨테이너의 포트와 로컬의 포트를 연결할 수 있다.
# 이렇게 입력을 하면 컨테이너의 8888포트는 로컬 포트 28888에, 6006은 26006에 연결된다.
$ nvidia-docker run -it -p 28888:8888 -p 26006:6006 이미지명:태그
$ pip install notebook
$ jupyter notebook --generate-config
# /root/.jupyter/jupyter_notebook_config.py 해당 파일을 열고 아래 입력 후 주피터 실행
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888
c.NotebookApp.allow_root = True
c.NotebookApp.password = ''
```