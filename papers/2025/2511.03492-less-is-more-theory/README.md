# Why Less is More (Sometimes): A Theory of Data Curation

- **Paper**: Why Less is More (Sometimes): A Theory of Data Curation
- **arXiv**: [2511.03492v1](https://arxiv.org/abs/2511.03492v1)
- **Authors**: Elvis Dohmatob, Mohammad Pezeshki, Reyhane Askari-Hemmat
- **Venue**: arXiv 2025
- **Read date**: 2026-04-21
- **Tags**: data-curation, theory, scaling-laws, less-is-more, model-collapse

## One-line Summary

이 논문은 imperfect oracle이 difficulty/correctness에 따라 데이터를 선별하는 설정에서, 작은 curated dataset이 전체 데이터보다 더 나을 수 있는 조건과 phase transition을 이론적으로 분석한다.

## Key Idea

고전적 scaling law는 보통 더 많은 데이터가 더 좋다고 본다. 하지만 LIMO, s1 같은 최근 결과는 작고 강하게 선별된 데이터가 전체 데이터보다 나을 수 있음을 보여준다. 이 논문은 이 "less is more" 현상이 언제 가능한지 이론적으로 설명한다.

핵심은 데이터 양뿐 아니라 선택기의 품질, label correctness, difficulty distribution이 generalization을 좌우한다는 점이다.

## Method

- imperfect oracle이 데이터의 difficulty와 correctness를 기준으로 subset을 선택하는 이론 모델을 둔다.
- label-agnostic curation과 label-aware curation을 구분한다.
- data pruning이 test error scaling curve에 미치는 영향을 닫힌 형태로 분석한다.
- data size와 quality에 따른 phase transition boundary를 도출한다.

## Experiments

이론 예측을 ImageNet 실험으로 검증하고, LLM mathematical reasoning에서 관찰된 small curated dataset 성공 사례와 연결한다. 모델 붕괴를 완화하는 curation 조건도 분석한다.

## Results

arXiv 초록 기준, 특정 compute/label-quality 조건에서는 full dataset보다 small curated dataset이 더 좋은 generalization을 낼 수 있다. 또한 curation이 label shift 또는 noisy synthetic data로 인한 model collapse를 막을 수 있는 phase boundary를 제시한다.

## Discussion

이 논문은 특정 LLM data selection method는 아니지만, "왜 적은 데이터가 더 나을 수 있는가"를 설명하는 이론적 배경으로 유용하다. 특히 benchmark-target selection이 과감한 filtering을 정당화하려면, selection oracle의 정확도와 target distribution alignment 조건을 함께 제시해야 한다.

사용자 연구에서는 empirical method의 주장 강도를 높이는 background theory로 쓰기 좋다.

## Limitations

- 이론 모델의 oracle/difficulty/correctness 가정이 실제 LLM corpus와 완전히 일치하지 않을 수 있다.
- ImageNet 검증이 LLM pretraining/fine-tuning으로 바로 이어지지는 않는다.
- 실제 selection method의 noisy score calibration 문제는 별도로 다뤄야 한다.

## References

- arXiv: <https://arxiv.org/abs/2511.03492>
- Emergent Mind: <https://www.emergentmind.com/papers/2511.03492>
