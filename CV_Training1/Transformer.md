### Vision Transformer
1. Flatten 3D to 2D (Patch 단위로 나누기) - pixel 단위는 연산량 너무 많아서
2. Learnable한 embedding 처리
3. Add class embedding, position embedding
4. Transformer
5. predict
---<br>
### DETR
- 기존의 object detection의 hand-crafted post process 단계를 transformer를 이용해 없앰
- CNN backbone, Transformer(encoder-decoder), prediction Heads (kneck구조는 없음)
    1. Transformer 특성 상 많은 연산량이 필요하여 highest level feature map만 사용
    2. Flatten 2D
    3. Positional embedding
    4. Encoder-Decoder
    5. Feed Forward Network(FFN)
    6. N개의 output(N은 한 이미지에 존재하는 object 개수보다 높게 설정)
- 전체적인 성능은 향상 but highest level feature만 사용하여 small image(APs) 하락
---<br>
### Swin Transformer - backbone에 transformer를 적용한 것임
- 원래 backbone로 적용 못 했는데 이는 CNN과 유사한 구조로 설계, Window라는 개념을 활용하여 cost 줄임
1. Patch partitioning
2. Linear Embedding(ViT와 달리 class embedding 제거)
3. Swin Transformer Block(multi-head attention 대신 / Window Multi-head self Attention이용)
    - 첫 번째 연산에는 Window Multi-head self Attention/두 번째는 Shifted Window Multi-head self Attention
    ---
    - Window Multi-head self Attention
        - window 단위로 embedding 나눈다
        - 기존 ViT는 모든 embedding을 Transformer에 입력, Swin은 Window 안에서만 transformer 수행
        - 따라서 이미지 크기에 따라 증가되던 computational cost가 Window 크기에 따라 조절 가능
        - Window 안에서만 수행하여 receptive field를 제한하는 단점 존재
    - Shifted Window Multi-head self Attention
        - 위 단점을 해결하기 위해 transformer block 2번째 layer에서 수행
        - window size와 다르게 나뉜 부분들 해결 필요 
4. Window Multi-head Attention(cost 줄임)
5. Patch merging
---
### 정리하면
- data 적어도 학습 잘 됨
- computation cost 줄임
- CNN과 비슷한 구조로 Object detection, segmentation 등의 backbone으로 general 하게 사용
- 성능 향상