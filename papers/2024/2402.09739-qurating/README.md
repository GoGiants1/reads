# QuRating: Selecting High-Quality Data for Training Language Models

- **Paper**: QuRating: Selecting High-Quality Data for Training Language Models
- **arXiv**: [2402.09739v3](https://arxiv.org/abs/2402.09739v3)
- **Authors**: Alexander Wettig, Aatmik Gupta, Saumya Malik, Danqi Chen
- **Venue**: ICML 2024
- **Read date**: 2026-04-21
- **Tags**: data-selection, pretraining, data-quality, llm-judge, static-filtering

## One-line Summary

QuRating은 LLM pairwise judgments로 writing style, expertise, facts/trivia, educational value 같은 품질 차원을 학습한 QuRater를 만들고, 이 rating으로 pretraining data를 sampling/curation하는 방법이다.

## Key Idea

기존 pretraining data filtering은 heuristic에 의존한다. QuRating은 사람이 느끼는 텍스트 품질의 추상적 차원을 LLM pairwise comparison으로 수집하고, scalar rating model을 학습해 대규모 corpus에 적용한다.

핵심은 high-quality top-k만 고르는 것이 아니라, quality rating을 sampling logits로 사용해 품질과 다양성의 균형을 맞추는 것이다.

## Method

- 네 가지 품질 기준을 정의한다: writing style, required expertise, facts & trivia, educational value.
- LLM pairwise judgments로 텍스트 쌍의 상대 품질 preference를 만든다.
- QuRater model이 각 품질 차원의 scalar score를 예측하도록 학습한다.
- 260B-token corpus에 rating을 부여하고, rating 기반 sampling 또는 curriculum을 적용한다.

## Experiments

품질 기준별로 30B tokens를 선택해 1.3B-parameter language model을 학습한다. perplexity와 in-context learning 성능을 random/uniform sampling baseline과 비교한다.

## Results

arXiv 초록 기준, quality ratings를 logits로 sampling하면 baseline보다 낮은 perplexity와 더 강한 in-context learning performance를 얻는다. educational value 기반 best model은 uniform sampling으로 50% 더 오래 학습한 모델과 비슷한 성능을 낸다.

## Discussion

OPUS와 비교하면 QuRating은 static, pre-training-agnostic quality filter다. OPUS가 모델 상태와 optimizer update 방향을 보고 매 iteration 선택하는 반면, QuRating은 학습 전 quality classifier로 corpus를 가중 sampling한다.

OPUS 논문에서 static industrial-level filter 비교군으로 들어가는 이유가 명확하다. 품질 필터링은 강하지만 training dynamics와 optimizer geometry를 반영하지 못한다.

## Limitations

- LLM judge와 prompt 설계의 bias가 rating에 들어간다.
- quality만 높게 고르면 diversity가 줄어들 수 있어 sampling temperature가 중요하다.
- downstream target이나 학습 단계별 유용성은 직접 반영하지 않는다.

## References

- arXiv: <https://arxiv.org/abs/2402.09739>
- PMLR record: <https://collaborate.princeton.edu/en/publications/qurating-selecting-high-quality-data-for-training-language-models>
- Code/data: <https://github.com/princeton-nlp/QuRating>
