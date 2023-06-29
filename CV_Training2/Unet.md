### U-net의 두 가지 한계점을 극복하기 위해 아래와 같은 방법을 고안 -> U-Net++
- Encoder를 공유하는 다양한 깊이의 U-Net을 생성
    - depth=1, 2, ...
- Skip connection을 동일한 깊이에서의 Feature Maps가 모두 결합되도록 유연한 Feature map 생성
    - 기존에는 depth n의 노드와 depth n+1의 반대쪽 노드를 concat하고 conv하는 방식이었음
    - U-Net++에서는 같은 depth의 모든 노드를 concat해주는 것(좀 더 dense함)
### U-Net++의 skill
- Hybrid loss 사용
    - Pixel wise cross entropy + soft dice coefficient
- Deep Supervision
    - x(0,1) x(0,2) x(0,3) x(0,4)의 loss구해서 평균낸 것을 loss로 구함
### 한계점
- 복잡한 connection으로 parametet 증가
- 많은 connection으로 인한 memory 증가
- Enconer-Decoder 사이에서의 connection이 동일한 크기를 갖는 feature map에서만 진행
    - 즉, full scale에서 충분한 정보를 탐색하지 못해 위치와 경계를 명시적으로 학습하지 못함
---<br>
### U-Net과 U-Net++의 한계
- U-Net에서의 decoder를 구성하는 방법은 같은 level의 encoder layer로 부터 feature map을 받는 simple skip connection 사용
- U- Net++에서는 nested and dense skip connection을 사용하여 encoder-decoder 사이의 semantic gap 줄임
    - 하지만 한계들이 있음
- 위와 같은 한계로 U-Net 3+ 등장
---
### U-Net 3+
- Full-scale skip connections(Conventional + inter + intra) skip connection
- decoder의 feature map 구성 방법
    - encoder layer로부터 same-scale의 feature maps 받음(conventional)
    - encoder layer로부터 smaller-scale의 low-level feature maps받음(inter)
        - 풍부한 공간 정보를 통해 경계 강조
    - decoder layer로부터 larger-scale의 high-level feature maps 받음(intra)
        - 어디에 위치하는지 위치 정보 구현
---
- parameter 수를 줄이기 위해 모든 decoder layer의 channel 수 320으로 통일시킴
    - 모든 encoder llayer에서 skip connection과정에서 64channel, 3x3 conv를 동일하게 적용(64x5=320)
---
### U-Net 3+에 적용된 techniques
- classification-guided module(CGM)
    - low-level layer에 남아있는 background의 noise발생하여, 많은 false-positive 문제 발생
        - 정확도를 높이고자, extra classification task 진행
- Full-scale Deep Supervision(loss function) - hybrid loss 사용
    - Focal loss(클래스의 불균형 해소)
    - ms-ssim Loss(Boundary 인식 강화)
    - IoU(픽셀 분류 정확도 향상)
---
- high level feature maps 사용
    - Dropout, 1x1 Conv, AdaptiveMaxPool, Sigmoid 통과하여 확률값 내고
    - Argmax를 통해 Organ이 없으면 0, 있으면 1 출력
    - 0 혹은 1 결과와 각 low-layer마다 나온 결과값을 곱한다.
    - 이를 통해 classification을 extra로 한 번 더 해서 잘못 분류한 것을 삭제