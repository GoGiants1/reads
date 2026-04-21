# GREATS: Online Selection of High-Quality Data for LLM Training in Every Iteration

- **Paper**: GREATS: Online Selection of High-Quality Data for LLM Training in Every Iteration
- **arXiv**:
- **Authors**: Jiachen T. Wang, Tong Wu, Dawn Song, Prateek Mittal, Ruoxi Jia
- **Venue**: NeurIPS 2024 Spotlight
- **Read date**: 2026-04-21
- **Tags**: data-selection, online-selection, dynamic-selection, taylor-approximation, llm

## One-line Summary

GREATS는 Taylor expansion으로 batch utility를 근사하고 greedy selection을 적용해, LLM 학습 매 iteration마다 유용한 batch를 고르는 online data selection 방법이다.

## Key Idea

static data selection은 학습 중 모델 상태 변화를 반영하지 못한다. GREATS는 online batch selection이 필요하다고 보고, validation loss reduction을 직접 최적화하되 비용이 큰 exact objective 대신 Taylor approximation을 사용한다.

핵심은 현재 step에서 어떤 candidate가 batch에 추가될 때 모델 업데이트가 validation objective를 얼마나 개선할지 빠르게 근사하는 것이다.

## Method

- 현재 모델과 validation/proxy objective를 기준으로 candidate batch utility를 정의한다.
- Taylor expansion으로 candidate를 batch에 넣었을 때의 objective 변화를 근사한다.
- greedy algorithm으로 batch quality가 높은 sample을 선택한다.
- ghost inner-product 기법으로 large model에서 gradient inner product 계산 비용을 낮춘다.

## Experiments

NeurIPS/OpenReview 초록 기준, 다양한 LLM training setting에서 online selection을 random/static heuristic selection과 비교한다. 핵심 평가는 convergence speed와 generalization performance다.

## Results

공식 초록 기준, GREATS는 LLM 학습 수렴 속도와 일반화 성능을 유의미하게 개선하며, large-scale training에 적용 가능하도록 계산 기법을 설계했다.

## Discussion

OPUS의 가장 직접적인 선행 연구다. 둘 다 "every iteration" dynamic selection을 표방하고 ghost-style efficient inner product를 쓴다. 차이는 GREATS가 raw gradient/Taylor approximation 중심이라 사실상 SGD geometry에 가깝고, OPUS는 AdamW/Muon이 만드는 optimizer-induced effective update space에서 utility를 재정의한다는 점이다.

따라서 OPUS를 설명할 때 GREATS는 "principled online selection은 가능하지만 optimizer-agnostic이라는 한계가 있다"는 비교 대상으로 적합하다.

## Limitations

- Taylor approximation이 long-horizon training effect를 완전히 반영하지는 않는다.
- validation/proxy set 선택에 민감할 수 있다.
- adaptive optimizer의 preconditioning geometry를 명시적으로 반영하지 않는다는 점이 OPUS가 지적하는 핵심 한계다.

## References

- OpenReview: <https://openreview.net/forum?id=232VcN8tSx>
- NeurIPS: <https://proceedings.neurips.cc/paper_files/paper/2024/hash/ed165f2ff227cf36c7e3ef88957dadd9-Abstract-Conference.html>
- Code: <https://github.com/Jiachen-T-Wang/GREATS>
