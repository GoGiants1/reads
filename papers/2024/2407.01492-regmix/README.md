# RegMix: Data Mixture as Regression for Language Model Pre-training

- **Paper**: RegMix: Data Mixture as Regression for Language Model Pre-training
- **arXiv**: [2407.01492v2](https://arxiv.org/abs/2407.01492v2)
- **Authors**: Qian Liu, Xiaosen Zheng, Niklas Muennighoff, Guangtao Zeng, Longxu Dou, Tianyu Pang, Jing Jiang, Min Lin
- **Venue**: ICLR 2025
- **Read date**: 2026-04-21
- **Tags**: data-mixture, pretraining, regression, proxy-models, scaling

## One-line Summary

RegMix는 다양한 data mixture로 많은 작은 모델을 학습한 뒤, mixture와 성능 사이의 regression model을 학습해 큰 LM에 적용할 고성능 mixture를 예측하는 pretraining data mixture 방법이다.

## Key Idea

좋은 pretraining mixture는 직관적으로 정하기 어렵고 domain interaction도 복잡하다. RegMix는 이를 탐색 문제로 보고, 많은 small-scale mixture experiments에서 얻은 성능 데이터를 이용해 unseen mixture의 성능을 예측한다.

## Method

- 여러 후보 data mixture를 샘플링한다.
- 각 mixture로 많은 작은 모델을 학습한다.
- mixture weights를 입력, validation/downstream performance를 출력으로 하는 regression model을 학습한다.
- regression model이 예측한 best mixture로 큰 모델을 학습한다.

## Experiments

1M parameter 모델 512개를 1B tokens로 학습해 regression model을 맞추고, 예측 mixture로 1B parameter 모델을 25B tokens 학습한다. 추가로 최대 7B parameter, 100B token 실험에서 human mixture와 DoReMi를 비교한다.

## Results

arXiv 초록 기준, RegMix가 예측한 mixture는 64개 candidate 1B model 중 최고 성능을 내며, 최대 7B scale에서도 human selection을 일관되게 능가한다. DoReMi와 같거나 더 좋은 성능을 10% compute로 달성한다고 보고한다.

## Discussion

RegMix는 TiKMiX/OPUS와 비교할 때 offline mixture search 계열이다. OPUS처럼 매 iteration sample을 고르지는 않지만, "domain mixture effect가 scaling law를 단순히 따르지 않고 복잡하게 상호작용한다"는 실험적 근거를 제공한다.

OPUS 연구에서는 static/offline mixture optimization baseline 또는 pretraining mixture 설계 배경으로 유용하다.

## Limitations

- 많은 small model pretraining runs가 필요하다.
- small-scale regression이 큰 모델/긴 token budget에 얼마나 안정적으로 transfer되는지가 핵심 가정이다.
- mixture-level 방법이므로 sample-level dynamic utility는 직접 다루지 않는다.

## References

- arXiv: <https://arxiv.org/abs/2407.01492>
- Code: <https://github.com/sail-sg/regmix>
