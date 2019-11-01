---

# Leveraging BERT for Extractive Text Summarization on Lectures
## (Submitted on 7 Jun 2019) Derek Miller https://arxiv.org/abs/1906.04165

---
# どんなもの？

* BERTを使ってMOOCs講義の発言テキストから抜粋要約する仕組みをつくった。
  * 字幕テキスト、要約後の文章数を入力すると、要約が出力される
  * RESTful APIで動作するサービス実装
  * CLIでも動作
  * https://github.com/dmmiller612/lecture-summarizer

---

# どうやって有効だと検証した？

* 比較対象
  * TextRankによる要約
  * BERT + K-Means
  * BERT + Gaussian Mixture Models
* 比較方法
  * 最終出力の比較 by 人の目によるチェック
  * クラスタリングの精度 by クラスタ中心間の距離

---

#  技術の手法や肝は？

* 手法
  * BERTはGoogle公開のpre-trainedモデル(large)を利用
  * 処理の流れ
    * 入力文のクレンジング
    * BERTを使って入力されたテキストの文ごとの分散表現を取得
      * [CLS]から文単位の分散表現を直接求めるよりも、token単位の分散表現の平均をとって文単位の分散表現を求めた方がクラスタリングの精度は高かった
    * 文をK-Meansでクラスタリング(クラス多数は入力時に指定された文章数)
    * 各クラスタの中心の文章を選ぶ

---

# 議論はある？

* 文章要約自体の課題
  * 要約の精度を定量的に比較する指標がないので、あくまで一目で見る限りで今回のケースはうまくいってそうとしか言えない
* 文章に含まれる文数が多く(100以上に)なると、講義全体を表現した要約になりづらい
  * 文章をグルーピングしたり、クラスタ数をあげたりするので緩和はできそう
* 発言テキストを入力とすることの課題
  * 指示語や言われていない言葉（コンテキスト）に弱い
  * 話し言葉をテキスト化しているため、抜粋要約に向かない文章が選ばれることがある

---

# 先行研究と比べて何がすごい？

* 文章要約タスク(とくに教育関係)での先行研究は最近のDeepLearningの進化が取り入れられていない
  * 主な研究の歴史
    * シンプルな統計モデル(~2005)
    * 統計モデルに修辞情報を追加で利用(2007)
    * TextRankなど複数の統計モデルを用いて、生徒の小論文を要約し、改善をフィードバックする研究(2013)
    * Naive Bayesを使った講義要約の試み(2016)
    * TF-IDF を使った講義要約の試み(2017)
    * 確率モデルを用いたMOOCs講義要約の研究(2018)
    * 黒板文字を抽出して利用した講義要約の研究(2018)
    * 講義スライドを利用した講義要約の研究(2018)

* DeepLearningが使われなかった理由
  * 今まで主流だったRNN(LSTM)が大量の学習データを必要とする割に、満足する精度が出なかった
  * BERTの登場(2018)により事情が変わった
    * Transformer(Attension機構)による精度向上
    * 汎用利用可能な事前学習モデルとして展開されるので、大量データを用意せずに適用可能
      * 転移学習でカスタマイズすることもできる

---

# 次に読むべき論文は？

* Kota, B. U., Davila, K., Stone, A., Setlur, S., & Govindaraju, V. (2018, August). Automated Detection of Handwritten Whiteboard Content in Lecture Videos for Summarization. In 2018 16th International Conference on Frontiers in Handwriting Recognition (ICFHR) (pp. 19-24). IEEE.
  * 黒板文字を抽出した講義要約。INSIGHTの時のデータが使えるかも？
* Vaswani,A.,Shazeer,N.,Parmar,N.,Uszkoreit,J., Jones, L., Gomez, A. N., & Polosukhin, I. (2017). Attention is all you need. In Advances in Neural Information Processing Systems(pp. 5998-6008).
  * Attention機構をきちんと理解したい
* VanLabeke,N.,Whitelock,D.,Field,D.,Pulman,S., & Richardson, J. T. (2013, July). What is my essay really saying? Using extractive summarization to motivate reflection and redrafting. In AIED Workshops.
  * 生徒の小論文を要約してフィードバックした取り組み。 e-ポートフォリオ時代に使えそう
