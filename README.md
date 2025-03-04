# DefaultKeyBinding.dict サンプル

macOS用の DefaultKeyBinding.dict のサンプルです。

## 概要

macOS では `~/Library/KeyBindings/DefaultKeyBinding.dict` にキーバインドの設定を入れておくと、Cocoa の標準テキストシステムを使用しているアプリケーションであれば共通のキー操作でテキストを編集をすることができます。  
macOS 限定であるため、人気のあるクロスプラットフォーム対応テキストエディタでは使えないのが残念ですが、いろいろなアプリケーションでちょっとした文章を入力することはよくありますので、使いやすい設定を入れておいて損は無いと思います。  

書式等については、そんなに難しくないので "DefaultKeyBinding.dict" とか "macOS Key Bindings"、"Cocoa Text System" といったキーワードで調べてみてください。  
注意点として、Control + N や Control + P (ほかにもあるかも) にシーケンス設定 (「移動」して「削除」する等の2つ以上の動作) を割り当てるとアプリケーションがクラッシュするようになります。MacOSX の初期からあるバグです。一旦設定ファイルを別の場所に移して編集し直しましょう。  
また設定しても動作しないキーバインドがいくつかあります。あきらめて別のキーに割り当てましょう。  
変更した設定はアプリケーションを再起動しないと有効になりません。  

## 設定集

### 基本

標準では単独押しでスクロール、Optionを押しながらで移動になっているキー (Home, End, PageUp, PageDown) の操作を逆にする  

```
"\Uf729" = "moveToBeginningOfDocument:";
"\Uf72b" = "moveToEndOfDocument:";
"\Uf72c" = "pageUp:";
"\Uf72d" = "pageDown:";

"~\Uf729" = "scrollToBeginningOfDocument:";
"~\Uf72b" = "scrollToEndOfDocument:";
"~\Uf72c" = "scrollPageUp:";
"~\Uf72d" = "scrollPageDown:";
```

### マーク付け

キャレットの位置にマークを付ける  

```
"^a" = "setMark:";
```

マークした位置から現在のキャレットまでを選択  

```
"^b" = "selectToMark:";
```

マークした位置までジャンプ

```
"^c" = (
	"swapWithMark:",
	"centerSelectionInVisibleArea:"  
);
```

### 選択範囲の編集

選択した文字を複製 (yank: を増やすとたくさん複製してくれます。コメントで区切り線を入れるとき便利)  

```
"^a"  = (
	"setMark:",
	"deleteToMark:",
	"yank:",
	"yank:"
);
```

選択した文字を括弧等で囲む (記号類の一部はバックスラッシュでエスケープする必要があります。全角文字も入力できます)  

```
"^a" = (
	"setMark:",
	"deleteToMark:",
	"insertText:", "\(", "yank:", "insertText:", "\)"
);
"^b" = (
	"setMark:",
	"deleteToMark:",
	"insertText:", "「", "yank:", "insertText:", "」"
);
```

### 単語の文字変換

キャレットがある単語全体を大文字化  

```
"^a" = (
	"setMark:",
	"uppercaseWord:",
	"swapWithMark:",
	"moveBackward:",
	"moveForward:"
);
```

キャレットがある単語の先頭のみ大文字化  

```
"^b" = (
	"setMark:",
	"capitalizeWord:",
	"swapWithMark:",
	"moveBackward:",
	"moveForward:"
);
```

キャレットがある単語全体を小文字化  

```
"^c" = (
	"setMark:",
	"lowercaseWord:",
	"swapWithMark:",
	"moveBackward:",
	"moveForward:"
);
```

### 文字単位の編集

キャレットの前後の文字の入れ換え (上の設定と下の設定では編集後のキャレットの位置が異なります)  

```
"^a" = "transpose:";

"^A" = (
	"transpose:",
	"moveBackward:",
	"moveBackward:"
);
```

### 空行の追加

行・段落の編集系はオートインデントと相性が悪いです。また文章の先頭や末尾であったり、文末に改行文字がない場合は上手く動作しない設定があります。  

上に空行を追加  

```
"^a" = (
	"setMark:",
	"moveToBeginningOfParagraph:",
	"insertNewlineIgnoringFieldEditor:",
	"swapWithMark:",
	"moveForward:"
);
```

下に空行を追加  

```
"^b" = (
	"setMark:",
	"moveToEndOfParagraph:",
	"moveForward:",
	"insertNewlineIgnoringFieldEditor:",
	"swapWithMark:"
);
```

上に空行を追加してそこに移動  

```
"^c" = (
	"moveToBeginningOfParagraph:",
	"insertNewlineIgnoringFieldEditor:",
	"moveBackward:"
);
```

下に空行を追加してそこに移動  

```
"^d" = (
	"moveToEndOfParagraph:",
	"moveForward:",
	"insertNewlineIgnoringFieldEditor:",
	"moveBackward:"
);
```

### 段落単位の編集

段落を削除  

```
"^a" = (
	"selectParagraph:",
	"delete:"
);
```

段落を複製  

```
"^b" = (
	"moveToEndOfParagraph:",
	"insertNewlineIgnoringFieldEditor:",
	"selectParagraph:",
	"delete:",
	"moveBackward:",
	"selectParagraph:",
	"delete:",
	"yank:",
	"yank:",
	"moveBackward:"
);
```

下の段落と結合  

```
"^c" = (
	"moveToEndOfParagraph:",
	"deleteForward:"
);
```

段落を上に移動  

```
"^d" = (
	"selectParagraph:",
	"delete:",
	"moveBackward:",
	"moveToBeginningOfParagraph:",
	"yank:",
	"moveBackward:"
);
```

段落を下に移動  

```
"^e" = (
	"moveToEndOfParagraph:",
	"moveForward:",
	"moveToEndOfParagraph:",
	"insertNewlineIgnoringFieldEditor:",
	"selectParagraph:",
	"delete:",
	"moveBackward:",
	"moveToBeginningOfParagraph:",
	"moveBackward:",
	"selectParagraph:",
	"delete:",
	"moveToEndOfParagraph:",
	"moveForward:",
	"yank:",
	"moveBackward:"
);
```

### その他

"000" を入力 (文字の入力は文字列であっても問題ありません)  

```
"^a" = (
	"insertText:", "000"
);
```

スペースを入力してキャレットを元の位置に戻す  

```
"^b" = (
	"setMark:",
	"insertText:"," ",
	"swapWithMark:"
);
```

キャレットを10行上に移動 (Up だと移動が遅い、PageUp だと移動しすぎなときに便利)  

```
"^c" = (
	"moveUp:",
	"moveUp:",
	"moveUp:",
	"moveUp:",
	"moveUp:",
	"moveUp:",
	"moveUp:",
	"moveUp:",
	"moveUp:",
	"moveUp:"
);
```

## リンク

- [全角英数や半角カナが判別しやすい、文字間隔調整機能付き等幅フォント「Cyroit」](https://omonomo.github.io/Cyroit/): 合成フォントを作ってみました。
- [小指の移動量が少ない日本語かな入力配列 「水草配列」](https://omonomo.github.io/Mizukusa/): オリジナル日本語かな入力配列を紹介しています。
