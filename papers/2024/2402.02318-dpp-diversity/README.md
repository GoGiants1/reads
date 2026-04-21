# Diversity Measurement and Subset Selection for Instruction Tuning Datasets

- **Paper**: Diversity Measurement and Subset Selection for Instruction Tuning Datasets
- **arXiv**: [2402.02318v1](https://arxiv.org/abs/2402.02318v1)
- **Authors**: Peiqi Wang, Yikang Shen, Zhen Guo, Matthew Stallone, Yoon Kim, Polina Golland, Rameswar Panda
- **Venue**: arXiv 2024
- **Read date**: 2026-04-21
- **Tags**: data-selection, instruction-tuning, diversity, dpp, log-determinant

## One-line Summary

이 논문은 instruction tuning dataset의 diversity를 normalized weight gradient space에서 DPP와 log determinant distance로 측정하고, 이 diversity measure가 downstream instruction-following performance와 연결됨을 보인다.

## Key Idea

Instruction tuning data curation에서 diversity는 중요하다고 자주 말하지만, 많은 연구가 task 수나 topic 수 같은 heuristic에 의존한다. 논문은 diversity를 더 직접적으로 측정하기 위해 determinantal point process(DPP)를 사용한다.

핵심은 dataset을 모델의 normalized weight gradient representation으로 놓고, 그 공간에서 얼마나 diverse한지 log determinant distance로 재는 것이다. 이 값이 작을수록 maximally diverse reference dataset에 가까워 더 diverse하다고 해석한다.

## Method

- instruction example을 여러 representation space에 매핑한다.
- 특히 normalized weight gradient space에서 RBF kernel을 구성한다.
- DPP MAP inference로 diverse subset을 고른다.
- dataset diversity를 log determinant distance로 정의한다. 이는 maximally diverse reference dataset의 log determinant와 target dataset의 log determinant 차이를 dataset size로 normalize한 값이다.
- 이 diversity score를 downstream instruction-following performance 및 pruning utility와 비교한다.

## Experiments

여러 instruction tuning datasets에서 subset selection과 diversity measurement를 평가한다. 다양한 representation choice를 비교하며, normalized weight gradient space의 diversity가 실제 instruction-following performance와 얼마나 correlate되는지 분석한다.

## Results

arXiv/AlphaXiv full markdown 기준, normalized weight gradient space에서의 log determinant distance는 downstream instruction-following performance와 상관이 있다. 따라서 fine-tuning 전에 어떤 dataset에서 data selection/pruning이 특히 도움이 될지 판단하는 signal로 쓸 수 있다.

또한 DPP 기반 subset selection은 단순 task-count heuristic보다 데이터의 quality-diversity trade-off를 더 구조적으로 다룰 수 있음을 보인다.

## Discussion

이 논문은 influence/target-gradient selection과는 결이 다르지만, balanced selection과 coverage를 논할 때 중요한 baseline이다. BIDS가 task별 influence imbalance를 보정한다면, 이 논문은 dataset-level diversity 자체를 gradient geometry로 측정한다.

사용자 연구에서는 selected subset의 diversity를 사후 분석하거나, target-aware score에 diversity regularization을 붙이는 근거로 활용하기 좋다.

## Limitations

- DPP kernel과 representation 선택에 성능이 민감하다.
- log determinant distance가 낮다고 해서 target benchmark에 필요한 capability coverage가 항상 충분하다는 뜻은 아니다.
- full markdown 기준 preliminary work로 표시되어 있어 최종 publication 여부와 실험 확장은 추가 확인이 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2402.02318>
- AlphaXiv: <https://www.alphaxiv.org/abs/2402.02318>
- arXiv HTML: <https://arxiv.org/html/2402.02318>
