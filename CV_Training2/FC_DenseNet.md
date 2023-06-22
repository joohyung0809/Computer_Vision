### Skip connection을 적용한 모델
- 이전에 배운 ResNet
    - Neural Network에서 이전 layer의 output을 일부 layer를 건너 뛴 후의 layer에게 입력으로 제공
- FC DenseNet(단순 이전 layer의 정보만 제공하지 않고, block 내부의 모든 이전 정보를 건네줌)
    - 즉 직전 것만이 아니라, 이전 정보 모두 전달
- Concatenation 사용함 (summation이 아니라)