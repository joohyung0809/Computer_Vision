### Cascade
- Fast R-CNN에서 positive sample과 negative sample을 나누는 것에 집중함
---
- IoU Threshold 에 따라 다르게 학습되었을 때 결과가 다름
- Input IoU가 높을수록 높은 IoU Threshold에서 학습된 model이 더 좋은 결과를 냄
---
#### 정리하면
- 학습되는 IoU에 따라 대응 가능한 IoU박스가 다름
- 그래프와 같이 high quality detection(Input IoU 높다면)을 수행하려면 IoU threshold를 높여 학습
    - 단, 성능이 하락함
- 이를 해결하기 위해 Cascade RCNN 제안
---
### Cascade R-CNN 방식은
- 여러 개의 RoI head(H1, H2, H3)를 학습
    - 이때 Head 별로 IoU Threshold를 다르게 설정
    - 가장 마지막 C3, B3이 결과임
- high quality detection에서 이 방식은 괄목할만한 성능 향상 보여줌
---<br>
#### 정리하면
- Bbox pooling을 반복하는 것은 성능 향상되는 것을 증명한다.
- Iou threshold가 다른 classifier가 반복될 때 성능 향상되는 것을 증명한다.
- Iou threshold가 다른 RoI head를 cascade로 쌓을 시 성능 향상되는 것을 증명한다.