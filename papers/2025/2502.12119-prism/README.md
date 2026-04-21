# PRISM: Self-Pruning Intrinsic Selection Method for Training-Free Multimodal Data Selection

- **Paper**: PRISM: Self-Pruning Intrinsic Selection Method for Training-Free Multimodal Data Selection
- **arXiv**: [2502.12119v3](https://arxiv.org/abs/2502.12119v3)
- **Authors**: Jinhe Bi, Aniri, Yifan Wang, Danqi Yan, Wenke Huang, Zengjie Jin, Xiaowen Ma, Sikuan Yan, Artur Hecker, Mang Ye, Xun Xiao, Hinrich Schuetze, Volker Tresp, Yunpu Ma
- **Venue**: arXiv 2025
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, visual-instruction-tuning, prism, training-free, anisotropy

## One-line Summary

PRISM은 visual feature distribution의 anisotropy가 global semantic drift를 만든다고 보고, implicit re-centering으로 intrinsic visual semantics를 복원해 학습 없이 visual instruction data를 고르는 training-free selection 방법이다.

## Key Idea

PRISM의 문제의식은 기존 선택 방법이 proxy inference나 training metric에 너무 비싸게 의존한다는 것이다. 논문은 visual feature 공간 자체에 anisotropy가 있고, global background feature가 selection을 오염시킬 수 있다고 본다.

분류상 **offline / model-involved representation / training-free / anisotropy correction** 방법이다.

## Method

PRISM은 visual feature에서 global background 또는 dominant direction의 영향을 줄여 intrinsic visual semantics를 드러내는 방식으로 self-pruning을 수행한다.

핵심 구성:

- visual feature distribution의 anisotropy를 selection failure 원인으로 분석한다.
- global semantic drift를 줄이기 위해 implicit re-centering을 적용한다.
- re-centered intrinsic semantics 기반으로 informative/redundant samples를 구분한다.
- proxy training이나 backpropagation 없이 selection을 수행한다.

## Experiments

논문은 multimodal benchmarks 8개와 language understanding benchmarks 3개에서 full dataset fine-tuning 및 기존 selection pipeline과 비교한다.

## Results

arXiv 초록 기준, PRISM은 selection + tuning end-to-end 시간을 conventional pipeline의 30%로 줄이고, full dataset fine-tuning을 넘는 101.7% relative improvement를 보고한다.

## Discussion

PRISM은 사용자의 메모처럼 main comparison에서 반드시 넣지 않아도 될 수 있지만, training-free multimodal data selection 계열에서는 중요하다. ScalSelect와 함께 읽으면 좋다.

- **PRISM**: visual feature anisotropy/global semantic drift correction
- **ScalSelect**: instruction-conditioned visual representation + global subspace leverage
- **DataTailor**: multi-criteria scoring

즉 PRISM은 representation geometry correction, ScalSelect는 subspace coverage, DataTailor는 criteria design에 강점이 있다.

## Limitations

- intrinsic semantics가 visual feature re-centering만으로 충분히 복원되는지는 데이터/모델에 따라 달라질 수 있다.
- instruction-conditioned signal을 명시적으로 쓰는 방법보다 instruction-specific region 중요도를 놓칠 수 있다.
- training-free라는 장점과 selection quality 사이 trade-off를 target domain별로 확인해야 한다.

## References

- arXiv: <https://arxiv.org/abs/2502.12119>
- arXiv HTML: <https://arxiv.org/html/2502.12119>
- Code: <https://github.com/bibisbar/PRISM>
