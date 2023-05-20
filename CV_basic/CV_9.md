## Landmark localiztion
#### 얼굴, 사람의 포즈 tracking
- 특정 물체에서 중요하다고 생각되는 부분을 정의하고 추정하고 추적
#### Coordinate regression
- 각 point의 x, y 위치의 2 dimension으로 regression 함
- 부정확하고 일반화에 문제
#### Heatmap classification
- 각 채널들이 각 keypoint가 되고, 각 keypoint가 클래스가 됨
- keypoint들이 확률값으로 표현됨
- 성능이 좋지만 모든 픽셀을 다 판별해야돼서 계산량 많음
---<br>
## LandMark location to Gaussian heatmap
#### landmark가 x, y로 찍히면 그걸 히트맵으로 바꿔줘야 함
- 가우시안 분포를 사용하면 됨(식이 있음)
#### 반대로 heatmap형태로 나온 것을 x,y 형태로 바꿔줄 수도 있어야 함
- ???
---<br>
## Hourglass network(U-Net과 비슷)
- 영상을 작게 만들어서 receptive field를 키움 -> 전반적인 큰 그림을 봄
- 그리고 low-level feature 참고해서 object위치를 정확하게 찾을 수 있게 했음
- 얘를 반복시킴 -> 개선해나감
- Stacked hourglass modules allow for repeated bottom-up and top-down inference that refines the output of the previous hourglass module
#### U-Net과 비슷하지만 다른 점이 있음
- Concatenation 하는 U-Net과 달리 Hourglass는 +연산을 함 -> dimension이 U-Net처럼 늘지 않음
- 대신 skip할 때 U-Net은 바로 마지막 단계로 skip했는데, Hourglass는 또 다른 conv를 거쳐서 skip함
---<br>
## DensePose
#### 특정 랜드마크에 집중하지 않고, object 신체 모든 부위의 landmark 고려
- 3D surface로 표현 -> UV맵
- 3D mesh와 UV맵의 관계가 움직여도 변하지 않음 -> UV맵에서는 고정되어 있지만 3D mesh가 움직여도 어디에 위치하는지 알 수 있음
- texture와 color를 입히는 건 -> UV맵 자체에 texture or color 코딩하는 것
- DensePose R-CNN = Faster R-CNN + 3D surface regression branch
    - mask를 UV 맵으로 대체했다고 생각하면 좋음
---<br>
## RetinaFace = FPN + Multi-task branches(classification, bounding box, 5 point regression, mesh regression)
- Multi-task branches -> 여러가지 regression(학습법) 결합
    - 각 task마다 공통적으로 오는 특징을 통해 backbone network가 강하게 학습
    - 한 번 업데이트 될 때 많은 task를 본 것 같은 효과, multi-task로 학습 잘 된 효과
## FPN + Target-task branches 중요
- 본인이 원하는 task에 맞는 기법 잘 조합하는 것.