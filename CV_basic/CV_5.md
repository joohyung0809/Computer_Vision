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

