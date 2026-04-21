# Task-Aware Data Selection via Proxy-Label Enhanced Distribution Matching for LLM Finetuning

- **Paper**: Task-Aware Data Selection via Proxy-Label Enhanced Distribution Matching for LLM Finetuning
- **arXiv**:
- **Authors**: Hao Cheng, Rui Zhang, Ling Li, Na Di, Jiaheng Wei, Zhaowei Zhu, Bo Han
- **Venue**: ICLR 2026
- **Read date**: 2026-04-21
- **Tags**: data-selection, task-aware-selection, distribution-matching, proxy-labels, llm-finetuning

## One-line Summary

TADS는 target instruction의 입력 X뿐 아니라 LLM이 생성한 proxy label Y까지 포함한 joint distribution을 맞추도록 source instruction data를 고르는 task-aware LLM fine-tuning selection 방법이다.

## Key Idea

기존 data selection은 source와 target의 instruction/input embedding이 가까운지를 주로 본다. 이 논문은 같은 입력처럼 보여도 필요한 output style, topic, task, audience가 다르면 target에 도움이 되지 않을 수 있다고 지적한다. 따라서 selection은 X만이 아니라 (X, Y)의 joint distribution alignment 문제로 봐야 한다.

명시적 label Y가 없으므로 LLM이 target set에서 proxy labels를 만들고 source corpus로 전파한다.

## Method

- 작은 target set에서 LLM prompt로 task/topic/style/audience 등 proxy labels를 생성한다.
- source corpus에도 proxy labels를 부여하거나 전파한다.
- LLM-generated quality score로 open-set label noise와 OOD sample을 제거한다.
- incremental sampling으로 source subset의 empirical distribution을 target distribution에 맞춘다.

## Experiments

OpenReview 초록 기준, 300K source pool에서 10K samples를 선택해 여러 alignment/fine-tuning benchmark에서 평가한다. LESS, TSDS 등 task-specific selection baseline과 비교한다.

## Results

공식 OpenReview/ICLR 페이지 기준, 300K 중 10K만 선택해도 state-of-the-art method와 경쟁적이거나 더 좋은 성능을 낸다. proxy labels를 쓰는 joint distribution alignment가 X-only similarity보다 더 task-aware한 subset을 만든다는 점을 보인다.

## Discussion

이 논문은 benchmark-target data selection에서 "target의 label/output side semantics를 selection에 넣어야 한다"는 중요한 지적을 한다. BETR이나 LESS가 target examples와 후보의 similarity/influence를 보는 반면, TADS는 target의 latent task schema를 LLM으로 구조화해 matching한다.

특히 target benchmark가 단순 topic이 아니라 response style, reasoning type, audience까지 요구할 때 유용하다.

## Limitations

- LLM-generated proxy label의 품질과 bias가 selection을 좌우한다.
- proxy label field 설계가 target domain을 충분히 포착하지 못하면 성능이 떨어질 수 있다.
- OpenReview 기준 TyDiQA 같은 multilingual task에서는 일반 label fields가 한계를 보일 수 있다.

## References

- OpenReview: <https://openreview.net/forum?id=R40WoYbYab>
- ICLR virtual poster: <https://iclr.cc/virtual/2026/poster/10009535>
- Code: <https://github.com/tmlr-group/TADS>
