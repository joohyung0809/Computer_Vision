### DeepLab v2
- FC6 ~ FC8에서 총 네 가지의 서로 다른 Dilated conv 가지들이 뻗어나감 -> ASSP(Atrous Spatial Pyramid Pooling)
- 각자만의 정보를 추출하여 마지막에 이들간의 sum을 취한 후 여기에 upsamling을 한다.
    - rate가 낮은 블록: 작은 object
    - rate가 높은 블록: 큰 object

- backbone을 ResNet-101모델을 활용
- 원래 conv4, conv5에서 down sampling을 진행했는데 DeepLab v2에서는 stride 1을 적용하여 down smapling 안 함
- 또 3x3 conv 대신, 3x3 Dilated conv 사용하여 큰 receptive field 얻을 수 있도록 함.

---<br>
### FCN문제점
1. Mismatched Relationship: 모델이 객체들간의 잠재적 관계를 잘 파악하지 못한다는 뜻입니다. 예를 들어 호수 위에서 자동차가 가는건 불가능한 일인데 마치 호수 위에서 자동차가 가는 것처럼 모델이 segmentation을 수행하는 경우가 있을 수 있습니다.
2. Confusion Categories: 비슷한 카테고리에 속하는 두 클래스를 서로 혼동하는 경우입니다. 사실 모델은 지엽적인 정보만 봐서는 비슷한 오브젝트를 분류하기 힘들 것입니다.
3. Inconspicuous Classes: 크기가 작거나 주변 오브젝트와 모양이 겹치는 등의 이유로 잘 안보이는 객체를 모델은 구분해내지 못한다는 뜻입니다.
### PSPNet
- FCN의 Max Pooling이 가진 실제 receptive field는 우리의 예상보다 작음
- Global Average Pooling 도입함
    - 일반적인 average pooling과 달리 feature map 하나를 1x1이라는 하나의 정보로 압축
    - 이는 모든 정보를 요약한 하나의 픽셀을 만드는 효과 -> 전체 영역에 대한 대표 값

### DeepLab v3+(Encoder-Decoder 구조)
- Encoder에서 spatial dimension의 축소로 인해 손실된 정보를 Decoder에서 점진적으로 복원
- skip connection도 이용
---
### Backbone으로 Xception 사용
- Xception은 Depthwise Seperable Convolution 사용
- input의 각 채널마다 다른 filter를 사용하여 convolution 연산을 별개로 수행한 후 이를 결합(depthwise연산)
- 이후 결합된 텐서에 1x1 Convolution을 적용하여 채널 수를 축소(pointwise)
---
### DeepLab v3+의 Entry에서
- 기존 Max Pooling부분이 Sep Conv 연산으로 변경
- stride를 2로 주어 크기 자체는 똑같이 변형하되, 학습 가능한 parameter가 들어간 Sep Conv를 이용하여 모델이 스스로 어디를 중요하게 생각하고, 어떻게 추출하는 게 정보의 손실을 방지할지 판단할 수 있도록 함.
### Middle에서는
- repeated 횟수를 늘려 깊은 구조 사용
### Exit 에서는
- Max Pooling을 Sep Conv로 대체
- 채널 수도 변경