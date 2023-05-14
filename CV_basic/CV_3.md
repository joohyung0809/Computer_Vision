## Problems with deeper layers
- Vanishing gradient 문제
## CNN architectures for image classification 2
1. GoogLeNet - inception module
- 한 층에서 1x1, 3x3, 5x5 convolution, 3x3 max pooling 사용하는데 계산 복잡도 증가 우려
- 3x3, 5x5을 적용하기 전에 1x1로 dimension 줄여주어 복잡도 줄이는 트릭 사용, 3x3 max pooling 하고 1x1 conv 넣어서 dimension 줄임
- GoogLeNet은 Vanishing gradient 문제를 해결하기 위해 Auxiliary classifiers를 사용
    - 중간 중간 gradient를 주사기처럼 꽂아주는 느낌
- output은 마지막에 single FC layer로

2. ResNet - Degradation problem 존재
- layer 많이 쌓았음에도 성능이 안 좋았음 -> optimiztion 문제였을 것.
- layers get deeper -> hard to learn good output(H(x)) directly. So we learn residual func (분할정복 아이디어)
    - 위를 구현하기 위해 shortcut connection solution 나옴
    - shortcut을 통해 Vanishing gradient 문제 해결 가능
    - gradiant backpropagation 할 때 지나가는 경우의 수가 2의 n제곱 -> n개의 residual block
- 구성: 모든 residual block은 2개의 3x3 conv layer 가짐 -> 상대적으로 연산 빠름, block 구조 나누어져 있는데 한 단계가 지나갈 때마다 공간은 2배씩 줄고, 채널 수는 2배씩 늘어남, 최종 출력은 하나의 FC layer
