## Object Detection(classification과 boundingBox를 동시에 추정)
#### instance segmentation과 panoptic segmentation은 Semantic segmentation과 다르게 객체 각각을 detecting 할 수 있다.
- 참고로 panoptic segmentation은 instance segmentation을 포함하는 개념

## 최근 two stage/single stage detector로 발전

#### 과거
- gradient based detector: gradient를 이용하여 경계선을 찾고, 그 경계선의 방향, 위치, 각도에 따라 학습
- selective search
1. over segmentation: 비슷한 색끼리 분할 
2. 분할한 것들을 비슷한 영역끼리 합쳐주는 과정을 반복
3. 이때 비슷한 영역을 많이 포함한 부분을 박스 후보로 추출할 수 있음 


## 딥 러닝 기반의 detecting 시작
## R-CNN
1. Input image
2. Extract region proposals (selective search같은 걸로)
3. Compute CNN features(image classification network)(미리 학습 되어 있음)의 input에 적절한 size로 warping함
---<br>
## Fast R-CNN
- 영상 전체에 대한 feature를 추출하고, 이를 재활용하여 여러 object들을 detection함
1. original image에서 conv feature map 추출 - 이때 fully conv 구조에서는 입력 size에 상관없이 feature map추출 가능
2. RoI feature가 추출 Rol Polling을 통한 feature map으로부터
    - fixed dimension을 가질 수 있도록 resampling을 하는 구간 : RoI Polling Layer
3. class와 boundingBox를 정확하게 추출하기 위해 boundingbox regression, classification 수행
---
## Faster R-CNN
- 그럼에도 성능이 높아지지 않아 region proposal을 neural networ로 대체(RPN 제안)
- keyword: 
    - IoU: Intersection over Union -> 두 영역이 잘 조합되면 수치가 높아짐
    - Anchor boxes: 각 위치에서 발생할 것 같은 box의 후보군
- Anchor box들이 일정 수치가 넘어가면 positive sample, 아니면 negative sample
### RPN도입하면 
1. conv layer에서 feature map 뽑고
2. sliding window를 통해 anchor box 적용시켜봄
    - 이때가 intermediate layer로 256-dimension 중 하나를 뽑아서 쓰는 거임
3. 다음으로 cls layer(2k scores)와, reg layer(4k coordinates)로 나뉨(k=anchor 개수)
    - 2k인 이유 : object vs non-object
    - 4k인 이유 : x,y,width,height
---<br>
#### 근데 엄청나게 많은 box가 생기게 됨 -> Non-Maximum Suppression(NMS) 사용
- bounding Box를 filtering 할 수 있음
---<br><br>
#### One stage는 정확도를 포기하더라도 real time detecting을 목적으로 함
- ROI pooling 없이 바로 box regression과 classification 진행 -> 시간 단축
- EX: YOLO
    - YOLO architecture에서 마지막 layer의 conv 형태는 아래와 같음
    (마지막 layer의 해상도),(5*anchor 수 + detecting할 class 개수)
---<br>
### YOLO의 아쉬운 성능으로 Single Shot MultiBox Detector(SSD)나옴
- The use of multi-scale outputs attached to multiple feature maps enable effectively modeling a diverse space of possible box shapes -> 다양한 feature map을 통해 나오는 box를 고려할 수 있게 되었다
- 각 feature map마다 anchor box를 다르게 사용하기 때문에 anchor box의 개수를 마지막에 구할 수 있음

---<br>
## Two Stage VS One Stage
- One Stage 문제점: class imbalanc 문제 발생 -> 의미 없는 곳에서 샘플링 많이 되고, 의미 있는 곳에서 샘플링 덜 됨
- 왜냐하면 모든 곳에서 gradient가 오기 때문에 
- Focal Loss로 해결 가능 -> 오답이라고 생각할 수록 그래프가 sharp하게 변해서 gradient가 커짐

## RetinaNet
- Feature Pyramid Networks(FPN) + class/box prediction branches
- class head와 box head가 따로 구성

## object Transformer (DETR)
- encoder-decoder를 거쳐 object queries를 이용하여 transformer에 대해 질의를 하는 것
- object query는 learning positional encoding
- 각 토큰에 질의를 함
- 질의할 때 single image에 최대 몇 개의 object가 존재하는지 정해줘야 함
