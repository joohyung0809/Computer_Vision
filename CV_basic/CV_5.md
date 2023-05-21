## Semantic Segmentation
- Image Classification은 사진이 주어졌을 때 사진 전체를 카테고리로 분류
- Semantic Segmentation은 사진이 주어졌을 때 사진 내 픽셀을 카테고리로 분류하는 task
- 하나의 픽셀이 어디 카테고리에 속하는지를 전부 파악

## what is Semantic Segmentation?
- 영상 속에 마스크를 생성, 이때 같은 클래스이지만 서로 다른 물체면 구분하지 않음
- user와의 interface를 제공하기 좋음
- computational photography에도 사용하기 좋음

####  Convolutional Networks(FCN)
- first end-to-end architecture -> 입력부터 출력까지 미분 가능한 layer로 이루어짐
- 그래서 input 해상도와 output해상도를 맞출 수 있다.
- Fully Connected는 해상도가 다르면 구성한 dimension에 안 맞기 때문에 출력이 이상하게 된 점을 해결함

#### difference fully connected and fully convolutional
- fully connected: Output a fixed dimensional vector and discard spatial coordinates
- fully convolutional: Output a classidication map which has spatial coordinates
- connected는 그냥 flattening하고 Fc layer input으로 적용 -> 영상의 공간정보 고려 X 하나의 데이터로 섞임 <br>
채널 축으로 flattening하고 그 layer들을 convolution화 시킴 -> 각 위치마다 vector가 하나씩 나옴 -> fully conv 방식이랑 비슷
- fully conv는 1x1 convolution with, channel 개수 filters 만들어서 연산 진행 <br>
이때 위에 거랑 다른 점은 slide window 방식으로 적용되기 때문에 spatial 정보 저장 가능
##### 이렇게 어떤 입력 size에도 대응이 가능한 방법이 나옴
---
#### 그러나 Predicted score map is in a very low-resolution이라는 Limitation 있음
- Why? For having large receptive field, several spatial pooling layers are deployed
- solution is Enlarge the score map by upsamapling
--- <br>
## what is upsampling
- receptive field를 늘려서(stride랑 Pooling 작게 해서) 영상의 전반적인 context를 파악하고 나중에 품질 올리는 것
#### upsampling 방법
1. Transposed convolution -> 학습 가능한 upsampling을 하나의 layer로 한 번에 처리
- Transposed convolutions work by swapping the forward and backward passes of convolution
2. Upsample and convolution(둘을 같이 이용) -> upsampling operation을 두 개로 분리
- Avoid overlap issues in transposed convolution -> better approaches for upsampling
- 이때 Nearest-neighbor, Bilinear interpolation followed by convolution
--- <br>
#### FCN에서의 layer 깊이에 따라 차이점
- low level: receptive field size가 작아서 작은 차이에도 민감
- high level: 해상도는 낮아지지만 receptive field size가 커서 의미론적인 정보를 포함함

#### FCN은 low level feature와 high level feature까지 고려함(합쳐서 modeling한다.)
- 따라서 Faster and Accurate

## 비슷한 모델로 Hypercolumns for Object segmentation이 있음
- Difference: Apply to each bounding box

---<br>
## U-Net -> 영상 전체가 아니라 영상의 일부분을 봐야하면 이것을 사용

- 더 높은 성능의 segmentation 결과
- Built upon 'fully conv network'
- 낮은 층의 feature와 높은 층의 feature를 더욱 잘 결합하는 방법 제시(similar to skip connections in FCN)

1. Contracting path
    - activation map의 해상도 절반씩 줄어듦
    - channel 수는 절반씩 늘어남
2. Expanding path(upsampling 경로)
    - activation map의 해상도 절반씩 늘어남
    - channel 수는 절반씩 줄어듦

### !> Concatenation of feature maps provides localized information

### U-Net에서는 down, up이 자주 일어나는데 이때 홀수라면 버림되는지 올림되는지 check 필요

## DeepLab(후처리(CRF)가 존재함, Atrous Conv 존재)
- Conditional Random field: 후처리 툴, 픽셀과 픽셀 사이의 관계를 이어줌
- 처음엔 엄청 별로인 score맵이 나오는데 mask를 적용하고 반복하여 score 올림

### DeepLab은 Dilated Conv 사용: Atrous Conv
- 실제 conv영역보다 넓은 영역 보게 하고, parameter 늘어나지 않게 함 -> receptive field가 exponential하게 증가하는 효과
- Depthwuse separable conv 사용
    1. 채널 별로 conv돌려서 값을 뽑음(일반적인 것과 달라) -> 채널 별로 activation map나옴
    2. 위를 진행하고 Pointwise Conv를 통해 하나로 합침
    - 위 두 단계를 거치면 conv특징 유지도 되고, 계산량도 줄어듦

### DeepLab 단계
1. Atrous Conv로 넓은 receptive field를 적용하여 feature map 구함
2. 다양한 rate의 Dilated conv를 통해서Atrous spatial pyramid pooling 구현(영상 내의 물체가 다양한 scale이 있는데 이를 고려하기 위함)
3. convolution을 통해 이를 하나로 합친다.
4. Decoder함(low level feature와 pyramid pooling을 upsampling한 것을 concat함)
5. upsampling 하여 segmentation map 구함