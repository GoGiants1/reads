# Meta-rater: A Multi-dimensional Data Selection Method for Pre-training Language Models

- **Paper**: Meta-rater: A Multi-dimensional Data Selection Method for Pre-training Language Models
- **arXiv**: [2504.14194v4](https://arxiv.org/abs/2504.14194v4)
- **Authors**: Xinlin Zhuang, Jiahui Peng, Ren Ma, Yinfan Wang, Tianyi Bai, Xingjian Wei, Jiantao Qiu, Chi Zhang, Ying Qian, Conghui He
- **Venue**: ACL 2025 Best Theme Paper Award
- **Read date**: 2026-04-21
- **Tags**: data-selection, pretraining, multi-dimensional-quality, proxy-models, meta-rater

## One-line Summary

Meta-rater는 professionalism, readability, reasoning, cleanliness 등 여러 품질 지표와 기존 quality metrics를 proxy model validation loss 예측으로 통합해, LLM pretraining data selection을 위한 최적 가중치를 학습한다.

## Key Idea

단일 품질 지표만으로는 pretraining data의 유용성을 설명하기 어렵다. Meta-rater는 여러 품질 차원을 만들고, 이들의 조합이 validation loss에 어떤 영향을 주는지 작은 proxy model 실험으로 학습한다.

사용자 메모처럼 임의 가중치 조합으로 작은 모델을 학습하고 validation loss를 측정한 뒤, weight combination에서 expected validation loss를 예측하는 regressor를 학습해 최적 조합을 찾는 구조로 이해할 수 있다.

## Method

- PRRC: professionalism, readability, reasoning, cleanliness 네 가지 품질 차원을 정의한다.
- 기존 quality metrics와 PRRC 점수를 함께 사용한다.
- 여러 metric weight 조합으로 proxy model을 학습하고 validation loss를 수집한다.
- regression model이 weight 조합에서 expected validation loss를 예측하도록 학습한다.
- 예측 loss가 낮은 optimal weighting으로 data selection score를 구성한다.

## Experiments

1.3B parameter model과 3.3B/7.2B scale model에서 pretraining efficiency와 downstream task 성능을 평가한다. 100B token scale 실험도 포함한다.

## Results

arXiv 초록 기준, Meta-rater는 1.3B 모델의 convergence speed를 두 배로 높이고 downstream task 성능을 3.23 개선한다. 7.2B parameter scale까지 이점이 유지된다고 보고한다. ACL 2025 Best Theme Paper Award를 받았다.

## Discussion

Meta-rater는 target benchmark 하나를 직접 겨냥하기보다, pretraining data quality를 multi-dimensional하게 통합하는 방법이다. benchmark-target selection과 결합한다면, target similarity/influence score와 PRRC-like quality score를 어떻게 trade-off할지 정하는 meta-selection layer로 쓸 수 있다.

## Limitations

- proxy model 실험이 최종 large model behavior를 완벽히 대변하지 않을 수 있다.
- 품질 차원 정의가 language/domain별로 달라질 수 있다.
- validation loss 중심 최적화가 특정 downstream capability를 과소평가할 수 있다.

## References

- arXiv: <https://arxiv.org/abs/2504.14194>
- Code: <https://github.com/opendatalab/Meta-rater>
- ACL 2025 Awards: <https://2025.aclweb.org/program/awards/>
