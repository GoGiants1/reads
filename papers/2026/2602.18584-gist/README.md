# GIST: Targeted Data Selection for Instruction Tuning via Coupled Optimization Geometry

- **Paper**: GIST: Targeted Data Selection for Instruction Tuning via Coupled Optimization Geometry
- **arXiv**: [2602.18584v1](https://arxiv.org/abs/2602.18584v1)
- **Authors**: Guanghui Min, Tianhao Huang, Ke Wan, Chen Chen
- **Venue**: arXiv 2026
- **Read date**: 2026-04-21
- **Tags**: data-selection, targeted-instruction-tuning, lora, gradient-alignment, optimization-geometry

## One-line Summary

GIST는 LoRA 같은 PEFT에서 Adam-style diagonal preconditioning이 target update geometry를 잘 반영하지 못한다고 보고, validation gradients의 low-dimensional coupled subspace를 SVD로 복원해 training gradients를 그 공간에 투영해 선택한다.

## Key Idea

LESS 계열 targeted instruction selection은 gradient projection과 optimizer statistics를 활용하지만, 많은 방법이 update geometry를 axis-aligned diagonal scaling으로 근사한다. 논문은 LoRA/PEFT에서는 parameter 간 coupling과 off-diagonal interaction이 강하고, task-relevant directions는 낮은 차원 subspace에 놓인다고 주장한다.

따라서 target-aware selection은 coordinate-wise independence 가정이 아니라 task-specific subspace alignment로 처리해야 한다.

## Method

- target validation examples에서 gradients를 수집한다.
- spectral filtering/SVD로 task-specific low-dimensional subspace를 복원한다.
- candidate training gradients를 이 coupled subspace에 투영한다.
- target directions와의 alignment로 candidate examples를 score한다.
- 같은 selection budget에서 선택된 subset으로 instruction tuning을 수행한다.

## Experiments

MMLU, TyDiQA, BBH를 target으로 두고 Llama2-7B, Llama3.2-3B 등 여러 model architecture에서 5% data selection을 평가한다. Random, length, perplexity, embedding similarity, RDS+, LESS와 비교한다.

## Results

arXiv/AlphaXiv 기준, GIST는 state-of-the-art baseline인 LESS와 같거나 더 좋은 성능을 내면서 저장 공간은 0.29%, 계산 시간은 25%만 사용한다. Table 2 기준으로 여러 모델/데이터셋 평균 improvement에서 heuristic baseline과 LESS를 대체로 앞선다.

특히 LoRA setting에서 validation gradient의 spectral structure를 이용하면, 높은 projection dimension과 Adam-state rescaling에 의존하는 LESS보다 훨씬 작은 subspace로도 target-relevant signal을 보존할 수 있음을 보인다.

## Discussion

GIST는 OPUS/Adam-Shapley와 같은 "optimizer/update geometry를 정확히 봐야 한다"는 흐름에 속하지만, 초점은 PEFT/LoRA targeted instruction tuning이다. diagonal optimizer statistics가 충분하지 않다는 주장은 LoRA 기반 데이터 선택에서 매우 중요하다.

사용자 연구에서는 LESS의 직접 후속/대안 baseline으로 기록할 가치가 크다.

## Limitations

- validation gradients가 target task를 잘 대표해야 한다.
- SVD 기반 subspace rank 선택이 dataset/model별로 민감할 수 있다.
- arXiv preprint/work-in-progress 성격이므로 broader tasks와 production-scale LoRA pipeline에서 추가 검증이 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2602.18584>
- AlphaXiv: <https://www.alphaxiv.org/abs/2602.18584>
- arXiv HTML: <https://arxiv.org/html/2602.18584>
- Code: <https://github.com/GuanghuiMin/GIST>
