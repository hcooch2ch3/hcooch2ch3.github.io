---
layout: post
title: LeRobot - Visualize Datasets
tags:
  - LeRobot
  - Rerun
---


이 글은 LeRobot 스터디에 참여하면서 한 주 동안 진행한 내용이다. (LeRobot 레포의 README 중 일부 내용)

처음엔 이게 Gazebo 같은 시뮬레이터로 SO-101 로봇팔을 시뮬레이팅 하는 것이라 생각하고 진행했는데, SO-101도 아니었고 시뮬레이터도 아니었다. ALOHA 로봇팔에 대한 데이터셋을 시각화?하는 방법에 좀 더 가까운 것 같다. 

“시뮬레이션”이란 가상의 환경을 실시간으로 계산 / 갱신하며 새로운 상호작용을 만들어내는 과정이고, visualize_dataset은 그런 과정 없이 기록된 데이터를 재생하는 ‘리플레이 / 플레이어’이기 때문에 이건 시뮬레이션이 아니라고 한다.

아무튼, 정리한 배경지식 & 용어와 ALOHA 데이터셋 시각화 방법은 다음과 같다.

## 배경 지식 및 용어

### LeRobot

Hugging Face가 개발한 오픈소스 로봇 AI 플랫폼으로, 로봇 제어를 위한 데이터셋 / 모델 / 시뮬레이션 환경을 통합 제공한다.

- **특징**
    - 모방학습, 강화학습 모두 지원하며, 시뮬레이션과 실제 로봇을 모두 대상으로 학습/평가 가능
    - 이미지 / 로봇 상태(state) / 행동(action)을 결합한 end-to-end 정책 학습을 구현할 수 있다.
    - Hugging Face Hub와 연동되어 모델, 데이터 공유가 쉽다.
- **제약**
    - 실제 하드웨어에 적용할 때는 센서 잡음, 마찰 등 **sim2real 간극**과 비용 문제 등 현실적 제약이 존재한다.

### ALOHA (A Low-cost Open-source HArdware)

저비용 / 개방형 설계를 기반으로 한 양손(bimanual) 로봇 조작 연구용 시스템

- **목적**: 복잡한 조작(manipulation) 과제를 저비용 하드웨어와 동일 API로 실험, 학습할 수 있도록 설계
- **구성**
    - 보통 **6자유도(6-DOF) ViperX 팔 2개(follower)**와 더 작은 **leader 팔**을 결합해 **원격조작(teleoperation)** 데이터 수집이 가능
    - 연구 세팅에 따라 부품/자유도 구성이 다를 수 있음
- **특징**
    - 시뮬레이터와 실제 로봇을 동일한 인터페이스로 다룰 수 있어 **sim2real 이전**을 쉽게 시도할 수 있다.
    - 전체 시스템 비용은 약 **2만 달러 수준**(약 2700 ~ 2800 만원)으로, 산업용 로봇 대비 저렴하지만 연구 장비치고는 여전히 고가

### PushT (Push Task)

탁자 위 물체를 목표 지점까지 **미는(push)** 조작을 수행하는 **간단한 벤치마크 환경**.

- **목적**: 시각 기반 강화학습/모방학습 알고리즘을 **반복 가능하고 비교 가능한 실험**으로 평가하기 위함
- **특징**
    - 2D 평면에서 물체를 목표로 이동시키며, **경로 계획/접촉 물리(contact physics)** 등 기본 제어 능력을 검증하기 적합
    - 이미지/상태 정보 모두를 관찰값으로 활용 가능하고, 빠른 학습/알고리즘 비교가 용이하다.

### Rerun.io
- **목적**: 센서·이미지·3D 데이터를 실시간 스트리밍하고 시각화하여 디버깅 / 분석을 할 수 있게 하는 툴
- **특징**: TensorBoard와 유사하지만 3D·로봇/센서 데이터 실시간 시각화에 강점이며, Gazebo·Unity 같은 시뮬레이터가 아닌 시뮬레이션, 실로봇에서 생성된 데이터를 시각화하는 용도로 사용된다.
- **공식 사이트**: [https://rerun.io](https://rerun.io)

### 기타 용어
- **End-to-end 정책 학습**: 이미지 등 원시 센서 입력을 받아 곧바로 행동을 출력하는 하나의 신경망을 학습하는 방식이다. 모듈별 오류 누적이 없고 전체 최적화가 가능하다는 장점이 있지만, 해석이 어렵고 대량의 데이터 및 안전성을 확보해야 한다는 단점이 있다.

- **sim2real**: 시뮬레이터에서 학습, 개발한 로봇 제어 정책이나 모델을 실제 환경에 적용하는 기술을 말함

- **miniconda**: Conda는 파이썬을 포함한 다양한 언어의 패키지 관리와 가상환경 관리를 함께 제공하는 오픈 소스 도구이고, Miniconda는 파이썬 패키지 관리와 가상환경을 제공하는 conda의 핵심 기능만 담은 경량 배포판 

## 실행해보기
### 실행 환경
- Apple Silicon (Macbook Pro M4)

### 실행 방법
(1) miniconda 설치
- [https://www.anaconda.com/download/success](https://www.anaconda.com/download/success)

![miniconda](/assets/images/lerobot/miniconda.png)

(2) conda 명령어로 가상환경 생성 및 활성화

```shell
conda create -y -n lerobot python=3.10

conda activate lerobot
```

![2](/assets/images/lerobot/2.png)

(3) [첫 설치일 경우] conda 약관 확인 명렁어 입력

```shell
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main

conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r
```

(4) 현재 활성화된 conda 환경에 conda-forge 채널을 사용해 FFmpeg를 설치

```shell
conda install ffmpeg -c conda-forge
```

- FFmpeg는 오디오·비디오를 인코딩, 디코딩, 변환, 녹화, 스트리밍할 수 있는 오픈 소스 멀티미디어 프레임워크
- LeRobot는 로봇 관련 AI (시뮬레이션, imitation learning, 행동 학습) 데이터를 다루기 때문에 이 패키지가 필요하다. 데이터셋에 비디오 파일이 포함되고, 이 데이터를 프레임 단위로 처리하기 위한 목적.

![4](/assets/images/lerobot/4.png)

(5) 프로젝트 클론 및 해당 프로젝트 경로로 이동

```shell
git clone https://github.com/huggingface/lerobot.git

cd lerobot
```

![5](/assets/images/lerobot/5.png)

(6) 파이썬 패키지 설치

```shell
 pip install -e .
 
 pip install 'lerobot[all]'
```

- pip install -e .
    - pip install -e . : 현재 디렉토리(.)의 파이썬 패키지를 *editable mode*로 설치
    - e(editable) : 설치 후에도 소스 코드를 바로 수정하면 곧바로 반영
- pip install 'lerobot[all]’
    - PyPI에 올라간 lerobot 패키지를 일반(배포) 버전으로 설치
    - [all]: all extra 의존성 세트까지 포함
![6](/assets/images/lerobot/6.png)

(7) Rerun 실행

```shell
python -m lerobot.scripts.visualize_dataset \
  --repo-id lerobot/aloha_sim_insertion_human \
  --episode-index 0
```

- [rerun.io](http://rerun.io)로 aloha 시각화를 실행하는 명령어
- 즉, Hugging Face에 올라간 로봇 시뮬레이션 데이터셋의 0번 에피소드를, LeRobot이 제공하는 시각화 스크립트로 파이썬 모듈 실행 방식으로 재생하라는 의미
- 이 명령어를 실행하면 Rerun이 실행되고 하단의 재생 버튼을 누르면 로봇팔이 데이터셋에 맞게 움직인다.

![7](/assets/images/lerobot/7.png)

<video controls width="100%">
  <source src="/assets/images/lerobot/rerun.mp4" type="video/mp4">
</video>

## 참고

- HuggingFace LeRobot: [https://github.com/huggingface/lerobot](https://github.com/huggingface/lerobot)
- Mobile ALOHA: [https://mobile-aloha.github.io/](https://mobile-aloha.github.io/)
- aloha_sim_insertion_human: [https://huggingface.co/lerobot/act_aloha_sim_insertion_human](https://huggingface.co/lerobot/act_aloha_sim_insertion_human)
- rerun.io: [https://rerun.io](https://rerun.io)