## Pytorch 기반
   - detection외에도 segmentation, Pose prediction 알고리즘도 제공
#### pipeline
- Setup Config / Setup Trainer / Start Training
#### 해당 library에 loss나 backbone이 없는 경우
- 직접 정의하고 등록해서 사용함
- Detectron2는 mapper를 정의하기 때문에 높은 자유도를 갖고 있음

### 참고로 R-CNN모델을 Detectron2로 구현하고, MM_Detection으로도 구현해서 ensemble하면 더 좋은 성능이 나오기도 함 -> 라이브러리 별로 다양성이 높아지니까