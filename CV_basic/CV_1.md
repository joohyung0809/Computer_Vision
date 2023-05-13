## CV란
>#### 이미지나 영상으로부터 본질을 파악하는 것
>#### Inverse rendering

## Computer graphics는 CV의 반대
>#### 이미지나 영상을 분석하여 얻은 representation을 다시 graphic화 시키는 과정을 의미함 

## CV는 우리가 가진 biological한 장단점을 알아둬야 함
>#### 우리의 눈은 편향된 학습을 하고 있음

## Image Classifier
#### 어떤 물체가 영상이나 사진에 들어있는지 매핑하는 것
- 모든 데이터를 가지고 있으면 단순 Search지만 현실적으로 불가능
- 그래서 Neural Network를 사용하는데 이게 모든 데이터를 Compress 한 것으로 생각하면 됨

## Image Classifier를 할 때 Fully Connected Layer를 사용할 때 문제점
#### 하나의 특징(w)을 뽑기 위해서 모든 픽셀을 고려하는 방법임
- 학습된 것과 조금이라도 다른 데이터가 들어가면 잘못된 결과를 반환할 수 있음.

## 그래서 Locally Connected Layer(CNN)를 사용함
#### 공간적 특성을 뽑아 고려한 방법
- 이 방법은 sliding window 방식으로 학습한 것과 조금 달라도 적절하게 특징을 뽑음

## CNN -> backbone으로 쓰임 (base network)