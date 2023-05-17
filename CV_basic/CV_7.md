## CNN Visualization - Convolution neural Network를 시각화 <br>
#### visualizetion의 목적
- What is inside CNN
- Why do they perform so well
- How would they be improved<br>
#### filter visualization
- 첫 번째 conv layer의 filter는 방향성 등을 가짐 -> 이 filter를 기반으로 만든 activation map도 visualization 가능
- 첫 번째 다음의 layer는 해석이 불가능하게 visualization됨 <br><br>
## Embedding feature analysis 1
---
#### Nearest Neighbors(NN) in a feature space
- 각 영상에 대해 유사도가 높은 영상을 DB에서 뽑아냄
- We can notice semantically similar concepts are well clustered
- The embedding feature comparison does not have the limitation of the simple pixel-wise comparison
- 픽셀 별로 비교하면 포즈나 위치가 달라지면 같다고 생각하지 못 하지만 이 모델은 가능하다.
---
- 각 DB에서 feature 들을 뽑아서 high dimensional feature space에 저장됨 (원래 영상과 association 되어 있음)
- 그 dimesion에 질의할 영상을 넣어 주변 관계를 파악하는 것
- 검색된 예제를 보고 분석하면 전체적인 그림을 파악하기 어려운 단점이 존재
## Embedding feature analysis 2(1을 보완하기 위함)<br><br>
#### Dimensionallity reduction
- t_SNE 방법 -> 색깔별로 구분되어 있는 것을 보고 학습이 잘 되었구나 확인할 수 있음
- 경계가 붙어있다는 것은 class끼리를 비슷하게 바라보고 있다(헷갈릴 수 있다)는 뜻.
---
## Activation investigation 
#### Hidden node를 시각화하여 얼굴을 찾는 노드인지, 계단을 찾는 노드인지 등을 파악함.
- 해석적인 방법
---
#### Maximally activating patches(patch를 뜯어서 살펴봄)
- 각 layer의 channel에서 하는 역할을 알아내기
- patch들의 특징이 공통적인지 확인할 수 있는 방법
- 중간적인 layer에서 적합<br>
#### 과정
1. 분석하고자 하는 layer하나를 정함
2. 예제 data를 backbone 네트워크에 넣고 각 layer의 activation을 뽑는다.
3. 1.에서 정한 layer를 저장함
4. activation에서 가장 큰 값을 찾고 그 큰 값을 갖게 한 receptive field를 찾는다.
5. 해당 입력 영상에 대한 patch를 가져옴
---
이 연산은 최적화 과정이 필요함(Regularization term)
- arg max f(I) - Reg(I)
- max f(I): 특정 class를 score로 사용하여 큰 값을 찾음 -> 이 값을 높이는 I를 찾으라는 과정
- 높이는 이유는 Gradient ascent방법을 사용해서임
- Reg(I)= (람다)(Ixy)^2의 합 -> 얘가 작아지는 것이 좋음(람다로 조정)
#### 과정(최종적인 결과를 보고 CNN의 결과를 분석)
- 임의의 영상을 즉, 분석하고자 하는 CNN모델을 넣어줌, 관심 있는 class의 score넣어줌
- 입력 단계까지 backpropagation을 해서, 입력값을 어떻게 바꿔야 target class의 score를 높아질지 찾는 것.
- 입력 update를 해주고, 그 update된 영상으로부터 반복 진행
- 결론: 어떻게 입력을 바꿔주어야 결과가 바뀌는지 확인하는 방법론
---<br><br>
## Model decision explanation(특정 입력이 어떤 각도로 바라보고 있는지 해석하는 방법론)
#### Saliency Test 1
- 영상이 제대로 판정되기 위한 각 영역의 중요도 도출
1. Occlusion map -> P(어떤 클래스 | Occlusion A) -> A라는 패치로 가리는 것
    - 중요한 부분을 가리면 score 많이 줄어들어
    - Prediction scores drop drastically around the salient parts
---<br>
#### Saliency Test 2
- via Backpropagation
1. 입력 영상 넣어주고, 하나의 class score를 얻는다.
2. Backpropagation을 input domain까지 함.
3. Visualize the obtained gradient magnitude map
- input 단에서 어느 부분이 민감한지 알 수 있음 -> data의 해석방법을 알고 싶음
---<br>
#### Backpropagation-based saliency
- Backpropagation할 때 weight를 마스킹 하는 방법
- Backprop + DeConv = Guided backprop
---<br>
#### Class activation mapping(CAM)
- 어떤 부분을 많이 참조하여 결과를 냈는지
- featur map(activation map)을 FC layer로 바로 통과하지 말고 Global average pooling을 먼저 하고 FC layer를 하나만 통과시킴
- 얘네들이 score를 만드는 식은 각 feature에 각 GAP의 combination을 통해 구함
- 단점: 마지막 부분의 architecture를 바꾸고 재학습을 시켜야 한다는 것 + CAM 사용하고 나면 성능이 바뀔 수 있음
- ResNet, GoogleNet은 구조가 똑같아서 수정 안 해도 적용 가능
---<br>
#### Grad-CAM(재학습도 필요없고, 구조도 변경없이 미리 학습된 구조에서 CAM 뽑아내는 것)
- backbone이 CNN이기만 하면 되고, 꼭 영상 task가 아니라 어느 task 종류여도 적용 가능.
- Key idea : How to get importance weight
    - 여기서는 input domain까지 backprop하지 않고 activation map 까지만
    - 다음으로 선형결합을 함. ReLU 사용하여 양수 값만 사용
- Grad-CAM은 CNN만 있으면 되서 뒷 단에 어떤 구조가 붙어도 상관 없음
    - Grad-CAM과 Guided BackProp을 곱해서 사용할 수 있음.
---<br>
#### SCOUTER
- 정답이 왜 아닌지까지 비교 대조할 수 있는 방법으로 확장
- CNN이 사람도 공감할 만한 지식을 자발적으로 알려줌
---<br>
#### 위 Visualization을 잘 이용하면 어떤 hidden node가 물체의 어떤 부분을 담당하는지, 생성하는 것을 기여하는 부분이 어딘지 파악 가능
- 이를 통해 사용자가 해당 영상을 수정하는 것에도 사용 가능
- EX: GAN을 통해 영상에 masking을 함