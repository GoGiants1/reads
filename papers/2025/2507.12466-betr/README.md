# Language Models Improve When Pretraining Data Matches Target Tasks

- **Paper**: Language Models Improve When Pretraining Data Matches Target Tasks
- **arXiv**: [2507.12466v1](https://arxiv.org/abs/2507.12466v1)
- **Authors**: David Mizrahi, Anders Boesen Lindbo Larsen, Jesse Allardice, Suzie Petryk, Yuri Gorokhov, Jeffrey Li, Alex Fang, Josh Gardner, Tom Gunter, Afshin Dehghan
- **Venue**: arXiv 2025
- **Read date**: 2026-04-21
- **Tags**: data-selection, pretraining, benchmark-targeting, scaling-laws, decontamination

## One-line Summary

BETR은 benchmark training examples와 pretraining documents의 embedding similarity를 이용해 target task에 맞는 pretraining data를 고르고, scaling law 실험으로 benchmark-targeted selection의 효과를 보인 논문이다.

## Key Idea

논문은 모든 data selection method에는 암묵적 target이 있다고 본다. 기존에는 benchmark 성능을 보고 selection rule을 반복 조정하지만, BETR은 이 target을 명시화한다. benchmark의 training examples와 유사한 pretraining 문서를 더 많이 학습하면 해당 benchmark 능력이 더 잘 형성된다는 가설이다.

사용자 메모의 핵심인 "benchmark target data selection"에 가장 직접적으로 맞는 논문이다. 특히 test set과의 decontamination을 강조하며, LAMBADA처럼 training set이 없는 경우 test set 일부를 targeting에 쓰고 나머지를 평가에 쓰는 방식까지 논의한다.

## Method

- benchmark examples와 pretraining document sample을 같은 embedding space에 넣는다.
- benchmark examples와의 similarity로 sampled documents에 target score를 부여한다.
- lightweight classifier를 학습해 전체 corpus의 target similarity score를 예측한다.
- score에 따라 pretraining documents를 선택하거나 reweighting한다.

## Experiments

500개 이상의 모델을 학습하고, 10^19에서 10^22 FLOPs 범위에서 scaling law를 맞춘다. DCLM-Baseline 및 unfiltered data와 비교하며, target benchmark와 disjoint한 benchmark set에 대한 generalization도 평가한다.

## Results

초록 기준, BETR은 DCLM-Baseline 대비 2.1x, unfiltered data 대비 4.7x compute multiplier를 보이고, 10개 task 중 9개에서 모든 scale에 걸쳐 개선된다. 또한 disjoint target set을 사용할 때도 baseline과 같거나 더 나은 결과를 낸다. 큰 모델일수록 덜 공격적인 filtering이 적합하다는 scale-dependent 경향도 보고한다.

## Discussion

이 논문은 benchmark-aligned pretraining data가 모델 capability를 직접 형성한다는 강한 근거를 제공한다. LESS, DsDm, TAROT이 target-aware finetuning/selection 쪽이라면, BETR은 pretraining corpus 자체를 benchmark train distribution에 맞춘다.

주의할 점은 benchmark leakage와 evaluation integrity다. 논문이 n-gram decontamination과 train/test 분리를 강조하는 이유도 여기에 있다.

## Limitations

- benchmark training examples가 target을 대표하지 못하면 selection도 좁아질 수 있다.
- benchmark에 직접 맞추는 전략은 평가 무결성 논쟁을 피하기 어렵다.
- embedding similarity와 classifier proxy가 실제 pretraining contribution을 완전히 설명하지는 않는다.

## References

- arXiv: <https://arxiv.org/abs/2507.12466>
- Apple ML Research: <https://machinelearning.apple.com/research/pretraining-data-matches>
- Hugging Face Papers: <https://huggingface.co/papers/2507.12466>
