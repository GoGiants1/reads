# The FineWeb Datasets: Decanting the Web for the Finest Text Data at Scale

- **Paper**: The FineWeb Datasets: Decanting the Web for the Finest Text Data at Scale
- **arXiv**: [2406.17557v2](https://arxiv.org/abs/2406.17557v2)
- **Authors**: Guilherme Penedo, Hynek Kydlíček, Loubna Ben allal, Anton Lozhkov, Margaret Mitchell, Colin Raffel, Leandro Von Werra, Thomas Wolf
- **Venue**: arXiv 2024
- **Read date**: 2026-04-21
- **Tags**: pretraining-data, data-curation, fineweb, fineweb-edu, filtering

## One-line Summary

FineWeb는 96개 Common Crawl snapshot에서 만든 15T-token open pretraining dataset이며, FineWeb-Edu는 LLM-generated educational annotations로 학습한 classifier를 이용해 추린 1.3T-token educational subset이다.

## Key Idea

open LLM의 pretraining dataset은 대개 비공개이거나 curation 과정이 불투명하다. FineWeb는 extraction, filtering, deduplication 설계를 공개하고 ablation을 통해 어떤 curation choice가 LM 성능을 바꾸는지 보여준다.

FineWeb-Edu는 특히 educational quality classifier로 web 문서를 거른 subset이 knowledge/reasoning benchmark에서 큰 이득을 줄 수 있음을 보인다.

## Method

- Common Crawl 96개 snapshot에서 raw web text를 추출한다.
- quality filtering, deduplication, ablation pipeline을 체계적으로 문서화한다.
- Llama3-70B-Instruct annotations로 educational quality classifier를 학습한다.
- FineWeb에서 educational score가 높은 문서를 남겨 FineWeb-Edu를 만든다.

## Experiments

여러 ablation model을 학습해 FineWeb, FineWeb-Edu, 기존 open web dataset을 비교한다. MMLU, ARC, OpenBookQA 같은 knowledge/reasoning-intensive benchmark와 general benchmark를 함께 본다.

## Results

arXiv/Hugging Face 요약 기준, FineWeb는 기존 open pretraining dataset보다 좋은 LM 성능을 내고, FineWeb-Edu는 MMLU/ARC 등 지식 및 추론 중심 benchmark에서 뚜렷한 개선을 보인다. 다만 threshold를 너무 높이면 HellaSwag/PIQA 같은 benchmark는 악화될 수 있어 quality filtering에도 trade-off가 있다.

## Discussion

OPUS가 FineWeb/FineWeb-Edu에서 동작을 검증한다는 점에서 중요한 배경 논문이다. FineWeb-Edu는 static educational filter의 강한 baseline이고, OPUS는 이런 static filter 위에서도 매 iteration dynamic selection을 통해 추가 효율을 얻는다고 주장한다.

즉 FineWeb는 "어떤 corpus를 준비할 것인가", OPUS는 "준비된 corpus에서 학습 중 어떤 token을 쓸 것인가"라는 서로 다른 층위의 문제다.

## Limitations

- web corpus 기반이므로 source bias와 coverage bias가 남는다.
- educational classifier의 annotation model과 threshold에 따라 downstream trade-off가 달라진다.
- static filtering이므로 학습 중 모델 상태와 optimizer dynamics는 반영하지 않는다.

## References

- arXiv: <https://arxiv.org/abs/2406.17557>
- Dataset: <https://huggingface.co/datasets/HuggingFaceFW/fineweb-edu>
- Hugging Face Papers: <https://huggingface.co/papers/2406.17557>
