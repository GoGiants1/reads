# IDEAL: Data Equilibrium Adaptation for Multi-Capability Language Model Alignment

- **Paper**: IDEAL: Data Equilibrium Adaptation for Multi-Capability Language Model Alignment
- **arXiv**: [2505.12762v1](https://arxiv.org/abs/2505.12762v1)
- **Authors**: Chenlin Ming, Chendi Qu, Mengzhang Cai, Qizhi Pei, Zhuoshi Pan, Yu Li, Xiaoming Duan, Lijun Wu, Conghui He
- **Venue**: arXiv 2025
- **Read date**: 2026-04-21
- **Tags**: data-selection, alignment, data-mixture, multi-capability, llm

## One-line Summary

IDEAL은 여러 capability를 동시에 SFT할 때 domain별 데이터 양이 성능 균형을 좌우한다고 보고, gradient 기반 data equilibrium adaptation으로 multi-domain mixture volume을 조절한다.

## Key Idea

대부분의 data selection 연구는 sample quality를 높이는 데 집중하지만, IDEAL은 이미 고품질 multi-domain dataset이 있을 때 각 domain의 양을 어떻게 배분해야 전체 capability가 균형 있게 좋아지는지를 묻는다.

즉 selection이라기보다는 mixture quantity adaptation이다. 특정 downstream task를 target으로 삼는 연구와는 범위가 다르지만, "도움 되는 데이터 비중은 capability마다 다르다"는 근거로 유용하다.

## Method

- capability/domain별 training data volume을 조절 가능한 변수로 본다.
- 모델 성능과 domain mixture 사이의 관계를 gradient 기반으로 추정한다.
- multi-capability objective가 균형 있게 좋아지도록 data volume을 업데이트한다.
- 조정된 mixture로 SFT dataset을 구성한다.

## Experiments

multi-domain/multi-capability SFT setting에서 IDEAL을 heuristic fixed-ratio mixture와 비교한다. 다양한 capability benchmark에서 성능 균형과 forgetting/overfitting 양상을 본다.

## Results

arXiv 초록 기준, IDEAL은 mixture SFT dataset의 domain volume을 효과적으로 최적화해 여러 capability에서 alignment와 성능을 개선한다.

## Discussion

사용자 메모처럼 이 논문은 "downstream task에 따라 도움이 되는 데이터들이 다르다"는 out-of-scope 관련 연구로 두는 것이 맞다. target benchmark data selection의 직접 baseline이라기보다는, multi-capability alignment에서 data mixture를 조절하는 문제를 다룬다.

MATES/TiKMiX가 pretraining 단계의 dynamic mixture라면, IDEAL은 SFT alignment 단계의 capability-balanced mixture adaptation이다.

## Limitations

- high-quality multi-domain dataset이 이미 존재한다고 가정한다.
- capability metric 설계가 잘못되면 equilibrium도 잘못된 방향으로 갈 수 있다.
- target benchmark 하나를 최적화하는 selection problem과는 목적 함수가 다르다.

## References

- arXiv: <https://arxiv.org/abs/2505.12762>
- Related paper record: <https://fugumt.com/fugumt/paper_check/2505.12762v1_enmode>
