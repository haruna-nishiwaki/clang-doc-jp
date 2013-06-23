# Clang 3.4 documentの興味あるところだけ翻訳する-Clangをライブラリとして使う編(3)
*このドキュメントは、[ Clang 3.4 documentation](http://clang.llvm.org/docs/index.html ) をテキトーな日本語にゆるふわ翻訳しているドキュメントです。誤解釈などがたくさんあると思いますので信用しないでください。*

# ClangのASTのイントロダクション
この項目では、神秘的なClangのAST（抽象構文木）を紳士的に紹介します。
Clang（プロジェクト）に貢献することを望んでいる方や、ASTマッチャーのようなClangのASTを使ったツールを作りたいと考えている方を対象としています。

## 動画 - The Clang AST -a Tutorial
動画を参照してください。

## イントロダクション
ClangのASTは、他のコンパイラとは違った作り方がなされています。例えば、括弧式とコンパイル時定数は減らされていないASTから利用できます。
これによって、ClangのASTはリファクタリングツールに適したものとなっています。
全てのClangのASTノードのドキュメントはDxygenを生成することで利用することができるようになります。doxygen のオンラインドキュメントはあなたのお好きなサーチエンジンでClangのASTノードのクラス名を検索すれば見ることが出来るでしょう、例えば、「clang ParenExpr」のように検索してみてください。

## ASTのテスト
ClangのASTに慣れ親しむには、実際にシンプルなサンプルコードのASTを見るというのがよい方法です。ClangはAST-dumpモードを持っており、 -ast-dumpや  -ast-dump-xmlなどとオプションを付けることでASTをダンプできます（-ast-dump-xmlオプションは、clangのdebugビルドをした場合しか動かせません）。

簡単なASTのサンプルを見てみましょう：

```test.cc
int f(int x) {
  int result = (x / 42);
  return result;
}
```
```
$ clang -cc1 -undef -ast-dump-xml test.cc
... cutting out internal declarations of clang ...
<TranslationUnit ptr="0x4871160">
 <Function ptr="0x48a5800" name="f" prototype="true">
  <FunctionProtoType ptr="0x4871de0" canonical="0x4871de0">
   <BuiltinType ptr="0x4871250" canonical="0x4871250"/>
   <parameters>
    <BuiltinType ptr="0x4871250" canonical="0x4871250"/>
   </parameters>
  </FunctionProtoType>
  <ParmVar ptr="0x4871d80" name="x" initstyle="c">
   <BuiltinType ptr="0x4871250" canonical="0x4871250"/>
  </ParmVar>
  <Stmt>
(CompoundStmt 0x48a5a38 <t2.cc:1:14, line:4:1>
  (DeclStmt 0x48a59c0 <line:2:3, col:24>
    0x48a58c0 "int result =
      (ParenExpr 0x48a59a0 <col:16, col:23> 'int'
        (BinaryOperator 0x48a5978 <col:17, col:21> 'int' '/'
          (ImplicitCastExpr 0x48a5960 <col:17> 'int' <LValueToRValue>
            (DeclRefExpr 0x48a5918 <col:17> 'int' lvalue ParmVar 0x4871d80 'x' 'int'))
          (IntegerLiteral 0x48a5940 <col:21> 'int' 42)))")
  (ReturnStmt 0x48a5a18 <line:3:3, col:10>
    (ImplicitCastExpr 0x48a5a00 <col:10> 'int' <LValueToRValue>
      (DeclRefExpr 0x48a59d8 <col:10> 'int' lvalue Var 0x48a58c0 'result' 'int'))))

  </Stmt>
 </Function>
</TranslationUnit>
```

一般的に、```-ast-dump-xml```は、デクラレーション（定義など）をXMLスタイルフォーマットで、ステートメントはS式でダンプします。

 >debugビルドをしていない場合は、-ast-dumpオプションを使うとよいと思います。

このように、解釈ユニットのトップレベルの定義は常に解釈ユニットの定義になります。たとえば、test.ccでは一番始めに``f``の関数定義があります。その関数fの本文は、変数resultを定義している定義文の子ノードで、複数のステートメントからなり、そしてそれはreturnステートメントです。

## ASTコンテキスト
解釈ユニットのASTについての全ての情報は、 **ASTContext**  クラスの中にまとめられています。 **ASTContext** クラスは全ての解釈ユニットをはじめる**getTranslationUnitDecl**あるいはClangの解釈ユニットをパースするための定義テーブルにアクセスすることを許可します。

## ASTノード
