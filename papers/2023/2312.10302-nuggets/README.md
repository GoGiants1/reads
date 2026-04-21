# One-Shot Learning as Instruction Data Prospector for Large Language Models

- **Paper**: One-Shot Learning as Instruction Data Prospector for Large Language Models
- **arXiv**: [2312.10302v4](https://arxiv.org/abs/2312.10302v4)
- **Authors**: Yunshui Li, Binyuan Hui, Xiaobo Xia, Jiaxi Yang, Min Yang, Lei Zhang, Shuzheng Si, Ling-Hao Chen, Junhao Liu, Tongliang Liu, Fei Huang, Yongbin Li
- **Venue**: ACL 2024
- **Read date**: 2026-04-21
- **Tags**: data-selection, instruction-tuning, one-shot-learning, llm, target-benchmark

## One-line Summary

Nuggets는 후보 instruction이 anchor set에서 one-shot example로 작동할 때 perplexity를 얼마나 낮추는지 측정해, 전체 instruction corpus 중 소수의 고가치 데이터를 고르는 방법이다.

## Key Idea

논문은 instruction tuning에서 데이터 양을 늘리는 것보다 "한 예시가 다른 예시를 잘 설명하게 만드는가"가 중요하다고 본다. 후보 데이터를 직접 학습하지 않고 one-shot demonstration으로 투입해 anchor examples의 perplexity 변화를 측정하면, 해당 후보가 instruction-following 학습에 유용한지 빠르게 추정할 수 있다.

사용자 메모의 관점에서는 golden/anchor set을 사실상 benchmark target으로 쓰는 계열이다. 명시적으로 test set을 겨냥한다기보다는, anchor set과 잘 맞는 instruction을 골라 특정 평가 능력을 끌어올린다.

## Method

- 다양한 task를 대표하는 anchor set을 준비한다.
- 각 후보 instruction을 one-shot example로 넣고 anchor response의 perplexity 변화를 계산한다.
- anchor perplexity를 크게 낮추는 후보를 높은 가치의 "Nugget"으로 본다.
- 상위 소수 데이터를 골라 LLM instruction tuning에 사용한다.

## Experiments

MT-Bench와 AlpacaEval을 포함한 instruction-following benchmark에서, 전체 데이터와 Nuggets가 고른 작은 subset을 비교한다. 핵심 비교는 전체 데이터 사용 대비 상위 1% 수준의 selected data가 얼마나 성능을 보존하거나 향상시키는지다.

## Results

arXiv 초록 기준, Nuggets가 고른 상위 1% instruction만으로 학습해도 전체 dataset을 쓰는 통상적 방법을 상당히 능가한다. 이는 instruction data selection에서 "대표 anchor에 대한 one-shot 유용성"이 강한 선택 신호가 될 수 있음을 보인다.

## Discussion

이 논문은 LESS나 BETR처럼 target examples를 명시적으로 활용하는 흐름과 연결된다. 다만 gradient influence나 embedding similarity가 아니라, 후보 예시를 직접 demonstration으로 넣었을 때 validation/anchor loss가 어떻게 바뀌는지를 본다는 점이 다르다.

연구 아이디어 측면에서는 "benchmark training/golden set을 target으로 삼아 selection score를 만들 수 있다"는 근거로 유용하다. 반면 online/dynamic selection이라기보다는 학습 전에 수행하는 offline scoring에 가깝다.

## Limitations

- anchor set 품질과 다양성에 민감하다.
- one-shot demonstration 효과가 실제 fine-tuning 후 영향과 항상 일치한다고 보장되지는 않는다.
- 후보마다 perplexity를 계산해야 하므로 대규모 corpus에서는 scoring 비용이 남는다.

## References

- arXiv: <https://arxiv.org/abs/2312.10302>
- arXiv HTML: <https://arxiv.org/html/2312.10302>
- ACL Anthology: <https://aclanthology.org/2024.acl-long.252/>
