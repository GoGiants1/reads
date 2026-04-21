# Mastering Collaborative Multi-modal Data Selection: A Focus on Informativeness, Uniqueness, and Representativeness

- **Paper**: Mastering Collaborative Multi-modal Data Selection: A Focus on Informativeness, Uniqueness, and Representativeness
- **arXiv**: [2412.06293v2](https://arxiv.org/abs/2412.06293v2)
- **Authors**: Qifan Yu, Zhebei Shen, Zhongqi Yue, Yang Wu, Bosheng Qin, Wenqiao Zhang, Yunfei Li, Juncheng Li, Siliang Tang, Yueting Zhuang
- **Venue**: ICCV 2025 Highlight
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, visual-instruction-tuning, datatailor, informativeness, diversity

## One-line Summary

DataTailor는 좋은 VIT 샘플을 informative, unique, representative해야 한다고 정의하고, 세 원칙을 협력적으로 점수화해 15% 데이터로 full-data fine-tuning을 넘는 성능을 보고한 data selection framework다.

## Key Idea

DataTailor의 핵심은 데이터 선택을 단일 품질 점수나 단순 diversity 문제로 보지 않는 것이다. informative하지만 중복된 샘플, unique하지만 outlier인 샘플, representative하지만 쉬운 샘플은 각각 한계가 있으므로 세 성질을 동시에 만족하는 subset을 목표로 한다.

분류상 **offline / model-involved / multi-criteria scoring / training-efficient selection**에 해당한다.

## Method

DataTailor는 세 원칙을 명시적으로 모델링한다.

- **Informativeness**: task 해결에 필요한 유용한 신호가 많은가.
- **Uniqueness**: 이미 선택된 샘플과 중복되지 않는가.
- **Representativeness**: 전체 데이터 분포를 대표하며 outlier가 아닌가.

논문은 각 원칙을 실제 dataset에 자동 적응하는 score로 구현하고, tedious hyperparameter tuning 없이 적용 가능한 collaborative framework를 제안한다.

## Experiments

다양한 MLLM benchmark에서 full-data fine-tuning과 selected subset fine-tuning을 비교한다. 핵심 설정은 전체 데이터의 15%만 선택하는 것이다.

## Results

arXiv 초록 기준, DataTailor는 15% 데이터만으로 full-data fine-tuning 성능의 101.3%를 달성한다. 즉 단순 비용 절감뿐 아니라 redundant/noisy data 제거를 통해 full data보다 나은 평균 성능을 낼 수 있다는 주장이다.

## Discussion

DataTailor는 비교 축을 세 가지로 나눠 주기 때문에 survey에서 설명하기 쉽다. CoIDO가 importance-diversity coupling을 학습 objective로 묶는다면, DataTailor는 informativeness/uniqueness/representativeness라는 사람이 해석하기 쉬운 기준을 전면에 둔다.

ScalSelect의 global subspace preservation과도 연결된다. ScalSelect의 leverage score는 대표성과 subspace coverage에 더 가깝고, DataTailor는 informativeness와 uniqueness까지 더 직접적으로 분리한다.

## Limitations

- 세 score가 실제로 독립적인 정보를 주는지, 혹은 특정 benchmark에서는 하나의 score가 대부분을 설명하는지 확인이 필요하다.
- "representative but not outlier" 기준은 embedding/model 선택에 민감할 수 있다.
- 15% budget에서 강한 결과가 다른 dataset scale이나 domain-specific VIT에서도 유지되는지 검증해야 한다.

## References

- arXiv: <https://arxiv.org/abs/2412.06293>
- arXiv HTML: <https://arxiv.org/html/2412.06293>
- Code: <https://github.com/Yuqifan1117/DataTailor>
