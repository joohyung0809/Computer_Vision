## After ResNets
## DenseNet
#### 각 layer를 지나올 때 이전의 layer의 출력만 가져오는 것이 아니라 그 전의 layer 출력도 가져옴
- 상위 layer도 하위 layer의 정보를 재참조 -> 학습이 용이
- concatenated along the channel axis (+는 두 가지 신호를 합치는 것, concatenated은 하위 값을 보존 but channel은 증가)

## SENET
#### 현재 주어진 activation의 관계가 더 명확해질 수 있도록 채널 간의 관계를 모델링.
- 중요도를 파악해서 attention 파악, attention 생성 방법은 아래
    1. Squeeze: capturing distribution of channel-wise responses by global average pooling
    2. Excitation: gating channels by channel-wise attention weights obtained by FC layer

## EfficientNet
#### Building deep, wide and high resolution networks in an efficient way
- width scaling: channel 늘리기 -> GoogleNet에서 사용
- depth scaling: layer 늘리기 -> ResNet
- resolution scaling: input image의 resolution 큰 것을 넣기
#### 위 세 개는 늘리면 성능 향상이 무뎌지는데 이 증가폭이 다름 -> 적당한 정도를 적당한 시기에 scaling하면 성능이 오를 것이다.
- compound scaling: 위 세 개를 적절히 scaling한 방법
### EfficientNet은 적은 FLOPS로 좋은 효율을 냄

## Deformable Convolution
#### 2D spatial offset prediction for irregular convolution
- 기존의 conv가 사각형을 찍는 것과 다르게 이 모델의 convolution은 물체를 따라 찍음

## SUMMARY
- GoogLeNet is the most efficient CNN model out of {AlexNet, VGG, ResNet}
- But it's complicated use
#### So, VGG, ResNet are typically used as backbone model for many tasks
- backbone은 feature map을 사용할 때 쓰는 모댈
- Powerful and Simple
