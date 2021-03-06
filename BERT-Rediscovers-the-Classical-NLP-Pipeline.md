# BERT Rediscovers the Classical NLP Pipeline

Ian Tenney1 Dipanjan Das1 Ellie Pavlick1,2
1Google Research 2Brown University

https://www.aclweb.org/anthology/P19-1452.pdf


# Abstract
事前に学習したテキストエンコーダーは、多くのNLPタスクにおいて急速に進歩してきた。本研究では、そのようなモデルの一つであるBERTに焦点を当て、言語情報がネットワーク内のどこに取り込まれているかを定量化することを目的としている。このモデルは、従来のNLPパイプラインのステップを解釈可能でローカライズ可能な方法で表現しており、各ステップを担当する領域が期待される順序で現れることを発見した。このように、本研究では、このような研究者の研究成果を、より多くの人に知ってもらうために、研究者の研究成果の一部を紹介しています。定性的分析では、モデルはこのパイプラインを動的に調整することができ、多くの場合、高レベルの表現からの曖昧性を解消する情報に基づいて低レベルの決定を修正することができることが明らかになった。

# Intoroduction

ELMo（Petersら、2018a）やBERT（Devlinら、2019）のような事前に訓練された文エンコーダーは、多くのNLPタスクにおける技術の状態を急速に改善してきた。
そして、自然言語処理システムの基礎として、静的な単語エンベッディング（Mikolovら、2013）や離散的なパイプライン（Manningら、2014）に取って代わろうとしているようです。
これは性能面では有利ではあるが、解釈可能性を犠牲にしており、そのようなモデルが実際に自然言語を表現するために重要だと直感的に信じている種類の抽象化を学習しているのか、それとも単に複雑な共起統計をモデル化しているだけなのかは不明なままである。

最近の研究では、最新のモデルが満足のいく方法で言語を表現しているかどうかを理解するために、最先端のモデルを「プローブ」する研究が始まっています。

この研究の多くは行動ベースであり、制御されたテストセットを設計し、エラーを分析して、モデルが表現しているかどうかの抽象化の種類をリバースエンジニアリングしている(例: Conneau et al., 2018; Marvin and Linzen, 2018; Poliak et al., 2018)。
並行して行われている研究では、ネットワークの構造を直接検査して、異なるタイプの言語的決定に関連する局在性のある領域が存在するかどうかを評価している。
そのような研究は、深層言語モデルが様々な構文情報および意味情報をコード化できるという証拠を生み出している（例えば、Shi et al., 2016; Belinkov, 2018; Tenney et al., 2019）。 モデルの(Peters et al., 2018b; Blevins et al. 2018).
我々は、BERTモデル（Devlinら、2019）に焦点を当てたこの後者の作業を構築し、従来のNLPパイプラインから派生した一連のプロービングタスク（Tenneyら、2019）を使用して、以下を定量化する。
ここでは、特定のタイプの言語情報がエンコードされている。言語モデルの下位層がより局所的な構文を符号化する一方で、上位層が以下のような情報を捉えるという観測（Peters et al. より複雑な意味論では、2つの新しい 貢献しています。
まず、従来のNLPパイプラインの共通コンポーネントにまたがる分析を提示する。
特定の抽象化が符号化される順序は、これらのタスクの伝統的な階層構造を反映していることを示す。第二に、個々の文がBERTネットワークによってレイヤーごとにどのように処理されるかを定性的に分析する。
このモデルは、パイプラインの順序が集合的に保持される一方で、個々の決定が任意の方法で相互に依存し、曖昧な決定を延期したり、より高いレベルの情報に基づいて誤った決定を修正したりすることができることを示す。

# Model

## エッジプロービング。
我々の実験は、Tenney et al. (2019)の「エッジプロービング」アプローチに基づいており、このアプローチは、事前に訓練されたエンコーダから言語構造に関する情報をどれだけうまく抽出できるかを測定することを目的としている。エッジプロービングでは、構造化予測タスクを共通のフォーマットに分解し、プロービング分類器がスパンs1 = [i1, j1]と(オプションで)s2 = [i2, j2]を受け取り、構成要素や関係型などのラベルを予測しなければならない。
確率分類器は、ターゲットスパン内のトークンごとの文脈ベクトルにしかアクセスできないため、これらのスパンと文中での役割の関係についての情報を提供するために、エンコーダに頼らなければなりません。
我々は、エッジプロービングスイートからの8つのラベリングタスクを使用している：part-of-speech(POS)、構成要素(Consts.)、依存関係(Deps.)、エンティティ、意味的役割ラベリング(SRL)、coreference(Coref.)、意味的プロトロール(SPR; Reisinger et al. 2015)。
と関係分類(SemEval)があります。
これらのタスクは標準的なベンチマークデータセットから導き出され、タスク間の比較を容易にするために、共通のメトリック（微小平均F1）で評価される。2

## BERT．
BERTモデル(Devlin et al., 2019)は、多くのタスクで最先端の性能を示しており、その深層変圧器アーキテクチャ(Vaswani et al., 2017)は、最近の多くの
モデル（例：Radfordら、2018,2019; Liuら、2019）。
我々は、3.3B単語の英語コーパス上でマルチタスク目的（マスクされた言語モデリングと次文予測）で訓練されたストックBERTモデル（ベースとラージ、ケースなし）に焦点を当てる。
我々は事前訓練の結果としてネットワークが言語をどのように表現するかを理解したいので、Tenney ら（2019）（標準的な BERT の使用法から逸脱して）に従い、エンコーダの重みを凍結する。これにより、エンコーダが内部表現を再編成して、プローブ作業に適したものにするのを防ぐことができる。

# Result

図1は、サマリー統計と絶対値を示しています。
F1 スコア、図 2 は層ごとのメトリクスを報告している。
どちらも 24 層 BERT-large の結果を示しています。
モデルを報告します。また、K(?) = KL(?||Uniform)を報告し、各統計量(? = sτ ,∆τ )がタスクごとにどれだけ非一様5であるかを推定する。

## 言語パターン。
タスクは自然な進行でエンコードされており、両方のメトリクスで一貫した傾向が見られます。
POSタグは最も早く処理され、構成要素、依存関係、意味的役割、およびcoreferenceの順に続きます。
つまり、基本的な構文情報はネットワーク内でより早く現れ、高レベルの意味情報はより高い層に現れているようです。
この発見は、構成要素がcoreferenceよりも早く表現されることを発見したPetersら(2018b)の最初の観察と一致していることに注意する。
さらに、一般的に、構文情報は局所化されやすく、構文タスクに関連する重みは数層に集中する傾向があり（高いK(s)とK(∆)）、意味タスクに関連する情報は一般的には
がネットワーク全体に広がっていることを発見した。
例えば、意味的関係と原始的役割 (SPR)では、混合重みが一様に近く、これらのタスクのための非自明な例は、ほぼすべての層で徐々に解決されることを示している。
実体ラベル付けについては、多くの例が層 1 で解決されているが、その後は長い尾を引き、高い層では混合重みの弱い集中が見られるのみである。
これは、BERT がこれらの作業に対する正しい抽象化を表現することが困難であるためなのか、あるいは意味情報が本質的に局在化しにくいためなのかを判断するために、さらなる研究が必要である。

# Conclusion
我々は、BERTネットワークのさまざまな層が文内の構文的および意味的構造をどのように解決できるかを探るために、エッジ・プロービング・タスク・スイートを使用する。
我々は2つの補完的な測定値を提示する。
学習コーパスから学習したスカラー混合重みと、評価セットで測定した累積スコアリングであり、一貫した順序が現れることを示す。
このような伝統的なパイプラインの順序は全体では保持されているが、個々の例では、述語の引数関係のような高レベルの情報を使用して、ネットワークは順序外の解決が可能であり、品詞のような低レベルの決定を区別するのに役立つことを発見した。
このことは、深層言語モデルが伝統的に言語処理に必要と考えられてきた種類の統語的・意味的抽象化を表現できることを裏付ける新たな証拠を提供し、さらに、深層言語モデルが異なるレベルの階層情報間の複雑な相互作用をモデル化できることを示している。
