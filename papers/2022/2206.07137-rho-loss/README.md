# Prioritized Training on Points that are Learnable, Worth Learning, and Not Yet Learnt

- **Paper**: Prioritized Training on Points that are Learnable, Worth Learning, and Not Yet Learnt
- **arXiv**: [2206.07137v3](https://arxiv.org/abs/2206.07137v3)
- **Authors**: Soren Mindermann, Jan Brauner, Muhammed Razzak, Mrinank Sharma, Andreas Kirsch, Winnie Xu, Benedikt Hoeltgen, Aidan N. Gomez, Adrien Morisot, Sebastian Farquhar, Yarin Gal
- **Venue**: ICML 2022
- **Read date**: 2026-04-21
- **Tags**: data-selection, rho-loss, prioritized-training, curriculum, active-learning

## One-line Summary

RHO-Loss는 현재 모델이 아직 배우지 못했지만 holdout/downstream model 기준으로는 learnable하고 worth learning인 샘플을 reducible holdout loss로 골라 training을 가속하는 prioritized training 방법이다.

## Key Idea

어려운 샘플을 고르는 high-loss selection은 noisy 또는 unlearnable samples를 과도하게 뽑을 수 있고, 쉬운 샘플 curriculum은 이미 학습된 샘플에 compute를 낭비할 수 있다. RHO-Loss는 현재 모델 loss에서 irreducible 또는 이미 설명 가능한 부분을 빼서 "지금 학습하면 일반화 loss를 줄일 샘플"을 고르려 한다.

## Method

RHO-Loss의 기본 score는 reducible holdout loss다.

- 현재 모델이 샘플에서 내는 loss를 계산한다.
- downstream/holdout model 또는 irreducible loss estimator가 설명하는 loss를 뺀다.
- score가 높은 샘플을 우선 학습한다.

PDS 논문에서는 RHO-Loss baseline을 downstream task인 LIMA에 맞춘 모델과 current model의 loss 차이로 구현한다.

## Experiments

원 논문은 MLP, CNN, BERT 등 다양한 model/dataset에서 prioritized training 효과를 평가한다. Clothing-1M 같은 noisy web-scraped image dataset에서도 학습 step을 크게 줄이는 결과를 보고한다.

## Results

arXiv 초록 기준, RHO-Loss는 여러 dataset, hyperparameter, architecture에서 accuracy를 개선하고 training step을 줄인다. Clothing-1M에서는 uniform shuffling 대비 18배 적은 step으로 학습하며 최종 accuracy도 2% 높다고 보고한다.

## Discussion

사용자 메모처럼 RHO-Loss는 적용이 비교적 쉽다. MLLM VIT에서도 current model loss와 reference/downstream loss를 정의하면 score를 만들 수 있다. 다만 image-text instruction data에서는 "worth learning"을 어떤 holdout 또는 target distribution으로 정의할지가 핵심이다.

ScalSelect/PRISM 같은 training-free 방법과 비교하면 RHO-Loss는 online/prioritized training 성격이 강하고, selection 중 모델 loss를 계속 본다는 점이 다르다.

## Limitations

- holdout/downstream model 또는 irreducible loss estimator가 필요하다.
- noisy high-loss와 useful high-loss를 분리하는 데 실패하면 선택 품질이 떨어진다.
- static offline subset selection이라기보다 training 중 우선순위 조정에 더 자연스럽다.

## References

- arXiv: <https://arxiv.org/abs/2206.07137>
