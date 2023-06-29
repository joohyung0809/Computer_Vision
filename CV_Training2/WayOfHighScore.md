### In Segmentation Competition
- Library: Segmentation Models
    - 많은 형태의 Encoder-Decoder 모델을 제공
1. library download = !pip install git+https://github.com/qubvel/segmentation_models.pytorch
2. 설치한 것 import = import segmentation_models_pytorch as smp
3. 모델 불러오기<br>
<img src = './model_call.png'> <br>
- encoder weights의 name에 따라서 달라지는 것 있음 -> 공식문서 확인
---<br>

### 주의해야할 사항들
1. 디버깅 모드: 실험환경이 잘 설정되었는지 체크하기 위한 과정
    - 실험이 작동되다 말 수도 있으니 미리 디버깅 해놓고 진짜 실험은 자기 전
- 샘플링을 통해서 데이터 셋의 일부분만 추출
- epoch 1~2정도 설정하여 loss가 감소하는지 확인
    - step에 따라 loss가 제대로 감소했다면 CFG.debug를 False로 바꿔서 전체 실험 진행
2. 시드 고정: 모델의 성능을 비교할 때, 성능이 달라지는 것을 방지하고 재생산해내기 위해 시드를 고정
- torch와 numpy, os 관련 시드 고정
<img src ="./seed.png"><br>

- validation 검증셋의 시드 고정
3. 실험기록: 팀원 간의 정보 공유, 같은 실험 반복하지 않도록 방지
- Network 종류, Augmentation 방법, Hyperparameter 등 성능에 영향을 주는 조건을 바꿔가며 실험을 진행한 후 그 결과 기록
    - 이때 notion, Google 스프레드 시트, slack, 카카오톡 등을 이용
    - 파일 명에 실험을 기록하거나 패키지 등의 도움을 받기도 함
4. 실험은 한 번에 하나씩(하나의 조건만을 변경)
- 에를 들어 이전 실험 조건에서 Network 종류와 Augmentation 방법을 모두 변경하여 실험할 경우, 두 조건 중 어떤 조건이 영향을 주었는지 알기 어려움
5. 팀원간의 역할 분배
- baseline 코드의 성능을 높이는 것, 같은 task의 대회에서 우승자들의 solution, 문제점이 생겼을 때 해결방법 찾기, 대회 참여자들의 논의들
- solution과 discussion 조사는 한 사람이 하기
    - segmentation workshop, kaggle 등
- 독립적으로 베이스라인 코드를 만들어서 마지막에 앙상블 하는 것이 효과가 좋음
    - 한 가지 baseline만 가지고 하지 않고, 중복되지 않게 실험하는 것도 중요
### baseline 이후에 실험해봐야 할 사항들(밑에 나오는 것 말고도 다른 대회 우승자들의 기술도 실험 해보기)
#### Validation 해야 함
- 제출하지 않아도 성능 평가할 수 있고, Public 리더보드의 성능에 오버피팅 되지 않도록 도와준다.
---
1. Hold out
- 전체 데이터를 8:2로 분리 -> 해당 모델로 inference 진행
2. K-Fold
- 8:2로 분리하는 것은 같지만, Split 개념을 도입해 모든 데이터가 학습에 참여한다는 장점
- Split 수만큼 독립적인 모델을 학습하고 검증
- 독립적인 모델로 Test 데이터에 대해 각각 interface한 후, Ensemble
---
- 신뢰성 있는 검증결과를 가져올 수 있지만, 시간소요 많고 K의 선택해야 한다는 문제
- 실제로 잘 사용하지 않음 -> 3, 4를 많이 사용
3. Stratified K-Fold
- 기존의 K-Fold 방식은 Class Distribution을 고려하지 못함
- 이를 고려하여, Fold마다 class distribution을 동일하게 Split 방식으로, Class가 imbalance한 상황에서 좋음
- 즉, class의 비율을 균등하게 나누어 folding함
4. Group K-Fold
- train과 validation의 그룹을 나누어 힌트를 보지 않고 검증을 평가한다고 생각하면 좋음
---
### Skill & Technique
1. Augmentation
- 데이터 수를 증가, Generalization 강화, 성능 향상, class imbalance 문제 해결
- Augmentation Library : Albumentation
    - classification, semantic segmentation, object detection, pose estimation 등 다양한 분야에 적용 가능
    - Numpy, OpenCV, imgaug 등 여러 library들을 기반으로 최적화해 속도가 빠름
    - pytorch와 연동 좋음
- 도메인에 맞는 Augmentation 기법이 필요(무지성X)
---
### Augmentation 종류 
1. Cutout
- Random 하게 box를 생성해서 사진마다 성능의 편차가 존재
2. Gridmask
- Cutout의 문제를 해결
- 규칙성 있는 box를 통해 Cutout하는 방안을 제시
- 각 x, y 방향 mask의 개수를 결정
- mask의 크기도 결정
- mask의 시작점이 random offset으로 정해짐
3. Mixup
- 두 이미지를 얼만큼의 비율로 섞을지 람다로 정함
- 이때 이미지만 해당 비율로 만드는 것이 아니라, label까지 비율 적용
4. Cutmix(효과적이고 자주 사용)
- Mixup 처럼 겹치는 것이 아니라 특정 부분은 a이미지, 나머지 부분은 b이미지를 적용하는 방법
---
- 먼저 랜덤하게 잘릴 박스를 추출(비율 setting)
- 다음으로 box위치 결정
- input 이미지들의 순서를 섞고 box의 크기를 결정할 lambda 추출(이때 섞을 이미지 추출은 randperm(permutation)으로)
    - cutmix에 대한 이미지의 loss는 각 이미지가 차지하고 있는 비율만큼의 loss를 구하여 합한 것을 loss로 사용함
- 이는 함수로 간단히 사용 가능
5. SnapMix - 영역 크기만을 고려한 라벨을 생성했던 CutMix와 달리 영역의 의미적 중요도를 고려해 라벨을 생성
- CAM(Class Activation Map)을 이용하여 이미지 및 라벨을 mixing하는 방법
6. CropNonEmptyMasklfExists
- object가 존재하는 부분을 중심으로 crop할 수 있다면 model의 학습을 효율적으로 할 수 있음
---
- Augmentation 무작정 하지말고 한 번 적용해서 어떻게 되는지 확인 후에 제대로 적용하는 것이 좋음 <br>
### 다양한 모델들에 대한 실험
- 가장 좋은 Encoder-Decoder모델을 세팅해야 함
- SOTA 까지 하면 좋음
- Segmentation에선 HRNet이 좋음
---<br>
### Hyperparameter setting
- batch size, weight decade, learning rate 등
- 이때 적절한 learning rate 찾는 것 중요
    - learning rate schedules를 사용해야 함
    - 상황마다 learning rate 수정하는 방법
    - 빠르고 정확함

### Scheduler의 종류
- CosineAnnealingLR
    - LR을 cosine 형태로
    - 최대값과 최소값 사이에서 learning rate를 급격히 증가시켰다가 감소시켜서 saddle point, 정체구간을 빠르게 벗어남
- ReduceLROnPlateau
    - metric의 성능이 향상되지 않을 때 learning rate를 조절하는 방법
    - Local minima에 빠졌을 때 learning rate를 조절하여, 효과적으로 빠져나옴
    - 즉, Val IoU가 epoch가 늘어남에도 감소하는 경우 lr을 감소시킴
    - 이 방법은 lr을 조절하는 폭을 loss를 사용할지, IoU를 사용할지, 몇 번 IoU가 증가하면 조절할지 등 인자들 고려해야 함
- Gradual Warmup
    - 작은 lr로 출발하여 특정 값에 도달할 때 까지 lr을 서서히 증가시키는 방법
    - 이 방식을 사용하면 weight가 불안정한 초반에도 비교적 안정적으로 학습을 수행할 수 있음
    - backbone 네트워크 사용시에 weight가 망가지는 것을 방지
---
### Batch Size setting
- gradient Accumulation -> batch size 크면 학습속도가 빨라지고 다양한 데이터 셋에 대해 학습하여 일반성이 좋아
- 하지만 메모리 문제가 있는데 이를 해결한 것이 gradient Accumulation
    - 모델의 weight를 매 step마다 업데이트 하지 않고 일정step 동안 gradient를 누적한 다음 누적된 gradient를 사용해 weight를 업데이트 하는 방법
    - 이는 배치 사이즈를 키우는 장점이 있음
- 참고로 마냥 크다고 좋은 것만은 아님
---
### Optimizer 결정
- Adam
- AdamW
- AdamP
- Radam
- Lookahead optimizer
    - Adam, SGE를 통해 k번 업데이트 후, 처음 시작했던 point 방향으로 1 step back 후, 그 지점에서 다시 k번 업데이트를 시작하는 방법
    - Adam이나 SGD로는 빠져나오기 힘든 Local minima를 빠져나올 수 있게 한다는 장점이 있음
---
### Loss 결정
- 보통 cross entropy loss 사용하겠지만 다른 loss도 정말 많음
- Compound loss계열은 imbalanced segmentation task에 강한 모습
- DiceFocal loss, DiceTopK loss 등 하나의 loss만 쓰는 것이 아니라 같이 쓰는 것이 성능이 좋음