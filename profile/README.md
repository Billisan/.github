<p align="center">
  <img src="https://github.com/Billisan/Billisan/raw/main/assets/kiosk-main.png" alt="빌리산 키오스크 메인 화면" width="720" />
</p>

<h1 align="center">빌리산 · Billisan</h1>

<p align="center"><b>얼굴 인증과 AI 파손 검수로 우산을 빌리고 반납하는 스마트 우산 공유 서비스</b></p>

갑작스러운 비에도 가까운 대여소에서 우산을 빌리고 반납할 수 있도록 돕는 서비스입니다.
키오스크에서 얼굴로 본인을 확인해 우산을 대여·반납하고, 반납한 우산은 AI가 촬영 이미지로 파손 여부를 판정합니다.
관리자는 웹에서 대여소별 재고와 검수 결과를 확인하고 최종 판정을 내립니다.

## 주요 기능

- **얼굴 인증 대여·반납**: 키오스크 카메라로 본인을 확인하고, 이용 자격을 검증한 뒤 슬롯 잠금을 해제
- **AI 우산 파손 검수**: 반납한 우산을 촬영해 YOLO로 검출하고 PatchCore로 파손 의심 여부를 판정
- **관리자 웹**: 캠퍼스 지도 기반 재고 분포, 슬롯 상태 관리, AI 검수 결과에 대한 관리자 판정
- **사용자 앱**: 대여 현황·이용 내역·미정산 조회, 대여소 지도, 서비스 안내 챗봇

## 저장소 구성

모든 코드는 [`Billisan`](https://github.com/Billisan/Billisan) 저장소 하나에 있습니다.

| 경로 | 구성 요소 | 기술 |
| --- | --- | --- |
| [`backend/`](https://github.com/Billisan/Billisan/tree/main/backend) | 인증, 대여·반납 상태, 재고·검수 API, MQTT, 챗봇 | Java 21, Spring Boot, JPA, Flyway, MySQL |
| [`frontend/web/`](https://github.com/Billisan/Billisan/tree/main/frontend/web) | 관리자 웹 | React, TypeScript, Vite, TanStack Query |
| [`frontend/kiosk/`](https://github.com/Billisan/Billisan/tree/main/frontend/kiosk) | 대여·반납 키오스크 | React, TypeScript, Vite, PWA |
| [`frontend/app/`](https://github.com/Billisan/Billisan/tree/main/frontend/app) | 사용자 앱 | React Native, Expo, TypeScript |
| [`iot/pi/`](https://github.com/Billisan/Billisan/tree/main/iot/pi) | 장치 제어, 키오스크 WebSocket, 백엔드 MQTT 연동 | Raspberry Pi 5, Python, Flask, paho-mqtt |
| [`iot/orin/`](https://github.com/Billisan/Billisan/tree/main/iot/orin) | 카메라, 얼굴 인증, 우산 검수 추론 | Jetson Orin Nano, InsightFace, pgvector |
| [`ai/anomaly_detection/`](https://github.com/Billisan/Billisan/tree/main/ai/anomaly_detection) | 우산 검출·파손 판정 모델 학습과 추론 | YOLO, DINOv2, PatchCore, ONNX |

## 시스템 구성

<p align="center">
  <img src="https://github.com/Billisan/Billisan/raw/main/assets/system-architecture.png" alt="빌리산 시스템 아키텍처" width="720" />
</p>

키오스크와 Raspberry Pi는 WebSocket, Pi와 백엔드는 MQTT, Pi와 Jetson은 ZeroMQ로 통신합니다.
얼굴 원본 이미지는 저장하지 않으며, 얼굴 임베딩은 Jetson 로컬 데이터베이스에만 보관합니다.
