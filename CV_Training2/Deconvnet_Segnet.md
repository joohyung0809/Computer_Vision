### DeconvNet
- Decoder를 Encoder와 대칭으로 만든 형태
- Unpooling과정 도입 -> 경계에 집중
    - Pooling의 경우 노이즈를 제거하는 장점이 있지만, 그 과정에서 정보가 손실되는 문제가 생김
    - Unpooling을 통해서 Pooling시에 지워진 경계의 정보를 기록했다가 복원
    - 학습이 필요 없어 속도가 빠름
    - 하지만 sparse(희소 행렬 특성)한 activation map을 가지기 때문에 이를 채워 줄 필요가 있음
        - 채우는 역할을 Transposed Convolution이 수행 -> 이미지 내부에 집중
        - 참고로 resolution의 변화는 주지 못함
- DeconvNet은 Conv-BatchNorm-ReLU / TransposedConv-BatchNorm-ReLU 반복형태
---
### Transpose convolution
- Low layer에서는 직선 및 곡선, 색상 등의 낮은 수준의 특징(local feature)
    - 전반적인 특징(location, shape, region, color)
- High layer에서는 복잡하고 포괄적인 개체 정보가 활성화(global feature)
    - 복잡한 패턴
- Deconvolution Network의 Activation map을 보면 층과 pooling 방법에 따라 다른 특징이 있음
---<br>
### SegNet(real time segmentation의 욕구)
- DeconvNet에 있던 가운데 7x7 Conv - 1x1 Conv - 7x7 Deconv 구조가 없음(Deconv와 차이)
- Transposed Conv를 UnPooling으로 대체하여 학습 필요를 없앰(Deconv와 공통)
- sparse matrix를 dense하게 만들어줄 때 Conv이용(Deconv와 차이)
