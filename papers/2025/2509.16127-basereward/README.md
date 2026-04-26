# BaseReward: A Strong Baseline for Multimodal Reward Model

- **Paper**: BaseReward: A Strong Baseline for Multimodal Reward Model
- **arXiv**: [2509.16127v1](https://arxiv.org/abs/2509.16127v1)
- **Authors**: Yi-Fan Zhang, Haihua Yang, Huanyu Zhang, Yang Shi, Zezhou Chen, Haochen Tian, Chaoyou Fu, Kai Wu, Bo Cui, Xu Wang, Jianfei Pan, Haotian Wang, Zhang Zhang, Liang Wang
- **Venue**: ICLR 2026
- **Read date**: 2026-04-26
- **Tags**: mllm, reward-model, preference-data, data-curation, rlhf, alignment

## One-line Summary

BaseReward는 multimodal reward model을 만들기 위한 architecture, training, preference data mixture를 체계적으로 ablation하고, Qwen2.5-VL 기반의 단순한 2-layer reward head 모델이 강한 실전 baseline이 됨을 보인다.

## Key Idea

MLLM alignment에서 reward model은 중요하지만, 어떤 reward modeling paradigm, reward head, preference dataset, backbone, scale, ensemble이 실제로 좋은지 정리된 recipe가 부족하다. BaseReward는 static benchmark와 실제 RL pipeline 양쪽을 기준으로 이 선택지를 실험적으로 좁힌다.

흥미로운 결론은 generative reward model이나 critic-style model이 항상 더 낫지 않다는 점이다. RL loop에서 자주 호출되는 reward model은 빠르고 안정적인 scalar scoring이 중요하므로, 잘 설계한 Naive-RM이 실용적으로 강하다.

## Method

- Naive-RM, Critic-RM, Generative RM을 비교하고, 효율성과 성능을 함께 고려해 Naive-RM을 중심으로 삼는다.
- Reward head는 단순 linear head보다 2-layer MLP와 SiLU activation이 더 좋다고 보고한다.
- Auxiliary loss보다 core reward objective와 data mixture가 중요하다는 식으로 training strategy를 정리한다.
- 10개 이상의 multimodal/text-only preference dataset을 평가해, 도움이 되는 데이터와 해가 되거나 이득이 작은 데이터를 구분한다.
- 최종 BaseReward는 Qwen2.5-VL backbone, 2-layer reward head, 선별된 multimodal 및 text-only preference mixture로 학습한다.

## Experiments

VL-Reward Bench, MM-RLHF-Reward Bench, Multimodal Reward Bench, RewardBench/RMBench 같은 text-only reward benchmark를 함께 사용한다. 또한 BaseReward를 실제 MLLM reinforcement learning pipeline에 넣어 rule-based reward, R1-Reward, BaseReward, hybrid reward를 비교한다.

## Results

OpenReview 최종본 기준 BaseReward는 MM-RLHF-Reward Bench accuracy에서 이전 SOTA 대비 +11.9%를 보고하고, Acc+에서는 Claude 3.7 Sonnet 대비 +23.32% 개선을 보고한다. VL-Reward Bench에서도 강한 성능을 보이며, Multimodal Reward Bench에서는 coding/safety 관련 preference data 부족 때문에 R1-Reward류보다 일부 항목이 약하다고 분석한다.

RL pipeline에서는 BaseReward가 R1-Reward보다 계산 효율적이며, rule-based reward와 BaseReward를 결합한 hybrid reward가 perception, reasoning, conversation benchmark에서 가장 안정적인 개선을 낸다.

## Discussion

이 논문은 data selection보다는 reward data curation recipe에 가깝다. 그래도 이번 ICLR 2026 MLLM data 묶음에서 중요한 이유는, MLLM post-training의 품질이 "어떤 preference data를 섞고 어떤 것은 빼는가"에 크게 좌우됨을 실험적으로 보여주기 때문이다.

특히 text-only preference data가 multimodal reward benchmark에도 도움이 될 수 있다는 점이 중요하다. 반대로 모든 multimodal preference dataset이 도움이 되는 것은 아니며, MMIF나 SHP처럼 이득이 작거나 부정적인 데이터도 보고된다.

## Limitations

- Coding, safety/bias처럼 현재 multimodal preference data가 부족한 영역에서는 Naive-RM의 한계가 드러난다.
- Static reward benchmark 성능이 실제 RL 유용성을 완전히 대변하지 않는다.
- Preference dataset의 length bias와 domain imbalance를 신중히 다뤄야 한다.
- 최종 recipe는 강한 Qwen2.5-VL backbone과 선별된 데이터에 의존한다.

## References

- arXiv: <https://arxiv.org/abs/2509.16127>
- OpenReview: <https://openreview.net/forum?id=EuN5iszF0a>
- AlphaXiv: <https://www.alphaxiv.org/?organizations=NLPR%2C+CASIA>
