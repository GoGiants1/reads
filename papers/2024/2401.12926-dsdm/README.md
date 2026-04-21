# DsDm: Model-Aware Dataset Selection with Datamodels

- **Paper**: DsDm: Model-Aware Dataset Selection with Datamodels
- **arXiv**: [2401.12926v1](https://arxiv.org/abs/2401.12926v1)
- **Authors**: Logan Engstrom, Axel Feldmann, Aleksander Madry
- **Venue**: ICML 2024 Spotlight
- **Read date**: 2026-04-21
- **Tags**: data-selection, datamodels, model-aware-selection, llm, target-tasks

## One-line Summary

DsDm은 사람 기준의 "좋은 데이터" 필터링 대신, target tasks에서 모델 성능을 최대화하는 subset을 datamodel로 직접 예측해 선택하는 model-aware dataset selection 방법이다.

## Key Idea

논문은 고품질 source와 유사한 데이터를 고르는 직관적 필터링이 실제 LLM 성능을 올리지 못하거나 오히려 해칠 수 있음을 지적한다. 따라서 데이터 선택 문제를 "후보 데이터, 학습 알고리즘, target task가 주어졌을 때 어떤 subset이 성능을 최대화하는가"라는 최적화 문제로 둔다.

핵심은 데이터 자체의 표면적 품질이 아니라, 특정 학습 절차를 거친 모델이 target example을 얼마나 잘 예측하게 만드는지다. 이 점에서 benchmark target data selection의 대표적인 model-aware 선행 연구다.

## Method

- 여러 random subset으로 모델을 학습해 target examples에서의 성능/예측을 관찰한다.
- 각 training datapoint의 포함 여부와 target behavior 사이 관계를 datamodel로 학습한다.
- 학습된 datamodel을 사용해 후보 데이터가 target tasks에 줄 영향을 예측한다.
- 예측 성능 향상이 큰 subset을 선택한다.

## Experiments

표준 LM 문제를 대표하는 target tasks를 잡고, 선택된 데이터로 학습한 모델을 다양한 held-out benchmark에서 평가한다. 비교 대상은 human-quality source similarity, random selection, 기존 필터링 방식이다.

## Results

arXiv 및 OpenReview 초록 기준, DsDm이 선택한 dataset은 baseline 대비 약 2x compute multiplier를 제공하고, 사전에 지정한 target뿐 아니라 unseen held-out benchmark에서도 성능 개선을 보인다.

## Discussion

DsDm은 "target benchmark를 직접 최적화 목표로 둘 수 있다"는 점을 정식화한다. LESS가 gradient similarity로 빠르게 target examples와 맞는 instruction을 찾는다면, DsDm은 datamodel을 통해 train datapoint와 target behavior의 관계를 학습한다.

비용은 높지만, target-aware selection의 이상적인 문제 정의를 제공한다는 점에서 중요하다. 후속 연구에서 더 싼 proxy, influence approximation, distribution matching으로 넘어가는 출발점으로 볼 수 있다.

## Limitations

- datamodel 학습을 위해 많은 subset training/evaluation이 필요해 비용이 크다.
- 선택 품질은 target tasks의 대표성에 강하게 의존한다.
- 실제 대규모 LLM pretraining 전체로 확장하려면 proxy modeling과 sampling 설계가 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2401.12926>
- OpenReview: <https://openreview.net/forum?id=GC8HkKeH8s>
- DBLP: <https://dblp.org/rec/conf/icml/EngstromFM24>
