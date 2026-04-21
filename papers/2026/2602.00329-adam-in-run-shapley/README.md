# In-Run Data Shapley for Adam Optimizer

- **Paper**: In-Run Data Shapley for Adam Optimizer
- **arXiv**: [2602.00329v3](https://arxiv.org/abs/2602.00329v3)
- **Authors**: Meng Ding, Zeqing Zhang, Di Wang, Lijie Hu
- **Venue**: DATA-FM Workshop @ ICLR 2026
- **Read date**: 2026-04-21
- **Tags**: data-attribution, shapley-value, adam, optimizer-aware, in-run

## One-line Summary

이 논문은 SGD 가정에 의존한 기존 In-Run Data Shapley가 Adam optimizer에서는 실제 기여도와 크게 어긋난다고 보이고, Adam-aware utility와 Linearized Ghost Approximation으로 학습 중 데이터 기여도를 효율적으로 추정한다.

## Key Idea

Data Shapley는 데이터 기여도 평가의 이론적 gold standard지만, subset retraining 비용 때문에 대규모 학습에는 직접 쓰기 어렵다. In-Run methods는 학습 중 gradient 정보를 이용해 Shapley를 근사하지만, 기존 방법은 SGD의 선형 구조에 강하게 의존한다.

이 논문은 data value가 dataset 자체의 고정 속성이 아니라 optimizer trajectory와 결합된 값이라고 주장한다. Adam에서는 momentum과 variance scaling 때문에 SGD 기반 proxy가 true marginal contribution과 거의 맞지 않는다.

## Method

- Adam update 아래에서 per-sample utility를 fixed-state assumption으로 재정의한다.
- adaptive variance term을 Taylor expansion으로 선형화해 additivity를 복원한다.
- Linearized Ghost Approximation으로 per-sample gradient를 직접 저장하지 않고 pairwise gradient dot-product를 계산한다.
- 학습 중 얻는 gradient/moment 정보를 사용해 Adam-aware In-Run Data Shapley를 추정한다.

## Experiments

ground-truth marginal contribution과의 correlation, throughput, data pruning, source identification downstream tasks를 평가한다. 비교 대상은 SGD 기반 In-Run Shapley/attribution proxy 및 naive Adam-aware implementation이다.

## Results

arXiv/AlphaXiv 기준, SGD-based proxy는 Adam 아래 true contribution과 Pearson R이 약 0.11에 그쳐 거의 쓸 수 없고, 제안 방법은 ground-truth marginal contribution과 R > 0.99의 높은 fidelity를 보인다. Linearized Ghost는 naive implementation보다 훨씬 빠르며, standard training throughput의 약 95%를 유지한다.

downstream attribution task에서도 Adam-aware attribution은 SGD 기반 baseline보다 data pruning validation accuracy와 semantic source identification robustness에서 더 좋은 결과를 보인다.

## Discussion

OPUS가 optimizer-induced update space에서 data selection utility를 정의했다면, 이 논문은 Shapley attribution도 optimizer-aware해야 한다는 같은 방향의 근거를 제공한다. 데이터 선택/영향도 추정에서 Adam을 단순 diagonal rescaling 정도로만 처리하면 실제 학습 기여도를 놓칠 수 있다.

사용자 연구 맥락에서는 "dynamic selection score는 optimizer와 분리해서 정의하면 안 된다"는 강한 supporting reference로 쓸 수 있다.

## Limitations

- fixed-state assumption과 linearization은 Adam dynamics를 근사하므로 long-horizon interaction을 완전히 포착하지는 않는다.
- Shapley attribution fidelity가 곧바로 최적 data selection policy를 의미하지는 않는다.
- workshop/preprint 성격이므로 더 큰 LLM pretraining setting에서의 검증은 추가 확인이 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2602.00329>
- AlphaXiv: <https://www.alphaxiv.org/abs/2602.00329>
- arXiv HTML: <https://arxiv.org/html/2602.00329>
