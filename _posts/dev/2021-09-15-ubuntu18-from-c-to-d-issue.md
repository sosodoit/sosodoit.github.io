---
layout: page
title: "우분투 18.04에서 드라이브 용량 관리 이슈"
description:
headline:
categories: deep-learning
tags: [DL]
comments: true
published: true
# typora-copy-images-to: ../images/2021-04-26 
---

# ISSUE
Ubuntu, Docker를 사용하면서 점점 C드라이브 용량의 부족현상을 마주했다. 학습하는 데이터가 다양하고 큰 것도 한몫했다. 따라서 서버 컴퓨터에 SSD를 추가 장착하였고 개발환경을 D드라이브로 옮겨야했다. 본 글은 그 이슈에 대해서 기록하였다.

<br>

## [용량관리#1] C드라이브에서 D드라이브로 Ubuntu, Docker 데이터 저장공간 변경
Docker의 데이터를 이동하려면 Ubuntu의 설치공간을 이동해야 한다.
- [Move WSL](https://github.com/pxlrbt/move-wsl) 사용
- Move WSL은 WSL distros VHDX 파일을 새 위치로 쉽게 이동 할 수 있게 해준다.
    - distro == 배포판(distribution)
    - VHDX == 파일포맷(virtual hard drvie file)

설치된 WSL리스트 확인
```powershell
wsl -l -v
# Ubuntu
# Ubuntu-18.04
# docker-desktop-data
# docker-desktop
```
Docker의 기본공간을 변경하려면 docker-destop-data, docker-desktop 모두 이동해야 한다(외장 SSD인 D 드라이브로).

<br>

### 시작
💡실행 터미널 : git bash <br>
💡service docker stop 꼭 멈추고 해야함

1. 클론 후 해당 파일이 있는 곳으로 경로 이동하고 아래 명령어를 실행
직접 경로를 확인하기 위해 Explicit way 명시적 방법을 사용함
```bash
#Get a list of WSL distros: 
wsl -l
#Move your image: 
./move-wsl [NAME] [LOCATION]
#Example: ./move-wsl Ubuntu-18.04 /d/wsl
#Example: ./move-wsl docker-desktop /d/docker
#Example: ./move-wsl docker-desktop-data /d/docker/data
```

---

### 내부 살펴봄
```powershell
# 목적지 폴더에 백업데이터를 tar 파일로 옮김
wsl --export docker-desktop-data "D:\wsl\docker\data\docker-desktop-data.tar"
# 기존 위치의 wsl distro 등록을 해제
wsl --unregister docker-desktop-data
# 복사한 tar 파일을 임포트 및 등록
wsl --import docker-desktop-data "D:\wsl\docker\data" "D:\wsl\docker\data\docker-desktop-data.tar" --version 2
```

<br>

---

## [용량관리#1-2] 이동 후 계정이 root로 변경된다는점 
1. 우분투 기본 계정 변경이 필요하다면([관련이슈참고](https://askubuntu.com/questions/816732/how-to-change-default-user-in-wsl-ubuntu-bash-on-windows-10))
```powershell
ubuntu1804.exe config --default-user ksh
sc stop LxssManager
sc start LxssManager
```

<br>

---

## [용량관리#2] 이미지, 컨테이너 관리([공식문서참고](https://docs.docker.com/engine/reference/commandline/system_prune/#description))
1. rm, rmi 명령어를 사용해서 이미지와 컨테이너를 지웠는데 docker 경로의 용량변화가 없을때
```powershell ubuntu
docker system prune -a -f
```
- -a : 컨테이너에 연결되지 않은 사용하지 않는 모든 이미지들 제거
- -f : 프롬프트로 확인 하지 않음

<br>

2. docker 기본 경로 변경
💡 docker 기본 경로를 변경하면 기존 경로에 있던 이미지와 컨테이너들은 삭제된다.
- 기본 경로 : /var/lib/docker/
- 바꿀 경로 : /data/docker/

> 🙋🏻‍♂️ 아래의 방법은 도커 설정 파일을 통해 변경하는 방법으로 /lib/systemd/system/docker.service 파일을 수정하는 방법도 있지만 아래의 방법을 권장한다고 한다.

- 2-0. 모든 작업을 하기 전에 docker 중지
```powershell ububtu
service docker stop
mkdir -p /data/docker
```

- 2-1. daemon.json 파일 생성(파일이 있다면 2-1-1.)

    도커가 실행할 때 이 파일을 읽어 path를 결정하게 된다. 
    변경될 경로를 json 파일에 넣어주고 저장 후 도커를 재시작한다.
```
vi /etc/docker/daemon.json
```
```json
{
  "graph": "/data/docker" # 원하는 경로로 설정
}
```
```
sudo service docker start
```

- 2-1-1. daemon.json에 data-root항목을 찾아 새로 생성한 폴더경로를 입력
```
#/etc/docker/daemon.json
...
"data-root": "/data/docker",
...
```

- 2-2. 컨테이너 실행후 도커 확인

    도저 재시작 후 컨테이너 실행 후
    아래 명령어로 확인해보며 변경된 경로로 나오는 것을 확인할 수 있다.
```
service docker start
...
ps -ef | grep docker
```
