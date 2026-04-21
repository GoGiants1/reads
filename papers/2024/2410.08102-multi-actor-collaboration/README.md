# Efficient Pretraining Data Selection for Language Models via Multi-Actor Collaboration

- **Paper**: Efficient Pretraining Data Selection for Language Models via Multi-Actor Collaboration
- **arXiv**: [2410.08102v3](https://arxiv.org/abs/2410.08102v3)
- **Authors**: Tianyi Bai, Ling Yang, Zhen Hao Wong, Fupeng Sun, Jiahui Peng, Xinlin Zhuang, Chi Zhang, Lijun Wu, Jiantao Qiu, Wentao Zhang, Binhang Yuan, Conghui He
- **Venue**: ACL 2025
- **Read date**: 2026-04-21
- **Tags**: data-selection, pretraining, multi-actor, dynamic-selection, llm

## One-line Summary

이 논문은 여러 data selection criterion을 독립 actor로 두고, pretraining stage마다 console이 actor들의 영향력을 동적으로 조합해 LM pretraining data를 선택하는 multi-actor framework를 제안한다.

## Key Idea

단일 data selection method는 특정 기준에는 강하지만 다른 기준과 충돌할 수 있다. 예를 들어 quality, diversity, difficulty, model-state preference는 서로 다른 데이터를 선호할 수 있다. 논문은 각 selection rule을 actor로 두고, 모델의 현재 상태에 맞게 actor들의 비중을 조절하는 console을 둔다.

사용자 분류에서는 downstream target 차이에 따른 out-of-scope 관련 연구지만, dynamic pretraining selection 관점에서는 MATES/TiKMiX와 함께 읽을 가치가 있다.

## Method

- 각 data selection method를 독립 actor로 정의한다.
- actor는 자기 기준에 따라 data priority를 계산하고 모델 현재 상태를 반영해 업데이트한다.
- console이 stage별 actor impact를 조절한다.
- 여러 actor의 정보를 통합해 최종 pretraining data를 선택한다.

## Experiments

LM pretraining 환경에서 convergence speed와 downstream benchmark 성능을 평가한다. ACL Anthology 기준, 여러 language model benchmark에서 기존 data selection method와 비교한다.

## Results

arXiv/ACL 초록 기준, multi-actor framework는 pretraining convergence를 가속하고, state-of-the-art 대비 평균 상대 성능을 최대 10.5% 개선한다.

## Discussion

이 논문은 "도움 되는 데이터는 task뿐 아니라 selection criterion에 따라서도 다르며, 그 기준들의 비중은 학습 중 바뀔 수 있다"는 관점을 제공한다. 단일 target benchmark 최적화보다는 general pretraining efficiency에 초점이 있어 메인 범위에서는 보조 관련으로 두는 것이 좋다.

## Limitations

- actor 설계와 console update rule이 복잡해진다.
- target benchmark에 대한 직접 최적화보다는 general benchmark 평균 개선에 초점이 있다.
- actor 간 conflict를 해석 가능하게 분석하는 추가 도구가 필요할 수 있다.

## References

- arXiv: <https://arxiv.org/abs/2410.08102>
- ACL Anthology: <https://aclanthology.org/2025.acl-long.466/>
- Code: <https://github.com/Relaxed-System-Lab/multi-actor-data-selection>
