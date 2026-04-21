# CoIDO: Efficient Data Selection for Visual Instruction Tuning via Coupled Importance-Diversity Optimization

- **Paper**: CoIDO: Efficient Data Selection for Visual Instruction Tuning via Coupled Importance-Diversity Optimization
- **arXiv**: [2510.17847v1](https://arxiv.org/abs/2510.17847v1)
- **Authors**: Yichen Yan, Ming Zhong, Qi Zhu, Xiaoling Gu, Jinpeng Chen, Huan Li
- **Venue**: NeurIPS 2025
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, visual-instruction-tuning, vlm, importance-diversity, scorer

## One-line Summary

CoIDO는 중요도와 다양성을 따로 계산해 후처리로 섞는 대신, 작은 random subset으로 학습한 lightweight scorer가 두 목표를 coupled objective로 함께 최적화하도록 만드는 VIT 데이터 선택 방법이다.

## Key Idea

기존 data selection은 중요한 샘플을 찾는 scoring과 중복을 줄이는 diversity filtering을 분리하는 경우가 많다. CoIDO는 이 분리가 suboptimal하다고 보고, importance와 diversity를 하나의 dual-objective 학습 문제로 묶는다.

분류상 CoIDO는 **offline / model-involved 또는 proxy-scorer / learned scorer / importance-diversity coupling**에 가깝다. 전체 candidate set에 대해 비싼 평가를 반복하지 않고, 일부 sample로 scorer를 학습한 뒤 전체 데이터에 적용한다.

## Method

CoIDO의 핵심 구성은 다음과 같다.

- 작은 random subset, 논문 기준 20%, 로 lightweight plug-in scorer를 학습한다.
- scorer는 candidate distribution을 학습하고, 이후 전체 데이터셋에 대해 selection score를 예측한다.
- objective는 data importance와 diversity를 동시에 고려한다.
- homoscedastic uncertainty 기반 formulation으로 두 목표 사이의 가중치를 자동 조절한다.
- 최종적으로 전체 데이터 중 일정 비율, 논문 핵심 설정에서는 20%, 를 골라 instruction tuning에 사용한다.

## Experiments

주요 실험은 LLaVA-1.5-7B visual instruction tuning에서 진행된다. CoIDO scorer는 전체 데이터가 아니라 20% random sample로 먼저 학습되고, 그 뒤 전체 candidate pool에서 20% subset을 선택한다.

평가는 10개 downstream task 평균 성능을 full-data fine-tuning과 비교하는 방식이다.

## Results

논문 초록 기준, CoIDO가 고른 20% subset으로 LLaVA-1.5-7B를 instruction tuning했을 때 full-data fine-tuning 평균 성능의 98.2%를 달성한다. 핵심 주장은 적은 scorer 학습 비용으로도 full-data에 근접한 성능과 높은 selection efficiency를 얻는다는 것이다.

## Discussion

CoIDO는 "selection objective 설계" 측면에서 비교하기 좋다. ScalSelect가 global subspace preservation, DataTailor가 informativeness/uniqueness/representativeness, ICONS가 cross-task influence consensus를 강조한다면, CoIDO는 importance-diversity coupling 자체가 기여다.

주의할 점은 완전한 training-free 방법은 아니라는 것이다. scorer를 small random sample에서 학습해야 하므로, PRISM/ScalSelect류의 forward-only 또는 representation-only selection과 계산 프로파일이 다르다.

## Limitations

- scorer 학습용 random subset이 전체 분포를 충분히 대표하지 못하면 selection이 흔들릴 수 있다.
- scorer architecture와 feature choice가 selection 성능에 미치는 영향이 중요하다.
- diversity objective가 실제 downstream skill diversity와 얼마나 잘 맞는지 별도 점검이 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2510.17847>
- arXiv HTML: <https://arxiv.org/html/2510.17847>
- OpenReview: <https://openreview.net/forum?id=pXXKKBx8G0>
- Code: <https://github.com/SuDIS-ZJU/CoIDO>
