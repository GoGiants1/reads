# Adam's Law: Textual Frequency Law on Large Language Models

- **Paper**: Adam's Law: Textual Frequency Law on Large Language Models
- **arXiv**: [2604.02176v2](https://arxiv.org/abs/2604.02176v2)
- **Authors**: Hongyuan Adam Lu, Z. L., Victor Wei, Zefan Zhang, Zhao Hong, Qiqi Xiang, Bowen Cao, Wai Lam
- **Venue**: ACL 2026 Main Conference
- **Read date**: 2026-04-21
- **Tags**: textual-frequency, data-selection, curriculum-learning, fine-tuning, prompting

## One-line Summary

Adam's Law는 의미가 같은 표현이라면 sentence-level textual frequency가 높은 표현이 LLM prompting과 fine-tuning에서 더 유리하다는 Textual Frequency Law를 제안하고, frequency 기반 paraphrasing과 curriculum fine-tuning을 실험적으로 검증한다.

## Key Idea

LLM은 같은 의미의 paraphrase라도 표현 방식에 따라 성능이 달라질 수 있다. 논문은 이 차이를 textual frequency 관점에서 설명한다. 더 자주 등장하는 문장 표현은 pretraining 중 더 익숙했을 가능성이 높고, 따라서 모델이 더 쉽게 처리하고 생성할 수 있다는 주장이다.

이 관점은 data selection에서 semantic quality뿐 아니라 "표현 빈도" 자체가 중요한 축이 될 수 있음을 제시한다.

## Method

- Textual Frequency Law(TFL): semantic meaning이 유지될 때 sentence-level frequency가 높은 input을 prompting/fine-tuning에 우선 사용한다.
- Frequency estimation: closed-source LLM의 training data를 모르기 때문에 Zipf frequency, ParaCrawl 등 외부 corpus의 word frequency를 조합해 sentence-level frequency를 추정한다.
- Textual Frequency Distillation(TFD): target LLM에 story completion을 시켜 distilled corpus를 만들고, 여기서 frequency estimate를 보정한다.
- Curriculum Textual Frequency Training(CTFT): fine-tuning data를 sentence-level frequency의 증가 순서로 정렬해 curriculum을 구성한다.
- Textual Frequency Paired Dataset(TFPD): high-frequency/low-frequency paraphrase pair를 구성해 prompting과 fine-tuning을 평가한다.

## Experiments

math reasoning, machine translation, commonsense reasoning, agentic tool calling에서 high-frequency paraphrase와 low-frequency paraphrase를 비교한다. DeepSeek-V3, GPT-4o-mini, Llama3.3-70B-Instruct, Qwen2.5 계열 등 closed/open models를 포함한다.

## Results

arXiv 초록 기준, 제안 framework는 TFPD의 여러 task에서 효과를 보인다. AlphaXiv 요약 기준, math reasoning에서 high-frequency paraphrase는 DeepSeek-V3 accuracy를 63.55%에서 71.54%, GPT-4o-mini를 60.70%에서 68.70%, Llama3.3-70B-Instruct를 80.49%에서 88.75%로 올린다.

machine translation에서는 DeepSeek-V3 기준 100개 language pair 중 99개에서 BLEU가 개선되고, 63개 pair는 1점 이상, 31개 pair는 3점 이상, 12개 pair는 5점 이상 향상된다. fine-tuning에서도 high-frequency partition이 low-frequency partition과 original FLORES-200보다 유리한 결과를 보인다.

## Discussion

이 논문은 target-aware data selection과 직접 같은 문제는 아니지만, "같은 의미라도 더 익숙한 표현이 더 잘 작동한다"는 practical signal을 제공한다. benchmark-target selection에서 source example의 task similarity만 볼 것이 아니라, target benchmark가 선호하는 textual register/frequency도 맞출 필요가 있다.

curriculum 관점에서는 CTFT가 easy-to-hard와 다른 축을 제시한다. 빈도 높은 표현부터 학습하는 것이 모델의 언어적 familiarization을 돕는다는 해석이 가능하다.

## Limitations

- sentence-level frequency 추정은 외부 corpus와 tokenization에 민감하며, 실제 pretraining distribution과 다를 수 있다.
- high-frequency paraphrase가 항상 더 precise하거나 task-faithful한 것은 아니다.
- TFD는 target LLM querying 비용이 있으며, closed model bias를 frequency estimate에 주입할 수 있다.

## References

- arXiv: <https://arxiv.org/abs/2604.02176>
- AlphaXiv: <https://www.alphaxiv.org/abs/2604.02176>
- arXiv HTML: <https://arxiv.org/html/2604.02176>
