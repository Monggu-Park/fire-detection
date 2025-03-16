# 고도화된 화재 및 연기 탐지를 위한 YOLOv9 실시간 성능 최적화 연구 

### Real-Time Performance Optimization of YOLOv9 for Advanced Fire and Smoke Detection

-------

Implementation of paper(Korean, 한국어) - https://drive.google.com/file/d/1tUDc6iFb-1lY77JKCxAELDSp_xPL_9n4/view?usp=sharing

동국대학교 2024학년 2학기 개별연구 프로젝트로 객체 탐지 모델인 YOLOv9의 일부 구조를 추가하거나 변경하여 화재와 연기 이미지에서 화재와 연기에 대한 평균 정밀도(AP)를 향상시키는 것을 목표로 한다.



## 🧑‍🤝‍🧑 팀원(Team)

------

박규영([**Monggu-Park**](https://github.com/Monggu-Park)) - YOLOv9에 WIoU Loss 적용, git 관리

김상현([**rlatkdgus2627**](https://github.com/rlatkdgus2627)) - YOLOv9에 CA, CARAFE, ODConv 적용, dataset 준비, 전체 모델 AP, fps 측정



## 🕰️ 프로젝트 기간

-------

2024.09.04 ~ 2024.12.18



## 🖥️ 사용 및 참고한 소스코드

------

YOLOv9 - https://github.com/WongKinYiu/yolov9

Coordinate Attention(CA) Module - https://github.com/houqb/CoordAttention

CARAFE - https://github.com/XiaLiPKU/CARAFE

ODConv - https://github.com/OSVAI/ODConv

## 📚 연구 방법

-----

YOLOv9에 Coordinate Attention Module, CARAFE, ODConv를 적용하여 각각 훈련을 진행하고 AP를 측정한 다음, 가장 AP가 높은 모델을 선택하여 WIoU를 적용 후 훈련을 진행하여 WIoU Loss가 화재 및 연기 탐지 성능을 증가시키는 지 확인

각 브랜치 명은 YOLOv9에 어떤 점이 변경되어 있는 지를 나타냄

ex) 

yolov9-CA-ODConv ➡️ YOLOv9에 CA, ODConv 적용

test/yolov9-CA-ODConv ➡️ YOLOv9에 CA, ODConv, **WIoU **적용



```
HOME = "디렉터리 경로 지정 필요"
```

모델 다운로드

```
git clone https://github.com/Monggu-Park/fire-detection.git
```

필요 패키지 다운로드

```
pip install -r requirements.txt
```

Pillow 버전 변경(필수)

```
pip install pillow==9.5.0
```

훈련 진행

```
python train.py \
--batch 8 --epochs 50 --img 640 --device 0 --min-items 0 --close-mosaic 15 \
--data {HOME}/yolov9/data.yaml \
--weights '' \
--cfg models/detect/yolov9-c.yaml \
--hyp hyp.scratch-high.yaml
```

모델 테스트

```
python val.py --img 640 --batch 32 --conf 0.001 --iou 0.7 --device 0 --data {HOME}/yolov9/data.yaml --save-txt --task 'test' --weights {HOME}/yolov9/runs/train/exp/weights/best.pt
```



## 📜연구 결과

----

YOLOv9에 CA + ODConv을 적용한 모델이 가장 성능이 좋은 것을 알 수 있다. (성능 측정 결과는 논문 참고)
