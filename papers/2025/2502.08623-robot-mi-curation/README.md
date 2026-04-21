# Robot Data Curation with Mutual Information Estimators

- **Paper**: Robot Data Curation with Mutual Information Estimators
- **arXiv**: [2502.08623v3](https://arxiv.org/abs/2502.08623v3)
- **Authors**: Joey Hejna, Suvir Mirchandani, Ashwin Balakrishna, Annie Xie, Ayzaan Wahid, Jonathan Tompson, Pannag Sanketi, Dhruv Shah, Coline Devin, Dorsa Sadigh
- **Venue**: RSS 2025
- **Read date**: 2026-04-21
- **Tags**: data-curation, robotics, imitation-learning, mutual-information, demonstration-quality

## One-line Summary

이 논문은 로봇 imitation learning dataset에서 각 trajectory가 state-action mutual information에 기여하는 정도를 추정해, 다양하면서도 예측 가능한 demonstration을 선별하는 robot data curation 방법이다.

## Key Idea

로봇 학습에서는 demonstration 수집량이 늘고 있지만, 데이터 품질 평가는 상대적으로 덜 연구되었다. 논문은 좋은 demonstration이 action diversity와 state-conditioned predictability를 동시에 가져야 한다고 본다.

이를 state와 action 사이의 mutual information 기여도로 측정하면, 단순히 많거나 다양한 데이터가 아니라 policy learning에 유용한 trajectory를 찾을 수 있다.

## Method

- state observations를 VAE embedding 등 간단한 representation으로 변환한다.
- trajectory가 dataset 전체의 state-action mutual information에 기여하는 정도를 추정한다.
- k-nearest neighbor 기반 MI estimator를 robotics scale에 맞게 적용한다.
- contribution이 높은 demonstration을 선택하거나 낮은 품질 trajectory를 제거한다.

## Experiments

robot imitation learning policy를 다양한 demonstration subset으로 학습해 성능을 비교한다. trajectory quality, action diversity, predictability를 동시에 고려했을 때의 downstream control 성능을 평가한다.

## Results

arXiv 초록 기준, 제안한 MI 기반 estimator는 individual demonstrations의 상대적 품질을 평가할 수 있고, data curation을 통해 imitation learning policy 성능을 개선한다. 논문은 RSS 2025에 게재되었다.

## Discussion

이 논문은 LLM benchmark-target selection과 직접적 관련은 약하지만, "데이터의 정보량을 모델 학습 관점에서 정량화한다"는 점에서 참고할 수 있다. 특히 diversity와 predictability의 균형은 instruction selection에서도 유사한 trade-off로 나타난다.

## Limitations

- robotics trajectory와 language instruction의 데이터 구조가 달라 직접 이전은 어렵다.
- MI estimation은 representation과 neighbor estimator 선택에 민감하다.
- 로봇 도메인에서는 environment/task coverage가 selection 결과 해석에 중요하다.

## References

- arXiv: <https://arxiv.org/abs/2502.08623>
- Project: <https://jhejna.github.io/demonstration-info>
- AlphaXiv: <https://www.alphaxiv.org/abs/2502.08623>
