### FCN의 틀 세 가지
1. VGG 네트워크 백본을 사용
2. VGG 네트워크의 FC Layer를 Convolution layer로 대체
3. Transposed Convolution을 이용해서 Pixel Wise Prediction을 수행

- 1번 VGG사용하면 pretrained 되어있어서 편함 <br>
- FC layer는 Flatten을 진행하는데 이는 각 픽셀의 위치정보를 해침(classification은 괜찮았음)
    - 하지만 2번으로 각 픽셀의 위치정보를 해치지지 않은 채로 특징을 추출할 수 있어
    - 1x1 Conv을 사용하면 임의의 입력값에 대해서도 파라미터 변경이 필요 없음
        - conv는 height, width와 상관이 없는데, Linear로 하면 기존 세팅과 다른 height, width가 들어오면 학습이 안됨<br>
- upsampling == Deconvolution == Transposed convolution
---<br>
### FCN
- MaxPooling에서 잃어버린 정보를 복원해주어야 함
    - 근데 한 번에 복원하면 좋지 않음
- 그래서 잃어버렸던 만큼 계속 복원해주는 방법 사용 -> 효율적인 이미지 복원 가능(skip connection)
---<br>
### FCN의 한계
1. 객체의 크기가 크거나 작은 경우 예측을 잘 하지 못하는 문제
- 큰 object의 경우 지역적인 정보만으로 예측
    - 버스의 앞 부분은 버스로 예측, 유리창에 비친 자전거를 보고 자전거로 인식하는 문제도 있음
    - 이는 conv의 receptive field의 문제
- 같은 object여도 다르게 labeling
- 작은 object가 무시되는 문제가 있음
---
2. Object의 디테일한 모습이 사라짐
- deconv 절차가 간단해서 경계를 학습하기 어려움