# LLM Data Selection and Utilization via Dynamic Bi-level Optimization

- **Paper**: LLM Data Selection and Utilization via Dynamic Bi-level Optimization
- **arXiv**: [2507.16178v1](https://arxiv.org/abs/2507.16178v1)
- **Authors**: Yang Yu, Kai Han, Hang Zhou, Yehui Tang, Kaiqi Huang, Yunhe Wang, Dacheng Tao
- **Venue**: ICML 2025
- **Read date**: 2026-04-21
- **Tags**: data-selection, dynamic-selection, bilevel-optimization, data-weighting, llm

## One-line Summary

이 논문은 고정된 data subset을 고르는 대신, Data Weighting Model이 batch 안의 데이터 가중치를 동적으로 조정하도록 bi-level optimization으로 학습하는 LLM data utilization 방법이다.

## Key Idea

기존 data selection은 학습 전에 score를 매기거나 static subset을 선택하는 경우가 많다. 논문은 LLM 학습 중 모델과 데이터의 상호작용이 변하므로, 좋은 데이터도 training stage에 따라 달라진다고 본다.

따라서 데이터 선택을 "쓸지 말지"의 문제가 아니라, batch마다 어떤 sample을 얼마나 강하게 학습할지 조절하는 dynamic weighting 문제로 바꾼다.

## Method

- Data Weighting Model(DWM)을 두어 batch 내 selected data의 weight를 예측한다.
- outer objective는 validation/target performance를 개선하도록 설정한다.
- inner loop는 weighted data로 LLM을 업데이트한다.
- bi-level optimization으로 DWM이 현재 모델의 data preference를 추적하게 한다.

## Experiments

randomly selected data에 DWM을 적용했을 때와 기존 data selection method 위에 DWM을 얹었을 때를 모두 평가한다. 모델 크기와 data selection method 간 transfer도 분석한다.

## Results

arXiv 초록 기준, DWM은 random-selected data로 학습한 모델의 성능을 개선하고, 학습된 weighting model은 다른 selection method 및 다른 model size에도 transfer된다. 또한 학습 진행에 따라 모델의 data preference가 변한다는 분석을 제공한다.

## Discussion

이 논문은 MATES와 같은 dynamic selection 주장과 맞닿아 있지만, selection model이 sample inclusion보다 utilization weight를 조절한다는 점이 다르다. 사용자의 연구 방향에서 "step마다 도움이 되는 데이터가 달라진다"는 실험적/방법론적 근거로 중요하다.

다만 실제 pretraining 전체 corpus를 매 step 재선택하는 online method라기보다는, differentiable weighting model을 통해 학습 과정에 data preference를 주입하는 방식이다.

## Limitations

- bi-level optimization은 구현과 안정화가 까다롭고 비용이 높을 수 있다.
- validation/outer objective가 실제 target capability를 잘 대표해야 한다.
- weight model이 learned bias를 만들거나 특정 데이터 유형을 과도하게 선호할 가능성이 있다.

## References

- arXiv: <https://arxiv.org/abs/2507.16178>
- Hugging Face Papers: <https://huggingface.co/papers/2507.16178>
