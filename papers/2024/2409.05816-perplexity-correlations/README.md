# Improving Pretraining Data Using Perplexity Correlations

- **Paper**: Improving Pretraining Data Using Perplexity Correlations
- **arXiv**: [2409.05816v2](https://arxiv.org/abs/2409.05816v2)
- **Authors**: Tristan Thrush, Christopher Potts, Tatsunori Hashimoto
- **Venue**: ICLR 2025
- **Read date**: 2026-04-21
- **Tags**: data-selection, pretraining, perplexity, benchmark-correlation, llm

## One-line Summary

이 논문은 여러 공개 LLM의 pretraining text loss와 downstream benchmark 성능 사이의 correlation을 추정해, 추가 LLM 학습 없이 benchmark에 유용한 pretraining data를 고르는 방법을 제안한다.

## Key Idea

좋은 pretraining data를 찾으려면 보통 비싼 pretraining 실험이 필요하다. 이 논문은 이미 존재하는 여러 LLM의 text별 loss와 benchmark score를 활용한다. 어떤 domain/text에서 loss가 낮은 모델들이 특정 benchmark에서도 잘한다면, 그 text/domain은 해당 benchmark에 유용한 pretraining source일 가능성이 높다.

즉 benchmark target을 직접 쓰되, target examples와의 embedding 유사도 대신 "perplexity-benchmark correlation"을 data quality signal로 삼는다.

## Method

- Open LLM Leaderboard의 여러 공개 모델을 사용한다.
- web domain별 text에서 각 모델의 log-likelihood/perplexity를 측정한다.
- 모델별 benchmark performance와 text loss 사이의 correlation을 추정한다.
- benchmark에 긍정적으로 correlated된 domain/text를 선택하도록 classifier/filter를 학습한다.

## Experiments

수만 개 web domain과 90개 LLM의 loss/performance 정보를 사용한다. 이후 160M parameter scale의 controlled pretraining 실험에서 8개 benchmark를 대상으로 DSIR, DataComp-LM bigram classifier 등과 비교한다.

## Results

초록 기준, 제안 방법은 8개 benchmark 모두에서 DSIR을 능가하고, DataComp-LM의 hand-engineered bigram classifier와 동등한 수준에 도달한다. 핵심 장점은 새 LLM pretraining 없이 public model loss만으로 selection rule을 만들 수 있다는 점이다.

## Discussion

BETR이 benchmark examples와 document similarity를 직접 쓰는 반면, 이 논문은 "어떤 data에서의 perplexity가 benchmark ability와 함께 움직이는가"를 본다. 따라서 target benchmark를 데이터 선택 신호로 쓰면서도, embedding matching보다 model behavior 기반이라는 차이가 있다.

이 관점은 dynamic selection으로도 확장될 여지가 있다. 학습 step마다 model loss landscape가 바뀌면, data-source correlation도 변할 수 있기 때문이다.

## Limitations

- 공개 LLM들의 성능 분산과 leaderboard 품질에 의존한다.
- 160M scale 실험 결과가 더 큰 pretraining scale에서 그대로 유지되는지는 별도 검증이 필요하다.
- correlation은 causal influence를 보장하지 않는다.

## References

- arXiv: <https://arxiv.org/abs/2409.05816>
- Hugging Face Papers: <https://huggingface.co/papers/2409.05816>
- Code/models: <https://github.com/TristanThrush/perplexity-correlations>
