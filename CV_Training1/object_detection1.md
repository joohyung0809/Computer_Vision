## detection 평가 방법
- 성능: mAP(mean average precision): 각 클래스 당 AP의 평균
    - Confusion Matrix : TP / TN / FP / FN
    - Precision & Recall
        - Precision = TP / (TP +FP) = TP / All Detections
        - Recall = TP / (TP +FN) = TP / All Ground truths
    - PR curve
        - precision과 Recall을 누적해서 계산해 나가는 방법
        - confidence 순서로 내림차순 정렬 후 TP->TP->FP->TP 순서였다면 <br> precision은 1/1->2/2->2/3->3/4, Recall은 1/10->2/10->2/10->3/10이 됨
        -> (Recall, Precision) 형태로 그래프 찍음
    - AP(Average Precision)
        - PR curve를 y축으로 선을 긋고 적분한 것
        - mAP는 한 클래스에 대해 AP를 구하고 클래스 별로 평균낸 것
    - IOU(Intersection Over union)
        - overlapping region / combined region = 겹치는 영역 / 박스들 전체 영역
- 속도: FPS(Frames Per Second), Flops(Floating Point Operations)
    - FPS
    - FLOPs : Model이 얼마나 빠르게 동작하는지 측정하는 metric, 연산량 횟수
        - Convolution layer Flops = Cin x Cout x K x K x Hout x Wout

## 라이브러리 종류
- MMDetection
- Detectron2
- YOLOv5
- EfficientDet

## detection domain 특성
- 통합된 library가 없음