---

# Temporal Lecture Video Fragmentation using Word Embeddings
## (Submitted on 11 Dec 2018) Damianos Galanopoulos and Vasileios Mezaris (https://www.iti.gr/~bmezaris/publications/mmm19_lncs11296_1_preprint.pdf)

---
# どんなもの？

* 講義動画を内容で分割するアルゴリズムとして新しいものを考案
  * 既存の動画分割の手法は、動画の画像面に着目したものが多い
  * しかし、講義動画は画面の内容がほぼ一定のためこれまでの手法が使えない
  * そこで、講義動画の音声を文字起こしして利用する動画分割手法を提案
  * 汎用的なWord2Vecを利用し、専用の教師データを用意せずとも利用可能という利点
  * さらに、講義動画分割のアルゴリズムを評価するためのある程度の量があるデータセットを作成し公開
    * https://github.com/bmezaris/lecture_video_fragmentation
  * 実際にMOOCsで利用されている
    * http://videolectures.net/deeplearning2017_bengio_rnn/

---

# どうやって有効だと検証した？

* 比較対象
  * 既存研究
    * 音声文字起こしテキストをBag of Words + 名詞抽出し、TextTilingで処理
    * 音声文字起こしテキストを教師ありのモデルで分割
  * 文書のベクトル化手法
    * Bag of Words + TF-IDF
    * Wrod2Vec
* 比較方法
  * 比較用のデータセットを作成
    * 動画の区切りは人間がやっても難しい問題。講師にやらせてもブレる。
    * そこで、文書分割の研究で提案された手法を使ってデータセットを作成
      * 1500個の講義動画の音声文字起こしテキストを用意
      * 4〜8分程度のランダムなサイズで分割し、混ぜ合わせて20個の要素から1つの文書を作成
* 比較結果
  * 既存研究と比較して、Word2Vecを利用した手法が優れていた

---

#  技術の手法や肝は？

* 手法
  * 講義動画の音声を文字起こし
  * Cue(手がかり)抽出
  * Cueのベクトル表現計算
  * TextTiling
    * 一定のCueを含んだウィンドウを作り、隣接するウィンドウ同士のcos類似度を計算
    * 分割境界の検出

---

# 議論はある？

* 記載なかった

---

# 先行研究と比べて何がすごい？

* 既存研究は複数のモダリティを利用するものが多い
  * しかし、講義動画は内容の性質からそれらが向かない
    * 画面が恐ろしいほど変化がない
    * 動画のvisualな側面が内容の側面と関係していることが少ない
    * スライドが提供されていると分割に使えるが、全ての講義で用意されているわけではない
* 講義動画分割の研究では、Bag of Wordsくらいしか使われていなかった
* publicに公開されててオンラインで入手可能な、比較用データセットを作成した

---

# 次に読むべき論文は？

* TextTiling
  * Hearst, M.A.: Texttiling: Segmentating text into multi-paragraph subtopic passages. Computational linguistics 23(1), 33-64 (1997)
