# GradAlign: Gradient-Aligned Data Selection for LLM Reinforcement Learning

- **Paper**: GradAlign: Gradient-Aligned Data Selection for LLM Reinforcement Learning
- **arXiv**: [2602.21492v1](https://arxiv.org/abs/2602.21492v1)
- **Authors**: Ningyuan Yang, Weihua Du, Weiwei Sun, Sean Welleck, Yiming Yang
- **Venue**: arXiv 2026
- **Read date**: 2026-04-21
- **Tags**: data-selection, reinforcement-learning, gradient-alignment, llm, curriculum

## One-line Summary

GradAlign은 작은 trusted validation set의 policy gradient와 방향이 맞는 RL training problems를 선택해, LLM reinforcement learning에서 noisy/imbalanced/low-utility data를 피하는 adaptive curriculum 방법이다.

## Key Idea

LLM RL은 rollout이 현재 policy에서 생성되고 reward feedback이 학습을 바꾸기 때문에 SFT보다 훨씬 non-stationary하다. 단순 accuracy filter나 intermediate-difficulty heuristic은 문제의 scalar 난이도만 보고, 해당 problem이 downstream objective를 실제로 개선하는 방향의 update를 만들지 확인하지 않는다.

GradAlign은 "좋은 RL training problem"을 validation policy gradient와 방향이 일치하는 problem으로 정의한다. 방향 alignment를 보므로 magnitude가 reward normalization에 의해 왜곡되는 문제를 줄인다.

## Method

- 작은 validation problem set에서 현재 policy의 rollouts와 rewards를 생성한다.
- GRPO policy gradient를 각 validation problem에 대해 계산하고 평균해 target validation gradient를 만든다.
- candidate training problem마다 적은 수의 rollouts로 policy gradient를 추정한다.
- candidate gradient와 validation gradient의 cosine similarity를 alignment score로 사용한다.
- 상위 문제만 선택해 GRPO update를 수행하고, 다음 round에서 policy가 바뀐 상태로 다시 score를 계산한다.

## Experiments

세 가지 어려운 data regime에서 평가한다. reward가 50% corrupt된 noisy reward setting, Countdown Game이 전체 mixed corpus의 약 10%만 차지하는 distribution imbalance setting, low-utility corpus setting이다. Random, accuracy-greedy, static/gradient alignment baseline과 비교한다.

## Results

arXiv 초록 기준, GradAlign은 세 regime에서 기존 baseline을 일관되게 앞선다. AlphaXiv 요약 기준, noisy reward setting에서 AccGreedy는 pass rate가 50% 근처인 corrupt data를 high-signal로 오해해 80% 이상 선택하지만, GradAlign은 corrupt sample 비율을 낮게 유지하며 step 0에서 17.8%, step 100에서 29.9% 수준으로 억제한다.

imbalance setting에서는 target validation/test가 Countdown인데 Countdown problems가 corpus의 약 10%뿐인 상황에서도 GradAlign이 Countdown-val/test에서 가장 좋은 성능을 낸다. 이는 scalar difficulty가 아니라 validation-aligned direction이 중요한 선택 신호임을 보인다.

## Discussion

GradAlign은 LESS/GIST 같은 targeted instruction selection의 RL version으로 볼 수 있다. 다만 SFT의 fixed trajectories가 아니라 on-policy rollouts와 reward feedback을 사용하는 non-stationary setting이므로, selection도 round별로 policy state에 맞게 갱신되어야 한다.

사용자 연구에서는 validation/golden set을 target으로 쓰는 data selection을 RL post-training까지 확장할 때 중요한 baseline이다.

## Limitations

- candidate마다 rollout과 gradient estimation이 필요하므로 추가 compute가 있다.
- validation set이 작거나 편향되면 curriculum도 그 방향으로 편향될 수 있다.
- preliminary work라 더 큰 real-world RLHF/agentic datasets에서의 검증 폭은 추가 확인이 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2602.21492>
- AlphaXiv: <https://www.alphaxiv.org/abs/2602.21492>
- arXiv HTML: <https://arxiv.org/html/2602.21492>
- Code: <https://github.com/StigLidu/GradAlign>
