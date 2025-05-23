# NextTokenPrediction
셰익스피어 문체를 학습하여, 셰익스피어처럼 문장을 생성하도록 하는 ai모델 만들기 (NextTokenPredicition)사용
목표: 셰익스피어 문체 학습 및 문장생성
본 실험의 목표는 셰익스피어의 희곡 문장을 잘 학습시켜 셰익스피어 특유의 문체로 글을 생성하는
것이다.
데이터셋: tiny-shakespeare 사용
허깅페이스에 명시된 바와 같이, tiny-shakespeare 데이터셋은 셰익스피어의 다양한 희곡에서
추출된 약 4만 줄의 문장으로 구성되어 있다. 본 실험의 목표는 이 데이터셋을 활용하여 셰익스피어
특유의 문체를 학습하고, 유사한 스타일의 문장을 생성하는 것이다.
1. 최초 모델: distilgpt2
모델 크기에 따른 성능 변화를 관찰하기 위해
distilgpt2를 기준으로 더 작거나 큰 모델들과의 비교
실험을 진행하였다. distilgpt2 모델과 AdamW
옵티마이저를 사용하여 학습 및 검증을 수행한 결과,
val_loss는 4.9829를 기록했다. 목표 val_loss
(5언더)에 도달했지만, 추가 실험을 통해 더 나은 성능을
달성하고자 했다.
(*모든 val_loss의 시각화 그래프의 x축은 epoch, y축은
평균 val loss다.)
2. 모델 크기 변화 실험: ssleifer/tiny-gpt2, EleutherAI/gpt-neo-125M
distilgpt2(82 million parameter)보다 작은 모델인 ssleifer/tiny-gpt2(10 million parameter)와 큰 모델인
EleutherAI/gpt-neo-125M(125million parameter) 을 각각 적용하여 실험을 진행하였다. 두 모델 모두
val_loss가 10을 초과하는 결과를 보였다. 특히 EleutherAI/gpt-neo-125M 모델의 경우, epoch 진행에
따른 val_loss 감소 없이 거의 수평적인 그래프가 나타났다. 이 둘다 생성된 문장을 보았을 때 뚜렷이
이상하다는 것을 감지할 수 있었다.
*ssleifier/tiny-gpt2가 생성한 문장
프롬프트: 'To be, or not to be'
생성 결과: 'To be, or not to
beananananananananananananananananananananananananananananananananananananananananananan
*EleutherAI/gpt-neo-125M이 생성한 문장
프롬프트: 'To be, or not to be'
생성 결과: 'To be, or not to be DONadowidentallyapy wanderedimilationpostsers Uruguay#$size
attempting Terenentu weighedimilation binaries Uruguay Equalityillionsgun
pneumoniaduleenospels completesoneliness Uruguayscripts Imm Tehran Tehran Midnighters forc
chilled attemptingers Uruguay Tehranillions'
(*ssleifer/tiny-gpt2) (*EleutherAI/gpt-neo-125M)
3. 추가 데이터셋을 활용한 학습 실험: Trelis/tiny-shakespeare
모델을 distilgpt2로 고정하고, 다른 데이터셋을 적용하여
성능 변화를 확인하였다. 셰익스피어 문체 학습을 위해
허깅페이스의 또 다른 셰익스피어 문장 데이터셋인
Trelis/tiny-shakespeare를 사용하여 학습을
진행하였다. 그 결과, val_loss는 4.830으로 가장 낮은 결과를
도출했지만, 첫 번째 실험 결과와 큰 차이는 없었다. 이
데이터셋의 사전 학습 문장 수가 590으로 상대적으로 적었던
점이 결과에 영향을 미쳤을 것으로 추측한다.
4. 옵티마이저 변화 실험: Lion
가장 낮은 val_loss를 기록한 4번 실험 조건에서 옵티마이저를 최신 옵티마이저인 Lion으로 변경하여
실험해보았다. 그 결과, val_loss가 감소하다가 특정 시점 이후 증가하는 불안정한 양상을 보였다.
따라서 옵티마이저는 AdamW를 유지하는 것이 적절하다고 판단했다.
결론
본 실험 결과, 사전 학습된 distilgpt2 모델과 AdamW 옵티마이저를 사용하는 것이 가장 효과적인
것으로 나타났다. 아쉽게도 코랩 GPU의 제한으로 인해 충분한 사전 학습을 진행하지 못했지만, 충분한
자원이 확보된다면 사전 학습 정도에 따른 모델 성능 변화를 추가적으로 관찰하고 싶다.
