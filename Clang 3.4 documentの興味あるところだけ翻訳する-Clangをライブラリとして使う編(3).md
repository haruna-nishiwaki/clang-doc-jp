# Clang 3.4 documentの興味あるところだけ翻訳する-Clangをライブラリとして使う編(3)
*このドキュメントは、[ Clang 3.4 documentation](http://clang.llvm.org/docs/index.html ) をテキトーな日本語にゆるふわ翻訳しているドキュメントです。誤解釈などがたくさんあると思いますので信用しないでください。*

# ClangのASTのイントロダクション
この項目では、神秘的なClangのAST（抽象構文木）を紳士的に紹介します。
Clangに貢献することを望んでいる方や、ASTマッチャーのようなClangのASTを使ったツールを作りたいと考えている方を対象としています。

## 動画 - The Clang AST -a Tutorial
動画を参照してください。

## イントロダクション
ClangのASTは、他のコンパイラちは違った作り方がなされています。例えば、括弧の式とコンパイル時普遍はASTから
これによって、ClangのASTはリファクタリングツールに適したものとなっています。
全てのClangのASTノードのドキュメントはDxygenを生成することで利用することができるようになります。doxygen のオンラインドキュメントはあなたのお好きなサーチエンジンでClangのASTノードのクラス名を検索すれば見ることが出来るでしょう、例えば、「clang ParenExpr」のように検索してみてください。

## ASTのテスト
ClangのASTに慣れ親しむには、実際にシンプルなサンプルコードのASTを見るというのがよい方法です。ClangはAST-dumpモードを持っており、 -ast-dumpや  -ast-dump-xmlなどとオプションを付けることでASTをダンプできます（-ast-dump-xmlオプションは、clangのdebugビルドをした土岐市か動かせません）。