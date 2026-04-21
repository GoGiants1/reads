# TAROT: Targeted Data Selection via Optimal Transport

- **Paper**: TAROT: Targeted Data Selection via Optimal Transport
- **arXiv**: [2412.00420v2](https://arxiv.org/abs/2412.00420v2)
- **Authors**: Lan Feng, Fan Nie, Yuejiang Liu, Alexandre Alahi
- **Venue**: ICML 2025
- **Read date**: 2026-04-21
- **Tags**: data-selection, targeted-selection, optimal-transport, distribution-matching, influence

## One-line Summary

TAROT은 target domain과 selected data 사이의 whitened feature 기반 optimal transport distance를 최소화해, multimodal target distribution에 맞는 데이터를 고르는 targeted data selection 방법이다.

## Key Idea

기존 targeted data selection은 influence score를 greedy하게 더하는 방식이 많다. 논문은 이 방식이 target distribution이 여러 mode를 가질 때 약하다고 본다. dominant feature component가 influence estimation을 왜곡하고, greedy linear additive assumption이 복합 target pattern을 제대로 반영하지 못하기 때문이다.

TAROT은 target domain과 selection subset 사이의 distribution matching 문제로 바꾸고, optimal transport로 selection ratio까지 추정한다.

## Method

- feature space에서 dominant component bias를 줄이기 위해 whitened feature distance를 사용한다.
- 후보 데이터와 target data 사이의 거리를 optimal transport 관점에서 계산한다.
- selected subset이 target distribution을 잘 덮도록 OT distance를 최소화한다.
- 이 과정에서 각 source group 또는 sample의 optimal selection ratio를 추정한다.

## Experiments

semantic segmentation, motion prediction, instruction tuning 등 여러 task에서 평가한다. 비교 대상은 influence-based targeted selection과 coreset/distribution matching 계열 방법이다.

## Results

PMLR/OpenReview 초록 기준, TAROT은 여러 deep learning task에서 기존 state-of-the-art보다 일관되게 좋은 성능을 보인다. 특히 target distribution이 복잡하거나 multimodal일 때 greedy influence selection의 약점을 보완한다.

## Discussion

TAROT은 benchmark target data selection을 "target examples를 어떻게 대표할 것인가"라는 문제로 읽게 해준다. LESS처럼 target few-shot gradient와 가까운 sample을 고르는 방식은 local similarity에 강하지만, TAROT은 target distribution 전체의 coverage를 본다.

LLM finetuning에서는 target benchmark가 단일 skill이 아니라 여러 하위 mode를 포함할 때 유용한 관점이다.

## Limitations

- feature representation 품질과 whitening 안정성에 민감하다.
- OT 계산과 selection ratio 추정이 대규모 corpus에서 비용 문제가 될 수 있다.
- LLM instruction data에서는 feature distance가 semantic/task alignment를 충분히 반영하는지 별도 검증이 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2412.00420>
- PMLR: <https://proceedings.mlr.press/v267/feng25l.html>
- OpenReview: <https://openreview.net/forum?id=EznrK7QWgK>
- Code: <https://github.com/vita-epfl/TAROT>
