## Neck
- 원래는 마지막 feature map을 가지고 ROI를 분석했는데 backbone 중간에 나오는 feature map도 사용하자
- backbone을 충분히 통과한 high level map은 큰 객체를 표현한다.
### Neck이 필요한 이유
- 다양한 크기의 객체를 더 잘 탐지하기 위해
- 하위 level의 feature는 semantic이 약하므로 상대적으로 semantic이 강한 상위 feature와의 교환이 필요
---<br>
## Feature Pyramid Network(FPN)
- high level에서 low level로의 semantic 정보 전달 필요
- 따라서 top-down path way 추가
    - pyramid 구조를 통해서 high level 정보를 low level에 순차적으로 전달
        - low level = Early stage = Bottom
        - High level = Late Stage = Top
---<br>
## PANet
- bottom-up을 한 번 더 추가한 것