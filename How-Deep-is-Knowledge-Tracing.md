---

# How Deep is Knowledge Tracing?
## (Submitted on 14 Mar 2016) Mohammad Khajah, Robert V. Lindsey, Michael C. Mozer (https://arxiv.org/abs/1604.02416 )

---
# どんなもの？

* DKTがなぜBKTよりも精度が高いのかを分析・考察
* DKTがデータに潜んでいる統計的な規則性を抽出していることがその原因と判断
* その規則性の仮説を4つ提示
* これらの規則性を考慮したBKTの拡張版を実装することでDKTに近い精度を出せることを確認

---

# どうやって有効だと検証した？

* ４種類のデータを用意して各モデルの精度を比較検証

---

#  技術の手法や肝は？

* 教育データに存在している統計的な規則性の4仮説を整理
  * 新近性効果(Recency Effect): 直近の問題の正誤が次の問題の正誤に影響しやすい
  * 問題の順序による文脈(Contextualized Trial Sequence): 複数の内容についての問題を実施する場合、問題の順序によって学習効果に差がでる
  * スキル間の類似性(Inter-Skill Similarity):複数のスキルに類似性や関係性があることが多く、習得が同時に起きたり、一方の習得が他に影響したりする
  * 能力の個人差(Individual Variation in Ability):
* DKTは上記の規則性を考慮できるモデル設計になっているが、BKTはそうではない。
  * それぞれの規則性を考慮した拡張版BKTモデルを用意してDKTと比較


---

# 議論はある？

* DKTの強みはデータに潜む規則性をモデルが勝手に考慮してくれる点にある(柔軟性とロバストネス)
* しかし、解釈が難しいという欠点を抱えている。
* 画像認識でのDeep Learningの成功のように、Deepにすることで新しい表現を獲得しているというわけでもない
* データごとの規則性に合わせられる柔軟性とロバストネスが主なメリット
  * 教育というのが本質的に抽象的で、学習者の心理が影響するし、学習内容は高度に記号化されているので、画像認識のような表現獲得自体が難しいと思われる
* BKTをデータに合わせて拡張することでDKT相当の精度が出せるし、結果の解釈性も高い。
  * 解釈が難しい多数のパラメータからできたDKTのモデルを使う必要性は低い

---

# 先行研究と比べて何がすごい？


---

# 次に読むべき論文は？
* R. V. Lindsey, M. Khajah, and M. C. Mozer. Automatic discovery of cognitive skills to improve the prediction of student learning. In Z. Ghahramani, M. Welling, C. Cortes, N. D. Lawrence, and K. Q. Weinberger, editors, Advances in Neural Information Processing Systems 27, pages 1386–1394. Curran Associates, Inc., 2014.
