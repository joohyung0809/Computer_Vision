### 2 Stage의 한계로 인해 1 Stage 생각해보게 됨
- RCNN, Fastrcnn, SPPNet, FasterRCNN -> Localization, Classification
- 속도가 매우 느리다는 한계가 있음
- Real World에서 응용
--- <br>
### 1 Stage는 feature map에서 RPN과정 없이 객체 뽑고, 위치 뽑는 것
- Localization, Classification 동시 진행
- 전체 이미지에 대해 특징 추출, 객체 검출이 이루어짐 -> 간단하고 쉬운 디자인
- 속도가 매우 빠름 (Real time)
- 영역을 추출하지 않고 전체 이미지를 보기 때문에 객체에 대한 맥락적 이해 높음 -> background error 낮아
- YOLO, SSD, RetinaNet, etc .. 
---
### YOLO v1
- 이 방법은 한 grid cell에 두 개의 box 정보, 클래스 개수가 들어가서 S x S x 30이 생긴다
- 한 grid cell당 2개의 bboxes가 나오고(사진 객체 2개라는 가정하에) 총 98개가 생김 (7x7이라는 가정하에)
- 그리고 threshold를 통해 걸러줄 거 걸러준 후 내림차순 정렬을 하면 score가 높은 순으로 정렬
- NMS algorithm 하고 남아있는 box를 object에 그려줌
- 이 모델의 Loss는 Localiztion + Confidence + Classification Loss의 합이다.
- Fast R-CNN과 YOLO ensemble하면 궁합 좋음
---<br>
### YOLO는 7x7 그리드 영역으로 나누었기에 그리드보다 작은 크기의 물체 검출 불가, 신경망을 통과하며 마지막 feature만 사용하기에 정확도 하락
#### 따라서 SSD 나오게 되었음
- SSD는 FC_layer 없앴음 대신 convolution layer 사용
- Extra convolution layers에 나온 feature map들 모두 detection 수행
    - 6개의 서로 다른 scale의 feature map 사용
    - 큰 feature map (early stage feature map)에서는 작은 물체 탐지
    - 작은 feature map(late stage feature map)에서는 큰 물체 탐지
- Default box 사용(anchor box)
    - 서로 다른 scale과 비율을 가진 미리 계산된 box 사용
- 큰 feature map은 작은 물체 탐지, 작은 feature map은 큰 물체 탐지
