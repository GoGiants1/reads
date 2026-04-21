# ScalSelect: Scalable Training-Free Multimodal Data Selection for Efficient Visual Instruction Tuning

- **Paper**: ScalSelect: Scalable Training-Free Multimodal Data Selection for Efficient Visual Instruction Tuning
- **arXiv**: [2602.11636v1](https://arxiv.org/abs/2602.11636v1)
- **Authors**: Changti Wu, Jiahuai Mao, Yuzhuo Miao, Shijie Lian, Bin Yu, Xiaopeng Lin, Cong Huang, Lei Zhang, Kai Chen
- **Venue**: arXiv 2026
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, visual-instruction-tuning, vlm, training-free, subspace-selection

## One-line Summary

ScalSelect는 target VLM의 early-layer instruction-conditioned visual representation을 만들고, 전체 데이터 표현의 dominant subspace를 잘 보존하는 샘플을 leverage score로 고르는 offline training-free VIT 데이터 선택 방법이다.

## Key Idea

기존 multimodal data selection은 proxy model, warm-up training, gradient, pairwise similarity, instruction-agnostic representation에 의존하는 경우가 많다. ScalSelect의 핵심 전환은 샘플 간 local distance를 보지 않고, 전체 데이터 representation matrix의 global low-rank subspace를 얼마나 잘 보존하는지를 본다는 점이다.

이 방법은 target VLM 내부 정보는 쓰지만 추가 학습이나 backpropagation은 쓰지 않는다. 따라서 분류하면 **offline / model-involved / target-model forward-only / training-free** 쪽에 가깝다.

## Method

ScalSelect는 두 단계로 구성된다.

1. **Instruction-Conditioned Early Representation**
   - 각 샘플의 이미지와 instruction을 target VLM에 넣고, LLM의 첫 transformer layer에서 attention을 본다.
   - user instruction token들이 어떤 visual token에 attention을 주는지 집계한다.
   - attention mass가 큰 visual token만 남기고, 해당 token들의 first-layer hidden state를 평균내 sample representation으로 사용한다.
   - 같은 이미지라도 instruction에 따라 중요한 visual region이 달라진다는 점을 representation에 반영하려는 설계다.

2. **Subspace-Aware Global Selection**
   - 모든 sample representation을 행렬로 쌓고 column-wise centering을 수행한다.
   - truncated SVD로 dominant low-rank subspace를 추정한다.
   - 각 sample row가 dominant subspace에 기여하는 정도를 leverage score로 계산한다.
   - 높은 leverage score를 가진 샘플을 선택해, full dataset representation space를 잘 근사하는 subset을 만든다.

이 관점에서는 data selection이 "중복이 큰 full representation matrix를 작은 row subset으로 압축하는 문제"가 된다. pairwise similarity matrix를 만들 필요가 없어 샘플 수에 대해 더 잘 스케일한다.

## Experiments

주요 실험은 LLaVA 계열 VIT 데이터에서 진행된다.

- **Training datasets**: LLaVA-V-625K, LRV-Sub-180K
- **Models**: LLaVA-Vicuna-7B, Qwen3-VL-4B-Instruct, Qwen3-VL-8B-Instruct
- **Benchmarks**: MMBench-En, MMBench-Cn, MME-P, MME-C, AI2D, POPE-A, POPE-P, SQA-IMG, OCRBench
- **Baselines**: Random, Length, Perplexity, COINCIDE, PRISM, RDS+, Full-Finetune

비교는 주로 추가 학습 없이 적용 가능한 strong training-free baselines에 맞춰져 있다. 그래서 gradient-heavy 방법이나 warm-up training 기반 방법을 모두 포함하는 "전체 방법론 지도"라기보다는, training-free selection 내부 비교 성격이 강하다.

## Results

LLaVA-V-625K에서 100K samples, 즉 약 16% budget으로 ScalSelect는 Full-Finetune 대비 평균 상대 성능 97.85%를 달성한다. 같은 100K 조건에서 COINCIDE 96.85%, PRISM 96.01%, RDS+ 95.71%, Random 95.66%보다 높다.

선택 budget을 늘리면 성능은 대체로 안정적으로 증가한다. 50K에서는 95.34%, 200K에서는 98.67%, 300K에서는 99.66%, 400K에서는 101.16% relative performance를 보고한다. 즉 일부 예산에서는 full-data fine-tuning을 넘는 결과도 나온다.

Qwen3-VL 계열에서는 100K selected subset이 full fine-tuning보다 더 높은 상대 성능을 보인다. 논문은 Qwen3-VL-4B-Instruct에서 104.00%, Qwen3-VL-8B-Instruct에서 102.39% relative performance를 보고한다.

## Discussion

ScalSelect는 "학습 없이 빠르게 고른다"는 주장 안에서도 target model의 내부 attention/hidden state를 적극적으로 쓰는 점이 중요하다. 완전한 model-free selector는 아니고, target VLM의 forward pass와 내부 activation 접근이 필요하다.

비교 연구 관점에서는 다음 축에서 유용하다.

- COINCIDE: small reference LVLM + clustering/transferability
- PRISM: training-free이지만 similarity/correlation 기반
- ICONS/TIVE/LESS: gradient 또는 influence 계열
- DataTailor: informativeness, uniqueness, representativeness의 다기준 조합

ScalSelect의 차별점은 "instruction-aware representation"과 "pairwise diversity가 아니라 global subspace preservation"의 결합이다. 특히 OCRBench에서 강한 결과를 보이는 점은 instruction-conditioned visual token selection이 국소적 시각 정보가 중요한 task에 잘 맞을 수 있음을 시사한다.

## Limitations

- target VLM의 attention map과 hidden state에 접근해야 하므로 black-box API 모델에는 바로 적용하기 어렵다.
- forward-only라고 해도 전체 candidate pool을 target VLM에 통과시켜야 하므로, 매우 큰 데이터셋에서는 activation extraction 비용과 저장 비용이 남는다.
- SVD 기반 subspace 추정의 실제 메모리/streaming 구현 세부가 대규모 운영에서 중요해진다.
- 평가가 주로 LLaVA/Qwen3-VL류와 표준 VLM benchmark에 집중되어 있어, domain-specific VIT나 continual setting으로 바로 일반화되었는지는 별도 확인이 필요하다.
- selection budget, subspace rank, attention threshold 선택이 성능에 미치는 영향은 실제 재현에서 다시 점검해야 한다.

## References

- arXiv: <https://arxiv.org/abs/2602.11636>
- arXiv HTML: <https://arxiv.org/html/2602.11636>
- Code: <https://github.com/ChangtiWu/ScalSelect>
- alphaXiv: <https://www.alphaxiv.org/abs/2602.11636>
