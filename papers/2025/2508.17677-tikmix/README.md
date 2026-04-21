# TiKMiX: Take Data Influence into Dynamic Mixture for Language Model Pre-training

- **Paper**: TiKMiX: Take Data Influence into Dynamic Mixture for Language Model Pre-training
- **arXiv**: [2508.17677v1](https://arxiv.org/abs/2508.17677v1)
- **Authors**: Yifan Wang, Binbin Liu, Fengze Liu, Yuanfan Guo, Jiyao Deng, Xuecheng Wu, Weidong Zhou, Xiaohuan Zhou, Taifeng Wang
- **Venue**: arXiv 2025
- **Read date**: 2026-04-21
- **Tags**: data-selection, pretraining, dynamic-mixture, data-influence, semi-offline

## One-line Summary

TiKMiX는 pretraining 중 domain별 Group Influence를 추정해 모델의 변화하는 data preference에 맞춰 data mixture ratio를 동적으로 조정하는 language model pretraining 방법이다.

## Key Idea

pretraining data mixture는 모델 최종 성능을 좌우하지만, 고정 ratio는 학습 단계별 preference 변화를 반영하지 못한다. TiKMiX는 domain/group 단위 영향도를 효율적으로 추정하고, 이 영향도를 최대화하는 mixture를 찾는다.

사용자 메모의 "semi-offline" 관점에서 중요하다. 완전 online으로 매 step 모든 데이터를 재평가하지 않고, stage-wise 영향도 평가와 mixture update를 결합하는 hybrid 설계로 볼 수 있다.

## Method

- domain/group 단위로 Group Influence metric을 정의한다.
- 현재 모델 상태에서 각 data domain이 성능에 미칠 영향을 추정한다.
- TiKMiX-D는 influence-maximizing mixture를 직접 최적화한다.
- TiKMiX-M은 regression model로 더 나은 mixture를 예측한다.
- 추정된 mixture를 다음 pretraining stage에 적용한다.

## Experiments

여러 parameter scale의 모델을 최대 1T tokens까지 pretraining하며, REGMIX 등 static/dynamic mixture baseline과 비교한다. 9개 downstream benchmark에서 평균 성능을 평가한다.

## Results

arXiv 초록 기준, TiKMiX-D는 REGMIX 같은 최신 method보다 20% compute만 사용하면서 더 높은 성능을 보이고, TiKMiX-M은 9개 downstream benchmark 평균 약 2% 성능 향상을 낸다. 분석은 model preference가 training progress와 scale에 따라 변함을 보인다.

## Discussion

MATES가 sample-level influence model로 다음 stage 데이터를 고르는 방식이라면, TiKMiX는 domain/group mixture ratio를 동적으로 조정한다. 따라서 대규모 pretraining에서 더 실용적인 granularity를 택한 방법으로 볼 수 있다.

연구 설계상 "online vs offline" 이분법 대신, semi-offline stage-wise mixture update라는 중간 지대를 정의할 때 좋은 reference다.

## Limitations

- group/domain 정의가 부정확하면 Group Influence도 왜곡된다.
- sample-level target selection보다 coarse한 조정만 가능하다.
- dynamic mixture의 이득이 target benchmark별로 균등한지는 추가 분석이 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2508.17677>
- Hugging Face Papers: <https://huggingface.co/papers/2508.17677>
