# A Unified Theory of Random Projection for Influence Functions

- **Paper**: A Unified Theory of Random Projection for Influence Functions
- **arXiv**: [2602.10449v2](https://arxiv.org/abs/2602.10449v2)
- **Authors**: Pingbang Hu, Yuzheng Hu, Jiaqi W. Ma, Han Zhao
- **Venue**: arXiv 2026
- **Read date**: 2026-04-21
- **Tags**: influence-functions, random-projection, data-attribution, theory, sketching

## One-line Summary

이 논문은 random projection이 influence function의 inverse-sensitive bilinear form을 언제 보존하는지 이론화하고, ridge regularization, effective dimension, Kronecker-factored curvature, out-of-range gradients까지 포함한 sketch size 가이드를 제시한다.

## Key Idea

대규모 모델에서 influence function은 per-example gradients와 curvature inverse가 필요해 계산이 어렵다. 그래서 많은 방법이 random projection으로 gradients/curvature를 낮은 차원에 sketch하지만, 이를 Johnson-Lindenstrauss lemma만으로 정당화하는 것은 부족하다. Influence는 단순 Euclidean distance가 아니라 `g^T F^{-1} g'` 형태의 inverse-sensitive quantity이기 때문이다.

논문은 projection이 influence를 보존하는 조건을 curvature operator와 sketch의 상호작용으로 분석한다.

## Method

- unregularized projection에서 exact preservation이 성립하려면 sketch가 `range(F)` 위에서 injective여야 하며, 따라서 `m >= rank(F)`가 필요함을 보인다.
- ridge-regularized projection에서는 required sketch size가 rank가 아니라 regularization scale에서의 effective dimension에 의해 결정됨을 증명한다.
- Kronecker-factored curvature `F = A ⊗ E`에서는 decoupled sketches `P_A ⊗ P_E`에도 보존 보장이 이어짐을 분석한다.
- test gradient가 `range(F)` 밖 성분을 가질 때 생기는 leakage term을 정량화한다.
- downstream LDS-style metric으로 sketch size/effective dimension 선택 절차를 제안한다.

## Experiments

이론 분석을 synthetic 및 empirical influence estimation setting에서 확인한다. validation set으로 regularization `lambda`를 고르고, 선택된 `lambda`의 effective dimension에 비례하도록 sketch size `m`을 키우는 practical recipe를 제시한다.

## Results

arXiv/AlphaXiv 기준, unregularized sketching은 rank barrier를 피할 수 없지만, ridge regularization은 barrier를 effective dimension으로 완화한다. 이 effective dimension은 실제 rank보다 훨씬 작을 수 있어 practical random projection influence computation의 근거가 된다.

또한 factorized influence에서는 structured row correlations가 있는 decoupled sketch도 보장 가능하며, out-of-range test gradients의 경우 kernel component가 leakage를 만든다는 점을 명시한다.

## Discussion

이 논문은 직접 data selection algorithm이라기보다는, LESS/GIST/OPUS/Influence 기반 방법들이 쓰는 sketching과 projection을 이론적으로 뒷받침하거나 경고하는 reference다. "random projection dimension을 그냥 JL lemma로 정하면 충분한가?"라는 질문에 대해, influence에서는 curvature inverse와 regularization까지 봐야 한다고 답한다.

사용자 연구에서 scalable influence selection을 쓰려면, projection dimension과 ridge scale을 effective dimension 관점에서 설명할 수 있어야 한다.

## Limitations

- projection이 unprojected influence를 잘 보존하는 조건을 주로 다루며, influence 자체가 leave-one-out effect를 얼마나 잘 근사하는지는 별도 문제로 남는다.
- EK-FAC 같은 더 복잡한 curvature approximation에는 확장이 쉽지 않다고 논문도 지적한다.
- 이론 중심 논문이라 end-to-end data selection 성능 비교는 제한적이다.

## References

- arXiv: <https://arxiv.org/abs/2602.10449>
- AlphaXiv: <https://www.alphaxiv.org/abs/2602.10449>
- arXiv HTML: <https://arxiv.org/html/2602.10449>
