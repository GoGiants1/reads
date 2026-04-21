# DataRater: Meta-Learned Dataset Curation

- **Paper**: DataRater: Meta-Learned Dataset Curation
- **arXiv**: [2505.17895v2](https://arxiv.org/abs/2505.17895v2)
- **Authors**: Dan A. Calian, Gregory Farquhar, Iurii Kemaev, Luisa M. Zintgraf, Matteo Hessel, Jeremy Shar, Junhyuk Oh, András György, Tom Schaul, Jeffrey Dean, Hado van Hasselt, David Silver
- **Venue**: NeurIPS 2025
- **Read date**: 2026-04-21
- **Tags**: data-curation, meta-learning, meta-gradients, foundation-models, data-selection

## One-line Summary

DataRater는 held-out data에서 training efficiency를 높이도록 meta-gradient로 개별 datapoint의 가치를 학습하고, 이를 이용해 foundation model training data를 fine-grained하게 선별하는 meta-learned curation 방법이다.

## Key Idea

기존 dataset curation은 coarse bucket mixture를 수동 튜닝하거나 heuristic filtering에 의존한다. DataRater는 "어떤 데이터가 실제 학습 효율을 높이는가"를 meta-learning으로 직접 학습한다.

개별 datapoint의 value를 추정하므로, 사람이 정의한 품질 지표보다 더 세밀한 selection을 목표로 한다.

## Method

- training process가 held-out objective를 얼마나 개선하는지 meta-gradient로 추적한다.
- DataRater가 각 datapoint의 training value를 예측하도록 학습한다.
- 예측 value를 기준으로 filtering 또는 weighting을 수행한다.
- 여러 model scale과 dataset에서 curated subset의 compute efficiency를 평가한다.

## Experiments

foundation model training setting에서 다양한 model scale과 dataset에 걸쳐 DataRater filtering을 평가한다. manual curation 또는 heuristic filter 대비 compute efficiency 개선이 핵심 지표다.

## Results

arXiv/AlphaXiv 요약 기준, DataRater로 filtering한 데이터는 여러 scale에서 학습 효율을 크게 높인다. NeurIPS 2025 채택 논문으로, meta-gradient 기반 dataset curation을 대규모 foundation model setting에 적용한 점이 중요하다.

## Discussion

이 논문은 사용자의 "실험 구성, figure 등 참고" 메모처럼 방법 자체뿐 아니라 실험 설계 참고 가치가 크다. target benchmark selection과 달리 broad held-out objective를 사용하지만, "데이터 가치 함수를 학습한다"는 점에서 dynamic/targeted selection 모두와 연결된다.

MATES가 현재 model state의 influence를 근사한다면, DataRater는 meta-gradient로 학습 효율을 직접 objective화한다.

## Limitations

- meta-gradient 계산과 학습 파이프라인이 복잡하고 비용이 높다.
- held-out objective 선택에 따라 DataRater가 학습하는 value가 달라진다.
- value model이 특정 scale/dataset에 overfit될 수 있어 transfer 검증이 중요하다.

## References

- arXiv: <https://arxiv.org/abs/2505.17895>
- AlphaXiv: <https://www.alphaxiv.org/abs/2505.17895>
- NeurIPS 2025 papers: <https://neurips.cc/virtual/2025/papers.html?filter=title>
