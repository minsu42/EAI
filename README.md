# EAI - 라즈베리파이 기반 유해조수 퇴치 시스템

임베디드 프로그래밍 수업 팀 프로젝트로 진행한 프로젝트입니다.
Raspberry Pi + Coral EdgeTPU 환경에서 YOLO 기반 실시간 객체 탐지를 수행하고,
유해조수(대상 동물)가 탐지되면 GPIO로 연결된 부저를 울려 퇴치하는 온디바이스 AI 시스템입니다.

> 팀 프로젝트입니다. 원본 레포: https://github.com/KimGeonWoo-p/EAI

## 시스템 구성

1. **모델 학습 & 경량화** — YOLOv5로 대상 객체 탐지 모델 학습 후, EdgeTPU에서 구동 가능하도록 fp16/int8 양자화 및 컴파일
2. **온디바이스 추론** — Raspberry Pi + Coral EdgeTPU 위에서 TensorFlow Lite로 실시간 추론
3. **후처리** — IoU 기반 NMS(Non-Max Suppression)로 중복 박스 제거
4. **경고 트리거** — 탐지 결과에 따라 wiringPi로 GPIO 부저 ON/OFF 제어

## 담당 부분 (Minsu Kang)

- **YOLOv5 모델 학습 및 경량화**: 데이터 선별(`data_select.ipynb`), 학습(`yolov5models/yolov5.ipynb`), fp16/int8 양자화 및 EdgeTPU 컴파일 (`yolov5models/`, `tpu_projects/`)
- **NMS 후처리 로직 구현**: IoU 계산 및 non_max_suppression 함수 (`warning/main.cc`)
- **GPIO 경고 시스템**: wiringPi 기반 부저 제어 로직 (`warning/warning.cc`, `warning/headers/warning.h`)

## 기술 스택

C++, TensorFlow Lite, OpenCV, Coral EdgeTPU, wiringPi, Python(YOLOv5), Jupyter Notebook

## 폴더 구조

- `warning/` — 최종 추론 + NMS + GPIO 경고 로직 (메인 결과물)
- `yolov5models/` — 모델 학습 및 경량화 산출물
- `main_project/`, `test_project/` — 프로젝트 진행 중 실험 코드
- `lab1/`, `hw5/`, `tpu_projects/` — 수업 실습 과제 (TFLite/EdgeTPU 기초 실습)
