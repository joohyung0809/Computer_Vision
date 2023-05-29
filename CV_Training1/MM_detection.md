## Obkect Detection Library
#### MMDetection & Detectron2
--- <br>

- MMDetection은 전체 프레임워크를 모듈 단위로 분리해 관리할 수 있음, 많은 프레임워크를 지원함, 다른 라이브러리에 비해 빠름
    - 지원 모델로는 Fast R-CNN, SSD, YOLO v3, DETR etc
- Detectron2는 전체 프레임워크를 모듈 단위로 분리해 관리할 수 있음, OD외에도 segmentation, Pose prediction 등의 알고리즘 지원
    - 지원 모델로는 R-CNN, RetinaNet, Mask R-CNN, DETR etc

---

## MMdetection
#### object detection에 가까운 pipeline 갖고 있음 <br> (input - Backbone - Neck - Dense Prediction - Prediction)
#### MMdetection에서 가장 중요한 건 config 파일을 얼마나 이해하고 변경할 수 있는지

- 2 stage 모델은 크게 Backbone / Neck / DenseHead / RoIHead 모듈로 나눌 수 있음
    - backbone - 입력 이미지를 feature map으로 변형
    - Neck - backbone과 head를 연결, feature map을 재구성(ex. FPN)
    - DenseHead - 특징 맵의 dense location을 수행 - localization
    - RoIHead - RoI 특징을 입력으로 받아 box 분류, 좌표 회귀 등을 예측하는 부분
- 각각의 모듈 단위로 커스터마이징
- 이런 시스템은 config 파일을 이용하여 통제됨 
#### Config file
- configs를 통해 데이터셋부터 모델, scheduler, optimizer 정의 가능
- 특히, configs에는 다양한 object detection 모델들의 config 파일들이 정의돼 있음
- 그중, configs/base/ 폴더에 가장 기본이 되는 config 파일이 존재
    - dataset, model, schedule, default_runtime 4가지 기본 구성요소 존재
- 각각의 base/ 폴더에 여러 버전의 config들이 담겨 있음
    - Dataset - COCO, VOC, Cityscape etc
    - Model - faster_rcnn, retinanet, rpn etc
---
- 중요한 건 configs/_base_/로 상속을 받고 우리가 바꾸고 싶은 부분만 바꿀 수 있음
    - 즉, 틀이 갖춰진 config를 상속받고, 필요한 부분만 수정해 사용함
---
#### Config 에서 모델의 파라미터를 살펴봄
- type
    - 모델 유형
    - Ex, FasterRCNN, RetinaNet etc
- backbone
    - ResNet, ResNext, HRNet etc
- Neck
    - FPN, NAS_FPN, PAFPN etc
- etc

#### 새로운 backbone을 등록할 수도 있음
#### runtime setting을 설정할 수 있음(재정의도 됨)
- optimizer
- Training schedules