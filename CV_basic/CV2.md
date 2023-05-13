## Data Augmented
#### bias되거나 부족한 data를 풍부하게 한다. 
- (ex 데이터 발기를 조절하거나, crop or rotate , affine transformation: (길이비율 유지, 평행관계 유지) -> point 잡아,
   Cut Mix : 데이터 합성하는 방법 -> iamge 합성 뿐만 아니라 라벨도 합성함,)
- RandAugment: 위 기법들을 어떤 방법으로 조합할까?(기법을 한 번만 사용하지 않아도 되기 때문) -> random 하게 적용하는 방법 -> 여기의 parameter는 어떤 augment 적용할까, 어떤 강도로 적용할까

