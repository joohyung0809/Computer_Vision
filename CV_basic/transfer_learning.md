## Transfer Learning
#### small dataset으로 실용적이게 할 수 있음 기존에 학습된 지식으로 연관된 새로운 테스크로도 높은 성능에 도달 가능
- 즉 한 데이터 셋에서 배운 기술을 다른 곳에서 적용이 가능하다.(단 공통점이 있어야 함)

## Approach 1 (데이터가 정말 적은 경우) - Fine Tuning
#### 하나의 데이터 셋에 미리 학습한 모델을 준비
- conv layer 이후에 있는 FC layer를 잘라냄
- 그리고 새로운 FC layer를 붙임
- 학습을 새로 붙인 Layer에 대해서만 학습함

## Approach 2 (Approach 1보다는 데이터가 많은 경우)
#### 하나의 데이터 셋에 미리 학습한 모델을 준비
- conv layer 이후에 있는 FC layer를 잘라냄
- 그리고 새로운 FC layer를 붙임
- Conv Layer까지 학습함 (이때 Conv Layer는 학습율을 낮추고(느리게 학습), FC LAyer에 대해서는 학습율을 높임(새로운 task를 빠르게 학습))


## Knowledge distillation (Already trained(teacher) -> Not trained(pre-trained 보다는 작은 network))
#### 모델 압축에 유용하게 사용
- 각 model의 ouput을 내고 back-propagation을 Not trained(student) 모델에만 적용시킴
- model 학습 방법이 student가 teacher를 따라하게 만드는 방법임 - teacher output쪽으로 맞추도록 학습됨


#### knowledge distillation in Labeld data
- 잠깐 용어 정리! One-hot Vector : Hard label / 수치적인 값: Soft label -> soft prediction을 사용하기 위해 soft-label 이용
* 이 softlabel들의 개형이 중요한 거지, 각 값이 중요한 것이 아님

- softmax(T=t)에서 t 값은 단순히 softmax를 적용했을 때 극단적으로 값을 벌리는 문제점을 보완하여 smooth하게 값을 벌림
 

## Leveraging unlabeled dataset for training

#### Semi-supervised learning : Unsupervised(Np label) + Fully Supervised(fully labeld)
- 추가적인 labeling없이 성능 향상
- labeld dataset을 활용하면 사용하는데 한계가 있는데 unlabeld는 인터넷 상의 무궁무진한 데이터 사용 가능

# 위에 나온 것들을 결합한 것이 Self-training
1. teacher model로 새로운 unlabeld data set 만들어서 Semi-supervised learning 시킨다.
2. learning시킨 student 모델을 다시 teacher로 바꾸고 새로운 student model 가져와서 반복
- 이 traing은 특이하게 반복될 수록 teacher 모델의 network가 작아짐




