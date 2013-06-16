# Clang 3.4 documentの興味あるところだけ翻訳する-Clangをライブラリとして使う編(2)
*このドキュメントは、[ Clang 3.4 documentation](http://clang.llvm.org/docs/index.html ) をテキトーな日本語にゆるふわ翻訳しているドキュメントです。誤解釈などがたくさんあると思いますので信用しないでください。*

# Clangの外部のサンプル
> ここでいう「外部」とは、Clangプロジェクト以外という意味だと思われる。

## 紹介
このページでは、あなたがClangのツールを開発するのに役立つガイド（または出発点）になるであろういくつかのサンプルを紹介します。
Clangのライブラリベースの設計は、外部のプロジェクトでも容易に使えるようにするためです。そして、我々は常にClangを外部のユーザにもより使いやすいよう改善することに興味を持っています。

Clangが使われる典型的なアプリケーションのカテゴリ：

* 静的解析
* ドキュメント/クロスリファレンス生成

もしClangが使われているプロジェクトやツールを知っていたら（あるいは書いたのなら！）、以下に追加するので Clang's development discussion mailing list にメールを送ってください（またはあなたが既にClangのコンとリビュータなら直接以下に追加して頂いても結構です）。このページの主な目的は開発者の手助けをするような例の提供なので、ソースコードを閲覧できることが必要です。

## プロジェクトとツール集
https://github.com/Andersbakken/rtags/  
**RTags**は、C/C++コードのインデックスを作り、参照やシンボル名、補完情報のインメモリデータベースを永続的に保持するクライアント/サーバ用のアプリケーションです。  
  
http://rprichard.github.com/sourceweb/  
C/C++コードのインデキサー、ナビゲータ  
  
https://github.com/etaoins/qconnectlint  
**qconnectlint**は、QObject::connectで生成されたシグナルとスロット接続の静的一貫性証明するツールです。  
  
https://github.com/woboq/woboq_codebrowser  
**The Weboq Code Browser**は、C/C++プロジェクト向けウェブベースのコードブラウザです。サンプルは http://code.woboq.org/ をチェックしてください！  
  
https://github.com/mozilla/dxr  
**DXR**は、コンパイラによる静的解析データ収集を使ったソースコードクロスリファレンスツールです。  
  
https://github.com/eschulte/clang-mutate  
このツールはC言語ソースファイルの操作の数を実行します。  
  
https://github.com/gmarpons/Crisp  
LLVM/Clang用のコーディングルールバリデーション・アドオンです。CrispのルールはPrologで書きます。新しいルールを書くことが簡単なハイレベルなDSLは開発中です。おそらくCRISPと呼ばれることになりますが、これはCoding Rules in Sugared Prologのアナグラムです。  
  
https://github.com/drothlis/clang-ctags  
C++ソースコード用のタグファイルジェネレータです。  
  
https://github.com/exclipy/clang_indexer  
これはC/C++用のLibClangライブラリをベースにしたインデキサです。  
  
https://github.com/holtgrewe/linty  
**Linty**は、PythonとLibClangによるC/C++スタイルチェッカです。  
  
https://github.com/axw/cmonster  
**cmonster**はClangのC++ パーサのためのPythonラッパです。  
  
https://github.com/rizsotto/Constantine  
**Constantine**は、Clang Pluginを学ぶための小さいプロジェクトです。偽の（おそらく適当なという意味の）const解析を実装しています。constなしで変数が宣言されたときに注意を生成します。  
  
https://github.com/jessevdk/cldoc  
**cldoc**はClangをベースにしたC/C++のドキュメントジェネレータです。cldocはC/C++のソフトウェアドキュメントを書かなければならないという問題を解決できる、近代的で（作業に）割り込まない強気なアプローチです。