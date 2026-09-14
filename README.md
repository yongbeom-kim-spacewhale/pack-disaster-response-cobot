# P.A.C.K. — 재난방송 기반 비상물품 준비 협동로봇

재난 유형에 필요한 비상물품을 인식하고 Doosan M0609가 파지·이동·배치하도록 구성한 ROS 2 협동로봇 프로젝트입니다.

[시연 영상](https://drive.google.com/file/d/1z7fK1n83B9fg4f2cSOuTmltv9qYJgL5V/view) · [원본 팀 저장소](https://github.com/Chan-Hyeok-Kim-git/cobot2-P.A.C.K.)

## 프로젝트 정보

- 기간: 2026.07.20–2026.07.29
- 인원: 5명
- 담당: 주제·초기 방향 제안, AI 비전 모델, RGB-D Point Cloud, 현장 검증, AI 발표
- 환경: ROS 2 Humble, Doosan M0609, OnRobot RG2, Intel RealSense D435I

## 담당 구현

- 11종 비상물품 instance segmentation 데이터셋과 모델 개발
- 기존 497장·3,557개 BBox를 Mask로 전환하고 Roboflow에서 수동 검수
- 취약 클래스와 빈 배경을 보강해 Dataset v2 633장·3,623 instances 구성
- Detection+MobileSAM 2단계를 단일 YOLO Segmentation 구조로 전환
- Mask와 aligned Depth를 3D로 투영해 객체·배경 PointCloud2, JSON과 Marker 발행
- 36개 실제 조건 장면을 촬영해 방향, 가장자리, 투명 물체와 빠른 움직임 검증

## 검증 결과

- 동일 test의 Mask mAP50-95: `0.8851 → 0.9220`
- 동일 89.831초 RGB-D bag의 AI worker 평균 처리시간: `88.98ms → 39.22ms` (55.9% 감소)
- 현장검증 36개 장면 중 33개에서 목표 클래스가 한 프레임 이상 검출
- 11종 물품별 접근·파지·이동·놓기를 최소 1회 완료
- 호루라기, 로프, 보호 마스크, 작업 장갑 4종을 안정적인 최종 시연 대상으로 선정

`33/36`은 장면 단위 검출 여부이며 프레임 단위 정확도가 아닙니다. 처리시간도 AI worker 내부 측정값이며 전체 시스템 지연을 뜻하지 않습니다.

## 배운 점

오프라인 지표만으로 실제 운용 성능을 판단할 수 없었습니다. 노이즈, 빈 배경, 다양한 방향과 여러 물체가 함께 있는 장면을 조건별로 설계하고, 현장 실패 사례를 다음 수집·학습 주기에 반영해야 한다는 점을 배웠습니다.
