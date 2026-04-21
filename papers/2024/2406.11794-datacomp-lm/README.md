# DataComp-LM: In Search of the Next Generation of Training Sets for Language Models

- **Paper**: DataComp-LM: In search of the next generation of training sets for language models
- **arXiv**: [2406.11794v4](https://arxiv.org/abs/2406.11794v4)
- **Authors**: Jeffrey Li et al.
- **Venue**: arXiv 2024
- **Read date**: 2026-04-21
- **Tags**: pretraining-data, data-curation, benchmark, datacomp-lm, dclm

## One-line Summary

DataComp-LM(DCLM)은 Common Crawl 기반 240T-token corpus, OpenLM pretraining recipes, 53개 downstream evaluations를 제공해 LM training set curation을 통제 실험으로 비교하는 benchmark/testbed다.

## Key Idea

LM 성능 개선에서 dataset design이 모델/optimizer만큼 중요하지만, pretraining data curation은 재현 가능한 benchmark가 부족했다. DataComp-LM은 deduplication, filtering, data mixing 같은 선택을 같은 corpus와 recipe, evaluation suite에서 비교할 수 있게 만든다.

## Method

- Common Crawl에서 추출한 240T-token standardized corpus를 제공한다.
- OpenLM 기반 pretraining recipe를 제공한다.
- 412M에서 7B parameter scale까지 curation strategy를 평가할 수 있게 한다.
- 53개 downstream evaluations로 dataset quality를 비교한다.
- DCLM-Baseline을 만들기 위해 model-based filtering을 포함한 curation 실험을 수행한다.

## Experiments

participants/baselines가 다양한 data curation strategy를 적용하고, 동일한 scale 및 evaluation protocol에서 성능을 비교한다. DCLM-Baseline은 7B model을 2.6T tokens로 학습해 MMLU 및 평균 NLU task 성능을 평가한다.

## Results

arXiv 초록 기준, DCLM-Baseline은 7B model에서 64% 5-shot MMLU accuracy를 달성한다. MAP-Neo보다 40% 적은 compute로 MMLU를 6.6%p 개선하고, Llama 3 8B 대비 6.6x 적은 compute로 53개 NLU task 평균에서 유사한 성능을 보인다. model-based filtering이 고품질 training set 구성에 중요하다는 결론을 제시한다.

## Discussion

OPUS, MATES, Group-MATES 같은 dynamic/data influence method가 DCLM setting을 사용하거나 DCLM-style baseline과 비교하기 때문에 중요한 기반 논문이다. DataComp-LM은 특정 selection algorithm이라기보다, pretraining data curation 연구를 공정하게 비교하기 위한 infrastructure다.

OPUS 관점에서는 DCLM-FastText 같은 static model-based filter가 강한 baseline으로 등장하며, OPUS는 이보다 training dynamics와 optimizer geometry를 더 직접 반영한다고 주장한다.

## Limitations

- benchmark/testbed는 연구 설계를 표준화하지만, 실제 production corpus의 모든 품질/법적/언어적 제약을 포괄하지는 않는다.
- evaluation suite 선택에 따라 좋은 data curation strategy가 달라질 수 있다.
- DCLM-Baseline은 강하지만 static filtering 중심이므로 학습 중 데이터 유용성 변화는 직접 반영하지 않는다.

## References

- arXiv: <https://arxiv.org/abs/2406.11794>
- Project: <https://www.datacomp.ai/dclm/>
- Hugging Face Papers: <https://huggingface.co/papers/2406.11794>
