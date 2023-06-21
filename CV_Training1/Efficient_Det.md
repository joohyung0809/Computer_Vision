### EfficientNet
- compound scaling method
    - model의 depth, width, resolution을 균형있게 늘려 성능을 올림
---<br>
### EfficientNet
- 각 feature map의 단순합이 아닌 가중합을 하기 위해 BiFPN를 제안
- 모델의 Efficiency를 향상시키기 위해 cross-scale connections 방법을 이용
---
- 하나의 간선을 가진 노드 제거
- Output 노드에 input 노드 간선 추가
- 양방향 path 각각을 하나의 feature layer로 취급하여 repeated blocks 사용