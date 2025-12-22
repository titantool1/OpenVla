# OpenVla
OpenVla 프로젝트


📌 UR Robot Control using OpenVLA (Vision-Language-Action)
🔹 Overview

This project demonstrates a camera-based robot control system that connects
a Vision-Language-Action (OpenVLA) model to a real UR robot via RTDE.

Unlike typical approaches, this system:

uses only a single RGB camera

avoids depth sensors or full 3D reconstruction

focuses on minimal calibration + real-time stability



🔹 System Architecture
RGB Camera (RTSP)
   ↓
OpenVLA (action inference)
   ↓
2D pixel delta (dx, dy)
   ↓
pixel → mm calibration
   ↓
Safety clamp & smoothing
   ↓
UR robot (RTDE servoL)

🔹 Demo
<p align="center"> <img src="demo/demo.gif" width="600"/> </p>
🔹 Key Constraints

No depth camera

Real-time control required

RTSP latency and stale frame issues

Vision model output contains noise

🔹 Design Decisions

Why 2D instead of full 3D?
Full 3D pose estimation increases system complexity and error sources.
For tabletop manipulation, relative 2D motion was sufficient and more stable.

Why minimal calibration?
A simple 2×2 linear mapping was used instead of full hand-eye calibration.

🔹 Implementation Highlights

OpenVLA action inference (7D → dx, dy)

Threaded RTSP capture to avoid stale frames

Safety clamp to prevent sudden robot motion

RTDE servoL control loop

🔹 Issues & Fixes (Real-world Debugging)
Issue	Cause	Fix
RTSP frame freeze	OpenCV buffer stall	grab/retrieve + thread
RTDE timeout	Network instability	reconnect logic
Torch + HPCX conflict	libucc symbol mismatch	LD_LIBRARY_PATH fix

➡️ See details in issues_and_fixes.md

🔹 Limitations & Future Work

Only planar (XY) motion supported

Orientation control not implemented

World-model based short-horizon prediction planned

🔹 Tech Stack

Python, PyTorch

OpenVLA

OpenCV, RTSP

UR RTDE

Linux

🔹 What This Project Shows

End-to-end system integration

Real-world debugging and constraints handling

Practical decision-making under limited sensing

















📌 OpenVLA를 이용한 UR 로봇 제어
🔹 개요
이 프로젝트는 시각-언어-행동(OpenVLA) 모델을 RTDE 통신을 통해 실제 UR 로봇에 연결하여, 카메라 기반으로 로봇을 제어하는 시스템을 선보입니다.
기존의 일반적인 방식들과 달리, 이 시스템은 다음과 같은 특징을 가집니다:
단일 RGB 카메라만 사용: 별도의 복잡한 센서 없이 일반 카메라 한 대만 활용합니다.
뎁스(Depth) 센서 및 3D 재구성 배제: 깊이 측정 센서나 복잡한 3D 입체 구현 과정을 거치지 않습니다.
최소한의 캘리브레이션과 실시간 안정성에 집중: 복잡한 교정 작업을 줄이면서도 실시간으로 안정적인 제어를 유지하는 데 최적화되어 있습니다.


🔹 시스템 아키텍처
RGB 카메라 (RTSP)
↓
OpenVLA (행동 추론)
↓
2D 픽셀 변화량 (dx, dy)
↓
픽셀 → mm 캘리브레이션
↓
안전 클램프(제한) 및 스무딩(부드러운 움직임)
↓
UR 로봇 (RTDE servoL 제어)
🔹 데모
(데모 영상 이미지)
🔹 주요 제약 사항
뎁스(Depth) 카메라 없음
실시간 제어 필요
RTSP 지연 및 프레임 끊김 문제
비전 모델 출력값의 노이즈 발생
🔹 설계 결정 이유
왜 3D가 아닌 2D인가?
전체 3D 포즈 추정은 시스템 복잡도와 오류 발생원을 증가시킵니다. 테이블 위 작업(Tabletop manipulation)의 경우, 상대적인 2D 움직임만으로도 충분히 안정적인 제어가 가능했습니다.
왜 최소한의 캘리브레이션인가?
복잡한 핸드-아이(Hand-eye) 캘리브레이션 대신 단순한 2×2 선형 매핑 방식을 사용했습니다.
🔹 구현 주요 특징
OpenVLA 행동 추론: 7차원(7D) 데이터를 dx, dy로 변환
스레드 기반 RTSP 캡처: 프레임 지연 및 멈춤 방지
안전 클램프: 로봇의 갑작스러운 움직임 방지
RTDE servoL 제어 루프
🔹 주요 이슈 및 해결책 (실제 디버깅)
이슈	원인	해결 방법
RTSP 프레임 멈춤	OpenCV 버퍼 정체	grab/retrieve 방식 + 멀티스레드 적용
RTDE 타임아웃	네트워크 불안정	재연결 로직 구현
Torch + HPCX 충돌	libucc 심볼 불일치	LD_LIBRARY_PATH 설정 수정
➡️ 자세한 내용은 issues_and_fixes.md 파일 참조
🔹 한계 및 향후 과제
평면(XY) 운동만 지원
그리퍼 방향(Orientation) 제어 미구현
월드 모델(World-model) 기반의 단기 예측 기능 계획 중
🔹 기술 스택
언어/프레임워크: Python, PyTorch
AI 모델: OpenVLA
비전/통신: OpenCV, RTSP
로봇 제어: UR RTDE
OS: Linux
🔹 이 프로젝트가 보여주는 것
엔드 투 엔드(End-to-end) 시스템 통합 과정
실제 환경에서의 디버깅 및 제약 사항 해결 능력
제한된 센서 환경에서의 실용적인 의사결정 과정
