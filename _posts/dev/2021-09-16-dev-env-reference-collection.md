---
layout: page
title: "딥러닝 개발환경에 관하여"
description:
headline:
categories: deep-learning
tags: [DL]
comments: true
published: true
# typora-copy-images-to: ../images/2021-04-26 
---

# [Docker - 설치부터 배포까지](https://velog.io/@swhybein/Docker-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)
1. 설치하기(ubuntu)
2. Docker 파일 만들기
3. 이미지 빌드하기
4. 이미지 실행하기
5. 실행한 컨테이너 보기
6. 실행한 컨테이너 종료하기
7. 도커 로그인 하기
8. 도커 저장소에 이미지 푸쉬하기
9. 컨테이너 삭제하기
10. (다른 서버에서) 이미지 가져오기

# [Docker Linux 빌드](https://www.tensorflow.org/install/source#gpu)

Docker는 컨테이너를 사용하여 TensorFlow 설치를 나머지 시스템에서 격리하는 가상 환경을 만듭니다. TensorFlow 프로그램은 호스트 머신과 리소스를 공유(디렉터리 액세스, GPU 사용, 인터넷 연결 등)할 수 있는 이 가상 환경 내에서 실행됩니다.

TensorFlow의 Docker 개발 이미지를 사용하면 소스에서 Linux 패키지를 빌드하는 환경을 간편하게 설정할 수 있습니다. 이 이미지에는 TensorFlow를 빌드하는 데 필요한 소스 코드 및 종속 항목이 이미 포함되어 있습니다. 설치 및 사용 가능한 이미지 태그 목록은 TensorFlow Docker가이드를 참조하세요.

Docker를 사용하면 TensorFlow GPU 지원을 가장 간편하게 빌드할 수 있습니다. 호스트 머신에 NVIDIA® 드라이버만 있으면 되고 NVIDIA® CUDA® Toolkit을 설치할 필요가 없기 때문입니다. nvidia-docker(Linux만 해당)를 설정하려면 GPU 지원 가이드 및 TensorFlow Docker 가이드를 참고하세요.

---
1. TensorFlow 설치(TensorFlow 컨테이너 실행을 통해)<br>    
    a. TensorFlow는 다음 64비트 시스템에서 테스트 및 지원됩니다.
    - Python 3.6~3.9
    - Ubuntu 16.04 이상
    - Windows 7 이상

2. TensorFlow Docker 요구사항<br>
    a. 로컬 호스트 머신에 Docker를 설치합니다.<br>
    b. Linux에서 GPU를 지원하려면 NVIDIA Docker 지원을 설치합니다.
    - docker -v로 Docker 버전을 기록합니다. 
    - 19.03 이전 버전에는 nvidia-docker2와 --runtime=nvidia 플래그가 필요합니다. 
    - 19.03 이상 버전에는 nvidia-container-toolkit 패키지와 --gpus all 플래그를 사용합니다. 
    > Docker version 20.10.8

3. 시작
Ubuntu LTS - 16.04, 18.04, 20.4 및 Debian - Stretch, Buster 배포판에서 NVIDIA Container Toolkit을 설정할 수 있습니다.

``` powershell ubuntu
#도커 설정
# Ubuntu의 Docker-CE는 Docker의 공식 편의 스크립트를 사용하여 설정할 수 있습니다.
$ curl https://get.docker.com | sh && sudo systemctl --now enable docker

# NVIDIA Container Toolkit 설정
# 설정 stable저장소와 GPG 키 :
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

# nvidia-docker2패키지 목록을 업데이트한 후 패키지(및 종속성)를 설치합니다 .
$ sudo apt-get update
$ sudo apt-get install -y nvidia-docker2
# 기본 런타임을 설정한 후 Docker 데몬을 다시 시작하여 설치를 완료합니다.
$ sudo systemctl restart docker
# 이 시점에서 기본 CUDA 컨테이너를 실행하여 작동 설정을 테스트할 수 있습니다.
$ sudo docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi
```

``` powershell ubuntu
# apt를 사용하여 CUDA 설치
# Add NVIDIA package repositories
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/ /"
sudo apt-get update

wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/nvidia-machine-learning-repo-ubuntu1804_1.0.0-1_amd64.deb

sudo apt install ./nvidia-machine-learning-repo-ubuntu1804_1.0.0-1_amd64.deb
sudo apt-get update

wget https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/libnvinfer7_7.1.3-1+cuda11.0_amd64.deb
sudo apt install ./libnvinfer7_7.1.3-1+cuda11.0_amd64.deb
sudo apt-get update

# Install development and runtime libraries (~4GB)
sudo apt-get install --no-install-recommends \
    cuda-11-0 \
    libcudnn8=8.0.4.30-1+cuda11.0  \
    libcudnn8-dev=8.0.4.30-1+cuda11.0

# Reboot. Check that GPUs are visible using the command: nvidia-smi

# Install TensorRT. Requires that libcudnn8 is installed above.
sudo apt-get install -y --no-install-recommends libnvinfer7=7.1.3-1+cuda11.0 \
    libnvinfer-dev=7.1.3-1+cuda11.0 \
    libnvinfer-plugin7=7.1.3-1+cuda11.0

# Windows 설정
# CUDA®, CUPTI 및 cuDNN 설치 디렉터리를 %PATH% 환경 변수에 추가합니다. 예를 들어 CUDA® Toolkit이 C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.0에 설치되고 cuDNN이 C:\tools\cuda에 설치된 경우 다음과 일치하도록 %PATH%를 업데이트합니다.
SET PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.0\bin;%PATH%
SET PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.0\extras\CUPTI\lib64;%PATH%
SET PATH=C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.0\include;%PATH%
SET PATH=C:\tools\cuda\bin;%PATH%
```

``` powershell ubuntu
# TensorFlow Docker 이미지 다운로드
$ docker pull tensorflow/tensorflow:latest  # Download latest stable image
$ docker run -it -p 8888:8888 tensorflow/tensorflow:latest-jupyter  # Start Jupyter server 
# TensorFlow Docker 컨테이너 시작
$ docker run [-it] [--rm] [-p hostPort:containerPort] tensorflow/tensorflow[:tag] [command]

# GPU를 사용할 수 있는지 확인합니다.
$ lspci | grep -i nvidia
# nvidia-docker 설치를 확인합니다.
$ docker run --gpus all --rm nvidia/cuda nvidia-smi
# 참고: nvidia-docker v2에서는 --gpus all 대신 --runtime=nvidia를 사용합니다. nvidia-docker v1은 --runtime=nvidia 또는 --gpus all 명령줄 플래그 대신 nvidia-docker 별칭을 사용합니다.

# GPU 사용 이미지를 사용한 예제
# GPU 사용 TensorFlow 이미지를 다운로드하여 실행합니다(몇 분 정도 걸릴 수 있음).
$ docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \
   python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
# bash 셸 세션에서 시작 
$ docker run --gpus all -it tensorflow/tensorflow:latest-gpu bash
> cd /tmp  # don't import from source directory
> python -c "import tensorflow as tf; print(\"Num GPUs Available: \", len(tf.config.list_physical_devices('GPU')))"
```