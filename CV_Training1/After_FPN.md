### DetectoRS
- Looking and thinking twice
    - RPN
    - Cascade R-CNN
---<br>

### Recursive Feature Pyramid 제안
- top-down, bottom-up을 반복하는 과정 -> 시간 굉장히 오래 걸림
- Atrous Convolution을 사용
    - receptive field를 크게하기 위해 사용
    - 방법은 기존 Convolution은 연속적으로 하지만 Atrous는 한 칸 띄어서 필터링
---<br>

### Bi-directional Feature Pyramid (BiFPN)
- PANet의 필요없는 노드를 줄여 효율성 높임
- FPN과 같이 단순 summation하는 것은 적합하지 않아 보임(Weighted Feature Fusion)
    - 각 feature별로 가중치를 부여한 뒤 summation해야 함
    - 모델 사이즈의 증가는 거의 없음
    - feature별 가중치를 통해 중요한 feature를 강조하여 성능 상승
---<br>

### NASFPN
- COCO dataset, ResNet기준으로 찾은 architecture, 범용적이지 못함 -> parameter 많아짐
- High search cost -> 다른 dataset이나 backbone에서 가장 좋은 성능을 내는 architecture를 찾기 위해 새로운 search cost
---<br>

### AugFPN
- FPN에서 문제를 지적
    - 서로 다른 level의 feature간의 semantic차이
    - Highest feature map의 정보 손실(가장 높은 level은 위에서 받을 정보가 없으니까)
    - 1개의 feature map에서 RoI 생성
--- <br>
- 해결하기 위해 Consistent Supervision, Residual Feature Augmentation, Soft RoI Selection
- Residual Feature Augmentation
    - 마지막 stage에 highest level은 정보 손실이 있기에 임의의 feature 맵을 만들어 보완
- 순서
    - Ratio-invariant Adaptive Pooling(다양한 scale의 feature map생성, 256_channels)
    - Adaptive Spatial Fusion 진행(이해 어려운 부분)
        - Fusion 진행하려면 각 map들의 크기를 맞춰주기 위해 upsample 해주어야 함
        - 이때 단순 summation이 아니라 가중치를 부여하여 summation
        - 3개의 feature map을 Concat - Conv 1x1 - Conv 3x3 - Sigmoid(중요도 측정하기 위함)
        - sigmoid를 통해 나온 값을 이용하여 가중합을 진행하는 과정임
    - FPN에서 RoI를 매핑해준 것과 다르게 여기서는 모든 feature map으로부터 RoI projection 진행
        - FPN에서는 결국 다 구해놓고 하나만 쓴 것과 다를 게 없다는 인식을 함
        - 여기선 Soft RoI Selection 진행
            - Channel-wise 가중치 계산 후 가중합을 사용
            - PANet의 max pooling을 학습 가능한 가중합으로 대체