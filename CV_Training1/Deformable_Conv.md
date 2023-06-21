### CNN 문제점
- 일정한 패턴을 지닌 cnn는 geometric transformations에 한계를 지님
    - geometric transformations이란 이미지의 기하학적 변화
    - 해결법: augmentation, invariant feature engineering -> but 사람이 인지하는 변화에 초점
### deformable convolution 제안
- 기존 conv 연산 방식에서 offset을 지정해주는 행렬 R을 만들어서 이를 뒤바꾸며 연산시킴
---
### 정리하면
- 일정한 패터을 지닌 cnn는 geometric transformations에 한계를 파악
- 일정한 패턴이 아니라 offset을 학습시켜 위치를 유동적으로 변화
- 주로 object detection, segmentation에서 좋은 결과를 보임
