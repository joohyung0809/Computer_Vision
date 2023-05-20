## sementic segmentation + object detection -> instance/panoptic segmentation
## landmark localization
---<br>

## Instance Segmentation 
#### Mask R-CNN
- ROIAlign이라는 Pooling layer 제안 - 소수점 픽셀 레벨의 풀링 지원(원래는 정수형 지원)
- ROIAlign이라는 pooling으로 Region CNN features 추출
- Region CNN features 추출 뒷 단에서는 Box offset regressor, softmax classifier가 있는데 추가로 Mask FCN predictor 추가
---<br>
#### YOLACT
- prototype - mask는 아니지만 mask를 합성해낼 수 있는 물체들의 softmax component를 생성
- 즉 mask를 합성할 수 있는 재료를 제공하는 형태로 진행됨
- 다음으로 prototype을 잘 합성할 수 있는 계수를 만들고 그 게수와 prototype을 선형결합함
- 그 이후 결합한 detection의 respone map 뽑아 합성(weighted sum)을 함
#### 즉, 이 모델은 나올 수 있는 모든 경우의 수의 마스크를 다 뽑는 게 아니고, 적절한 양만 뽑고 그것의 선형결합으로 다양한 마스크를 생각하는 것이 key point
---<br>
#### YolactEdge (YOLACT도 realtime하게 빠르지만 소형화 되어있지 않음)
- 특정 맵의 계산량을 줄이기 위해 feature pyramid에서 특정 layer를 가지고 옴
- 아직 부드럽게 동작은 안 됨
---<br>
## Panoptic Segmentation
#### 기존의 instance는 배경에는 관심이 없고 물체에만 관심이 있었음
#### 또 배경에 관심을 가지려면 semantic이 필요했는데 이는 객체 구별을 못 했음
#### 이를 해결하기 위해 panoptic이 나옴
---<br>
#### UPSNet
- FPN 구조로 고해상도의 feature map 뽑고
- Head를 semantic Head와, Instance Head로 나눔
    - semantic Head : semantic logits 추출
    - Instance Head : object detection과 mask logits 담당
#### VPSNet(for video)
1. Align reference features onto the target feature map(Fusion at pixel level)
2. Track module associates different object instances (Track at instance level)
3. Fused-and-tracked modules are trained to synergize each other