# Gloss Markdown — 完全リファレンス {heading color=blue}

```toc title="目次" depth=3
```

Gloss Markdown の directive と GitHub 互換記法を網羅したリファレンスドキュメントです。`.gloss.md` ファイルは GitHub 上でプレーン Markdown として表示され、Gloss 対応レンダラーではリッチな UI として表示されます。

---

## GitHub Alert

GitHub Alert は Gloss directive ではありません。GitHub.com と同様に使用できます。

> [!NOTE]
> 参考情報の表示に使います。GitHub Alert 内では **Markdown** が解釈されます。

> [!TIP]
> 有用なヒントやベストプラクティスに使います。

> [!IMPORTANT]
> 読者が見落としてはならない重要な情報に使います。

> [!WARNING]
> 無視すると問題が起きる可能性がある動作の説明に使います。

> [!CAUTION]
> 元に戻せない操作や破壊的な操作の警告に使います。

---

## 見出し属性

見出し行の末尾に `heading` 属性ブロックを置くと、見出し全体に背景色が付きます。
それ以外の見出しの挙動は変わりません。

使用可能な色: `gray` `blue` `green` `yellow` `red` `purple`

## gray {heading color=gray}
## blue {heading color=blue}
## green {heading color=green}
## yellow {heading color=yellow}
## red {heading color=red}
## purple {heading color=purple}

### ネストされた見出しセクション

`nest` 属性を使うと、Markdown の見出しレベルを変えずに見出しセクションを視覚的に 1 段深く表示できます。
下位見出しは親セクションのネスト段数を継承します。下位見出しにも `nest` がある場合は、さらに 1 段追加されます。

## nest なし（デフォルト） {heading color=blue}

本文は追加の視覚的ネストなしで見出しの下に表示されます。

## nest {heading color=blue nest}

本文は次の `##` または `#` 見出しまで、視覚的に 1 段深く表示されます。

### child nest {heading color=green nest}

この子セクションは親のネスト段数を継承し、さらに 1 段追加します。

---

## インラインディレクティブ

### バッジ

`badge` は色付きのラベルを表示します。  
バッククォートの直後に `{` を続け、スペースを入れないでください。

`Stable`{badge color=green} `Beta`{badge color=yellow} `Deprecated`{badge color=red} `Removed`{badge color=gray} `New`{badge color=blue} `Internal`{badge color=purple}

使用可能な色: `gray` `blue` `green` `yellow` `red` `purple`

### キーボードショートカット

`kbd` はテキストを`Ctrl+Shift+P`{kbd}のようにキーの組み合わせとして表示します。

### 小さい文字

`small` はテキストを小さく表示します。  

`小さな文字`{small} — 通常テキスト

---

## 脚注

脚注は Gloss directive ではありません。GitHub.com と同様に使用できます。

このサンプルでは GitHub 形式の脚注を使っています[^gfm]。

[^gfm]: GitHub Flavored Markdown — GitHub で使用される Markdown 方言。

---

## コードブロック

### シンタックスハイライト

コードフェンスには、シンタックスハイライトのための言語名またはファイル拡張子を指定できます。

```ts
function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

### ファイルラベル

言語識別子の後に `filename="path"` を付けると、ブロックの上にファイルパスラベルが表示されます。

```ts filename="src/greet.ts"
function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

```go filename="cmd/main.go"
func main() {
    fmt.Println("Hello, world!")
}
```

---

## 数式

数式は Gloss directive ではありません。GitHub.com と同様に使用できます。

$$
E = mc^2
$$

$$
\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}
$$

質量とエネルギーの等価性 $E = mc^2$ と、オイラーの等式 $e^{i\pi} + 1 = 0$。

---

## 詳細（折り畳み）

`details` は折り畳み可能なセクションを作成します。  
`open` を指定しない場合は閉じた状態で表示されます。  
`color` 属性で色が付きます。

| 属性 | 値 | デフォルト |
| ---- | -- | --------- |
| `title` | 文字列 | `Details` |
| `open` | 真偽値フラグ | `false` |
| `color` | `gray` `blue` `green` `yellow` `red` `purple` | なし |

```details title="デフォルト（折り畳み）"
クリックするまでこの内容は非表示です。
```

```details title="デフォルトで展開" open
`open` フラグ（`open=true` の省略形）を指定するとページ読み込み時に展開されます。
```

```details title="色付き" color=blue
`color` 属性で色が付きます。
```

---

## カード

`card` はタイトル付きの枠で囲まれたコンテンツブロックを表示します。  
`title` でヘッダーを追加し、`href` でリンクを設定し、`color` で色を付けます。

| 属性 | 値 | デフォルト |
| ---- | -- | --------- |
| `title` | 文字列 | なし |
| `href` | 安全なリンク先 | なし |
| `color` | `gray` `blue` `green` `yellow` `red` `purple` | なし |

```card title="基本カード"
タイトルバー付きのシンプルなコンテンツブロックです。
```

```card title="カラーカード" color=blue
`color` を使うとカードに色が付きます。
```

```card title="リンクカード" href="https://example.com" color=green
`href` を追加するとカード全体がリンクになります。
```

---

## タブ

`tabs` はタブコンテナを作成します。  
各 `tab` 子要素は `title` を指定できます。省略した場合は `Tab 1`, `Tab 2`, ... が使われます。  
`color` 属性でアクティブタブのインジケーター色を設定します。

| ディレクティブ | 属性 | 値 | デフォルト |
| -------------- | ---- | -- | --------- |
| `tabs` | `color` | `gray` `blue` `green` `yellow` `red` `purple` | なし |
| `tab` | `title` | 文字列 | `Tab 1`, `Tab 2`, … |
| `tab` | `color` | 同上 | `tabs` から継承 |

````tabs
```tab title="タブ A"
最初のタブのコンテンツです。ここでは Markdown が使えます。
```

```tab title="タブ B"
2 番目のタブのコンテンツです。
```

```tab title="タブ C"
3 番目のタブのコンテンツです。
```
````

タブ本文にコードブロックを含む場合、その内側フェンスに合わせて各フェンスが 1 段ずつ増えます（`tabs`: 5、`tab`: 4、コードブロック: 3）:

`````tabs color=blue
````tab title="TypeScript"
```ts
const x: number = 42;
```
````

````tab title="Go"
```go
var x int = 42
```
````

````tab title="Python"
```py
x: int = 42
```
````
`````

---

## ステップ

`steps` は番号付きの手順を表示します。  
各 `step` は本文を持ち、`title` を指定できます。省略した場合は `Step 1`, `Step 2`, ... が使われます。

| ディレクティブ | 属性 | 値 | デフォルト |
| -------------- | ---- | -- | --------- |
| `steps` | `color` | `gray` `blue` `green` `yellow` `red` `purple` | なし |
| `step` | `title` | 文字列 | `Step 1`, `Step 2`, … |
| `step` | `color` | 同上 | `steps` から継承 |

````steps
```step title="インストール"
`go install github.com/aXisho/glmo@latest` を実行してレンダラーをインストールします。
```

```step title="ファイルを開く"
`glmo README.gloss.md` を実行してブラウザでファイルを開きます。
```

```step title="編集して保存"
保存のたびにブラウザが自動でリロードされます。
```
````

---

## グリッド

`grid` は `cell` の子要素を CSS グリッドでレイアウトします。  
`cols` で列数を制御し、`border` でセルの区切り線を切り替えます。

| ディレクティブ | 属性 | 値 | デフォルト |
| -------------- | ---- | -- | --------- |
| `grid` | `cols` | 正の整数 | セル数 |
| `grid` | `border` | `solid` `none` | `solid` |
| `grid` | `color` | `gray` `blue` `green` `yellow` `red` `purple` | なし |
| `cell` | `title` | 文字列 | なし |
| `cell` | `color` | 同上 | `grid` から継承 |
| `cell` | `border` | `solid` `none` | `grid` から継承 |

### ボーダーあり（デフォルト）

````grid cols=3
```cell
**概要**

ここに説明するトピックの簡単な紹介文が入ります。
```

```cell
**特徴**

主な機能や、試してみる価値がある点を記述します。
```

```cell
**参考情報**

リンク、参考文献、追加の読み物などを掲載します。
```
````

### ボーダーなし（`border=none`）

````grid cols=3 border=none
```cell color=blue
**デザイン**

初期設定のままで見栄えのよい出力が得られます。
```

```cell color=green
**ワークフロー**

既存の執筆・レビューフローに自然に溶け込みます。
```

```cell color=purple
**互換性**

どこでも読め、対応環境ではリッチに表示されます。
```
````

### 列数は cell 数が既定（`cols` 未指定）

`````grid border=none
````cell
**左カラム**

リスト、コードブロック、その他の要素も問題なく使えます。

- 項目 1
- 項目 2

```ts
const x = 42;
```
````

````cell
**右カラム**

`cols` を指定しない場合、グリッドは cell の数だけ列を作ります（ここでは 2 列）。行の高さは揃います。

> [!TIP]
> `cell` 内に GitHub Alert をネストできます。
````
`````

## 属性の構文

すべてのディレクティブの属性は共通のキー・バリュー形式に従います。

````grid cols=3 border=none
```cell color=blue
**クォートなし**

`key=value`

スペースを含まない単語の値に使います。
```

```cell color=green
**クォートあり**

`key="long value"`

スペースを含む値には必須です。
```

```cell color=purple
**真偽値フラグ**

`open` または `open=true`

キー単独は `true` の省略形です。
```
````

クォート内のエスケープシーケンス: `\"` (リテラルクォート)、`\\` (バックスラッシュ)。
