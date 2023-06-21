### YOLO v2
- 정확도 향상, 속도 향상, 더 많은 class 예측
- fine-grained features for accurate
    - 크기가 작은 feature map은 low level 정보가 부족
    - early feature map은 작은 low level 정보 함축
    - early feature map을 late feature map에 합치는 passthrough layer 도입
    - 26 x 26 feature map을 분할 후 결합

- change Backbone for faster
    - 마지막 fc layer 제거 -> 3x3 conv layer 대체
    - 1x1 conv layer layer 추가 -> channel 수 125

- make WorkTree for Stronger
--- <br>

### 1 Stage의 문제점
- class imbalance (객체 영역 < 배경 영역)
- anchor box 대부분 background samples
- 2 stage detector의 경우 region proposal에서 background sample을 제거(selective search, RPN)
- Positive / negative sample 수 적절하게 유지(hard negative mining)
--- <br>
### 해결을 위해 RetinaNet
- 새로운  loss func: cross entropy loss + scaling factor
- 쉬운 예제에 작은 가중치, 어려운 예제에 큰 가중치
- 결과적으로 어려운 예제에 집중