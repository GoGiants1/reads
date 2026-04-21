# Data Selection for Language Models via Importance Resampling

- **Paper**: Data Selection for Language Models via Importance Resampling
- **arXiv**: [2302.03169v3](https://arxiv.org/abs/2302.03169v3)
- **Authors**: Sang Michael Xie, Shibani Santurkar, Tengyu Ma, Percy Liang
- **Venue**: NeurIPS 2023
- **Read date**: 2026-04-21
- **Tags**: data-selection, language-models, dsir, importance-resampling, pretraining

## One-line Summary

DSIR는 raw unlabeled corpus에서 target distribution과 가까운 pretraining subset을 고르기 위해, reduced feature space에서 importance weights를 추정하고 그 가중치로 resampling하는 LM data selection 방법이다.

## Key Idea

DSIR는 data selection을 "대규모 raw corpus에서 원하는 target distribution을 match하는 subset을 고르는 문제"로 공식화한다. feature space가 너무 크기 때문에 hashed n-gram features 같은 reduced representation을 사용해 importance resampling을 가능하게 만든다.

분류상 **offline / mostly model-free feature-based / distribution matching / LM pretraining selection**이다.

## Method

DSIR의 절차는 다음과 같다.

- raw corpus와 target samples를 같은 feature space로 변환한다.
- feature distribution 차이를 이용해 importance weight를 추정한다.
- estimated weight에 따라 raw corpus에서 resampling한다.
- 선택된 subset으로 LM을 pretrain 또는 continue pretrain한다.

논문 구현은 hashed n-gram features를 사용해 Pile 전체에서 100M documents를 4.5시간에 선택할 수 있음을 보인다.

## Experiments

general-domain LM과 domain-specific LM 설정에서 DSIR를 expert selection, random selection, heuristic filtering 등과 비교한다. target distribution은 downstream/domain-specific unlabeled samples로 주어진다.

## Results

arXiv 초록 기준, hashed n-gram feature에서의 KL reduction은 downstream accuracy와 높은 상관을 보이고, DSIR는 expert curation에 근접하거나 random/heuristic baselines보다 나은 성능을 달성한다.

## Discussion

사용자 메모처럼 DSIR는 MLLM VIT에 적용하기 어렵다. 이유는 target distribution과 feature representation을 어떻게 정의할지가 애매하기 때문이다.

- text-only LM: hashed n-gram target distribution이 자연스럽다.
- visual instruction tuning: image, instruction, answer, task skill, multimodal grounding을 모두 반영해야 한다.

그래서 DSIR는 "model-free/distribution matching baseline"의 좋은 개념적 대조군이지만, MLLM 비교에는 직접 구현보다 설명용 baseline에 가깝다.

## Limitations

- feature representation이 target quality를 잘 보존해야 한다.
- target samples가 필요하다.
- multimodal instruction data에서는 text n-gram만으로 visual/task distribution을 대표하기 어렵다.

## References

- arXiv: <https://arxiv.org/abs/2302.03169>
- Code: <https://github.com/p-lambda/dsir>
