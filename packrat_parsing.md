# Packrat Parsing: Simple, Powerful, Lazy, Linear Time
[https://bford.info/pub/lang/packrat-icfp02.pdf](https://bford.info/pub/lang/packrat-icfp02.pdf)

1. Introduction
* 関数型プログラミングにおけるパーサーの実装方法にはいくつかある。一番簡易的なのは top-down または recursive descent parsing と呼ばれる手法。
* top-down パーサーはさらに2つの手法に分かれる
  1. Predictive Parsers: ある点からいくつかのシンボルを先読みして、どういった言語を構築できるかを予測する手法。
  2. Backtracking Parsers: 成功するまで懐疑的な決定を行い続け、失敗したら元の位置に戻る方法。
* この論文では、top-down パース戦略なんだけど、prediction と backtracking の間を行き来する手法を取る。Packrat Parsing と呼ぶ。
* LL(k) や LR(k) 文法にも対応可能。
* Packrat parser は longest-match, followed-by, not-followed-by という手法を用いて、文脈自由文法 (context-free grammar) の実装を線形時間で行えるようにする。
* この手法の主なデメリットは空間効率。なんだけど、他とのトレードオフを考えたらこの空間効率の悪さは微々たるもの、らしい。

2. Building a Parser
2.1 Recursive Descent Parsing
* 今回置き換えを試みる再帰降下構文解析器についてのかんたんな解説と導入。
* Parsed: Scala に直すと `case class Parsed[V](success: V, remainder: String)` かな？Haskell がわからんちん。
* NoParse: こちらは失敗した場合に入る。
* pXXX: 
  * pAdditive: 最終的には足し算の評価まで行う関数。
  * pMultitive: multiplicative-precedence expression らしい。この関数が成功すると、論文中における vleft の値を成功として返して、その他を入力の残りとして認識する。
  * pPrimary: primary 表現 (AST でよく見るやつ) だと思う。
  * pDecimal: 数値そのものを格納しているものだと思う。
  
2.3 Tabular Top-Down Parsing
* いろいろ手続き上の状態遷移について書かれていたが、重要なことは、こういったパースのプロセスは実はマトリックスにできるということなんだと思った。
* そしてこれを利用して、後続の改善過程につなげていくアイディア。

2.4 Packrat Parsing
* 書かなかったが、2.3 でやったみたいな表形式の右から左のパーシングアルゴリズムの特性を利用して処理を行っていくのが Packrat Parsing らしい。
