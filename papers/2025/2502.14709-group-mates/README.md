# Group-Level Data Selection for Efficient Pretraining

- **Paper**: Group-Level Data Selection for Efficient Pretraining
- **arXiv**: [2502.14709v2](https://arxiv.org/abs/2502.14709v2)
- **Authors**: Zichun Yu, Fei Peng, Jie Lei, Arnold Overwijk, Wen-tau Yih, Chenyan Xiong
- **Venue**: NeurIPS 2025
- **Read date**: 2026-04-21
- **Tags**: data-selection, pretraining, group-influence, dynamic-selection, mates

## One-line Summary

Group-MATES는 개별 sample influence가 아니라 데이터 간 관계를 반영한 group-level influence를 모델링해, pretraining의 speed-quality frontier를 개선하는 data selection 방법이다.

## Key Idea

MATES는 개별 데이터의 influence를 현재 모델 상태에 맞춰 근사한다. Group-MATES는 여기서 한 단계 더 나아가, 실제 pretraining batch/subset 효과는 개별 influence의 단순 합이 아니라 데이터들 사이의 관계와 중복성에 의해 달라진다고 본다.

따라서 data curation의 목표를 group influence가 큰 set을 찾는 문제로 재정의한다.

## Method

- LM training trajectories를 샘플링하고 oracle group-level influence를 수집한다.
- relational data influence model이 individual influence와 data relationship weights를 결합해 group influence를 근사한다.
- relationship weights로 dataset을 작은 cluster로 나눈다.
- 각 cluster 내에서 독립적으로 selection을 수행해 inference 비용을 낮춘다.

## Experiments

DCLM 400M-4x, 1B-1x, 3B-1x setting에서 22개 downstream tasks를 평가한다. random selection과 individual data selection baseline을 비교한다.

## Results

arXiv/OpenReview 초록 기준, Group-MATES는 random selection 대비 3.5%-9.4% 상대 성능 향상을 보이고, individual selection baseline의 개선 폭을 거의 두 배로 늘린다. 특정 downstream performance에 도달하는 데 필요한 token 수를 최대 1.75x 줄인다.

## Discussion

OPUS와 함께 보면 dynamic selection의 granularity 차이가 드러난다. OPUS는 매 iteration sample utility를 optimizer update space에서 본다. Group-MATES는 data relationship과 group utility를 중시해, 개별 score가 실제 subset score와 다를 수 있음을 강조한다.

대규모 pretraining에서는 sample-level online selection과 group-level relational selection을 어떻게 결합할지가 좋은 후속 질문이다.

## Limitations

- relationship weights와 clustering 품질에 selection 결과가 의존한다.
- group influence oracle 수집 비용이 있다.
- OPUS처럼 adaptive optimizer geometry를 직접 utility에 반영하지는 않는다.

## References

- arXiv: <https://arxiv.org/abs/2502.14709>
- OpenReview: <https://openreview.net/forum?id=uX4dyc7Z5Z>
- Code: <https://github.com/facebookresearch/Group-MATES>
