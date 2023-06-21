### YOLO v4
- 하나의 GPU에서 훈련할 수 있는 빠르고 정확한 Object deterctor
- BOF, BOS 방법들을 실험을 통해 증명하고 조합을 찾음
    - BOF(Bag of Freebies): interface 비용을 늘리지 않고 정확도 향상시키는 방법
        - augmentation, CutMix, semantic distribution Bias(데이터셋에 특정 라벨이 많은 경우 불균형을 해결하기 위함), Label smoothing(라벨에 0또는 1로 설정하는 것이 아니라 smooth하게 부여), Bounding box Regression(Bounding box 좌표값들을 예측하는 방법은 거리가 일정하더라도 IoU가 다를 수 있음), GIoU(IoU가 0인 경우 차별화하여 loss 부여)  
    - BOS(Bag of Special): interface 비용을 조금 높이지만 정확도가 크게 향상하는 방법
        - Enhance receptiv field(SPP, ASPP, shift window etc..)
        - attention module(SE, CBAM) -> feature map에 global 정보를 추가 -> 채널의 중요도를 추가
        - feature integration (feature map 통합하기 위한 방법 like feature Pyramid Network)
        - Activation Function
        - Post-processing Method(불필요한 Bbox 제거)

- 고려사항
    - 작은 물체를 검출하기 위해 큰 네트워크 입력 사이즈 필요
    - 네트워크 입력 사이즈가 증가함으로써 큰 receptive field 필요 -> 많은 layer 필요
    - 하나의 이미지로 다양한 사이즈의 물체 검출하기 위해 모델의 용량이 더 커야 함 -> 많은 파라미터 필요

---
### YOLO v4 architecture
- darknet에 cross stage partial 적용
    - 기존 densenet 문제는 가중치 업데이트 할 때 gradient 정보가 재사용
---

### M2Det
### CornerNet