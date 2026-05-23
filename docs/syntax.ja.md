# Gloss Markdown — 記法ガイド

[English](./syntax.md) | [日本語](./syntax.ja.md) | [简体中文](./syntax.zh-CN.md)

> [!NOTE]
> **目次**
> 1. ファイル拡張子
> 2. ブロック directive（フェンス形式）
> 3. ネストコンテナ directive
> 4. インライン directive（`` `text`{name attrs} `` 形式）
> 5. 見出し属性
> 6. 属性構文
> 7. 識別子ルール
> 8. シンタックスハイライト
> 9. GitHub Markdown passthrough

Gloss Markdown のベースは GitHub Flavored Markdown (GFM) です。Gloss Markdown は、その上に軽量な **インライン gloss layer** を重ねるフォーマットです。このガイドでは、その記法について説明します。すべての directive 形式は、標準的な Markdown レンダラー（GitHub、VS Code プレビュー等）でも意味が読み取れるように設計されており、Gloss Markdown 対応レンダラーではよりリッチな出力を生成できます。

GitHub.com 独自の Markdown 機能（Alert、脚注、数式、task list、table、取り消し線、autolink、通常の code highlighting など）は、Gloss Markdown の独自記法としては定義しません。これらは入力に含まれていても Gloss directive として解釈せず、そのまま Markdown レンダラーに渡します。

> [!NOTE]
> **このガイドのサンプルコードについて**
> 本ドキュメント中で Gloss Markdown のソースを *示す* コードブロックは、内側のバッククォートフェンスを保護するためにチルダフェンス（`~~~md`）で囲んでいます。実際の `.gloss.md` ファイルではバッククォートフェンス（`` ``` `` または `` ```` ``）を通常通り使用します。

---

## 1. ファイル拡張子

Gloss Markdown のファイルは `.gloss.md` 拡張子を使います。記法は GFM として有効な Markdown に重ねて書かれるため、github.com などの標準 Markdown レンダラーでも通常の Markdown として読めます。

Gloss directive は、専用ビューアがない場合は主に通常のコードブロックやテキストとして表示されます。

---

## 2. ブロック directive（フェンス形式）

ブロック directive は、info string に directive 名と属性を持つ **フェンスコードブロック** として記述します。

~~~md
```details color=blue title="補足"
本文をここに記述します。**Markdown** がパースされます。
```
~~~

- 開始フェンス: info string に `NAME` とオプションの `ATTRS` を書きます。directive 名を先頭に書き、その後ろに属性を続けます。
- **フェンスの長さ:** Gloss Markdown は CommonMark/GFM のフェンス規則に従います。開始フェンスは 3 個以上の同じ文字（バッククォートまたはチルダ）で、終了フェンスは同じ文字を開始フェンス以上の長さで書きます。本文に同じ種類のフェンスを含める場合だけ外側のフェンスを内側より長くすればよく、入れ子のフェンスが無ければ 3 個で十分です。
- フェンス間の本文は、Gloss 対応レンダラーでは Markdown としてパースされます。プレーン Markdown ビューアでは本文がコードブロックとして表示されるため、グループ化とスコープが視覚的に伝わります。

本文を持たない directive（現在は `toc` のみ）は、同じ形式で本文を空にして書きます — 開始フェンスと終了フェンスの間に行はありません。

~~~md
```toc title="目次" depth=3
```
~~~

この形式で記述する標準 directive:

```text
details  card  toc
```

次の fence info 名は Gloss Markdown directive 用に予約されています。フェンスの info string の最初の空白区切り token がこれらの名前のいずれかである場合、Gloss 対応レンダラーは通常の code block ではなく directive として扱います。

```text
details  card  toc  tabs  tab  steps  step  grid  cell
```

**使用可能な属性**

| Directive | 属性 | 値 | 既定値 |
| --------- | ---- | -- | ------ |
| `details` | `title` | 文字列 | `Details` |
| `details` | `open` | 真偽値（§6.3） | `false` |
| `details` | `color` | color 値（§6.4） | なし |
| `card` | `title` | 文字列 | なし |
| `card` | `href` | リンク先（§6.5） | なし |
| `card` | `color` | color 値（§6.4） | なし |
| `toc` | `title` | 文字列 | なし |
| `toc` | `depth` | 1–6（見出しレベル） | `3` |

> [!NOTE]
> **なぜフェンスコードブロックなのか**
> プレーン Markdown リーダーには枠付きのコードブロックとして見えるため、directive の境界 — 開始・終了・含まれる本文 — が視認できます。

---

## 3. ネストコンテナ directive

型付きの子を持つ directive（`tabs` は `tab` を、`steps` は `step` を、`grid` は `cell` を持つ）では、フェンスを入れ子にします。§2 と同じフェンス長の規則が適用され、同じフェンス文字を入れ子にする場合は外側を内側より長くします。

子が内側にフェンスを持たない場合は、子が 3 個・コンテナが 4 個が最小です。子の本文が同じフェンス文字のコードブロックを含む場合、そのコードブロックは 3 個のまま、子のフェンスは 4 個、コンテナのフェンスは 5 個になります。

~~~md
````tabs
```tab title="TypeScript"
type User = {
  id: string;
  name: string;
};
```

```tab title="JavaScript"
const user = {
  id: "u_123",
  name: "Ada",
};
```
````
~~~

標準のコンテナ／子ペア:

| コンテナ | 子     |
| -------- | ------ |
| `tabs`    | `tab`    |
| `steps`   | `step`   |
| `grid`    | `cell`   |

子 directive（`tab`, `step`, `cell`）は対応するコンテナの内側で使います。

**使用可能な属性**

| Directive | 属性 | 値 | 既定値 |
| --------- | ---- | -- | ------ |
| `tabs` | `color` | color 値（§6.4） | なし |
| `tab` | `title` | 文字列 | 自動連番（`Tab 1`, `Tab 2`, …） |
| `tab` | `color` | color 値（§6.4） | 親の `tabs` から継承 |
| `steps` | `color` | color 値（§6.4） | なし |
| `step` | `title` | 文字列 | 自動連番（`Step 1`, `Step 2`, …） |
| `step` | `color` | color 値（§6.4） | 親の `steps` から継承 |
| `grid` | `cols` | 正の整数 | `cell` の数 |
| `grid` | `color` | color 値（§6.4） | なし |
| `grid` | `border` | `solid`, `none` | `solid` |
| `cell` | `title` | 文字列 | なし |
| `cell` | `color` | color 値（§6.4） | 親の `grid` から継承 |
| `cell` | `border` | `solid`, `none` | 親の `grid` から継承 |

`title` を省略した場合、レンダラーが出現順に補います（`Tab 1`, `Step 2`, …）。

---

## 4. インライン directive（`` `text`{name attrs} `` 形式）

インライン directive は、文中の短いテキストに装飾を付けるための記法です。表示したい文字をバッククォートで囲み、その直後に `{directive 属性}` を続けます。

~~~md
この API は `Stable`{badge color=green} です。
保存は `Ctrl + S`{kbd} を押します。
更新: `2025-01-01`{small}。
~~~

`{ … }` の最初の語が directive 名です。それ以降は `color=green` のような属性として扱われます。

バッククォートと `{` はつなげて書きます。

```md
`Stable`{badge color=green}
```

間に空白を入れると inline directive にはなりません。

```md
`Stable` {badge color=green}
```

インライン directive は 1 行の中で閉じます。directive の text にバッククォートを含めることはできません。`{...}` を付けずにバッククォートだけを書いた場合は、通常のインラインコードとして扱われます。プレーン Markdown ビューアでは、インラインコードの後ろに `{...}` がそのまま文字として表示されます。

標準のインライン directive:

```text
badge  small  kbd
```

**使用可能な属性**

| Directive | 属性 | 値 | 既定値 |
| --------- | ---- | -- | ------ |
| `badge` | `color` | color 値（§6.4） | なし |
| `small` | なし | - | - |
| `kbd` | なし | - | - |

---

## 5. 見出し属性

見出し属性は ATX 見出し行の末尾に書きます。ATX 見出しとは `# 見出し` の形式です。Setext 見出し（`見出し` の次行に `---` や `===` を置く形式）では見出し属性を使えません。

~~~md
## Button {heading color=blue}
### Props {heading color=green nest}
~~~

末尾に closing `#` sequence を置く形式とは併用しません。つまり、`## Button {heading color=blue}` と書き、`## Button ## {heading color=blue}` とは書きません。

Gloss Markdown として認識されるのは `{heading ATTRS}` 形式だけです。`heading` を省略した `{color=blue}` のような属性ブロックは、通常の見出しテキストとして扱われます。

属性ブロックは見出し全体に適用されます。プレーン Markdown レンダラーでは末尾のテキストとして表示され、見出し本文自体は通常の Markdown テキストのままです。標準の Markdown（GitHub を含む）は見出し属性構文をサポートしないため、Gloss 対応レンダラーがなければ `{heading ...}` の文字列がそのまま見出しの末尾に残ります。

`nest` 属性は、その見出しのセクションを視覚的に 1 段深く表示します。

- **効果:** Markdown 見出しレベルは変えずに、その見出しと本文を視覚的に 1 段インデントします。
- **適用範囲:** その見出しから、同じまたは上位の Markdown 見出しレベルの次の見出しの直前までです。
- **継承:** 下位見出しは親の視覚的なネスト段数を継承します。下位見出しにも `nest` がある場合は、さらに 1 段追加されます。

**使用可能な属性**

| 属性 | 値 | 既定値 |
| ---- | -- | ------ |
| `color` | color 値（§6.4） | なし |
| `nest` | 真偽値（§6.3） | `false` |

---

## 6. 属性構文

属性文法は、すべての directive 形式（§2–§4）と見出し属性（§5）で共通です。

### 6.1 キー・バリューペア

```text
key=value
key="スペースを含む値"
```

- 引用符なしの値は次のホワイトスペースまたは属性リスト末尾まで続きます。
- 引用符付きの値は `"` で区切られ、同一行内で閉じる必要があります。
- インライン directive と見出し属性の `{...}` 内では、属性値に `}` を含めることはできません。フェンス形式の directive では、`}` は通常の属性値の文字として扱われます。
- 未知の属性は無視されます。
- 不正な値は、その属性の既定値として扱われます。既定値がない属性では、属性が指定されていないものとして扱われます。

### 6.2 エスケープシーケンス（引用符付き値の中）

| シーケンス | 意味                       |
| ---------- | -------------------------- |
| `\"`       | ダブルクォートのリテラル   |
| `\\`       | バックスラッシュのリテラル |

### 6.3 真偽値

真偽値属性の値は `true` または `false` のみです。値は小文字で書きます。`True`、`0`、`yes` などは不正な値です。

真偽値属性だけは `=value` を省略できます。`=value` のない真偽値属性は `key="true"` として扱われます。

真偽値属性ではない属性で `=value` を省略した場合、その属性指定は不正として扱われ、属性が指定されていないものとして扱われます。

~~~md
```details title="詳細" open
ページを開いたときから展開された状態で表示されます。
```
~~~

### 6.4 color 値

すべての `color` 属性は同じパレットを使います。

```text
gray  blue  green  yellow  red  purple
```

実際に描画される色は各レンダラーの実装に委ねられます。

### 6.5 リンク先（`href`）

`href` 属性は、次の形式のリンク先だけを受け付けます。

- `http://...` と `https://...` の URL
- `#anchor` のような fragment link
- `/docs/guide.md` のような root-relative path
- `./guide.md` や `../guide.md` のような dot-relative path
- `foo.md` や `docs/guide.md` のような、URI scheme を持たない裸の相対パス

次の形式は無効な値として**拒否**されます。

- `javascript:`、`data:`、`vbscript:` などの危険な scheme
- `//example.com` のような protocol-relative URL
- 上に挙げていないその他の URI scheme

他の無効な値（§6.1）と同様に、拒否された `href` は指定されていないものとして扱われ、リンクは生成されません。

---

## 7. 識別子ルール

| 識別子        | 使用可能な文字          | 例                           |
| ------------- | ----------------------- | ---------------------------- |
| directive 名  | `[A-Za-z][A-Za-z0-9-]*` | `tabs`, `details` |
| 属性名        | `[A-Za-z][A-Za-z0-9-]*` | `color`, `title`, `border` |

名前は**大文字小文字を区別しません**。`Details`、`details`、`DETAILS` はすべて同じ directive として扱われます。

Gloss Markdown ツールは、directive 名と属性名を小文字に正規化してからレンダリングします。

慣例として、Gloss 独自の directive（`toc`、`details`、`badge` など）は**小文字**で書きます。GitHub native の Markdown 機能は GitHub 側の記法に従います。

---

## 8. シンタックスハイライト

Gloss Markdown は、コードブロックの syntax highlighting 用に独自の directive を追加しません。通常の Markdown と同じように、フェンスの info string に言語名を書きます。

~~~md
```ts
type User = {
  id: string;
  name: string;
};
```
~~~

`ts`、`typescript`、`js`、`javascript`、`json`、`bash` など、どの言語名がハイライトされるかは使用するレンダラーに依存します。未対応の言語名は通常のコードブロックとして表示されます。

### 8.1 ファイル名ラベル（Gloss 拡張）

**言語名の後ろに** `filename="path"` を付けると、Gloss 対応レンダラーではブロックの上にファイル名ラベルを表示できます。通常のハイライト付きコードブロックと同様に、言語名を info string の先頭に書き、`filename` はその後ろに置きます。これは GFM 標準の機能ではなく、Gloss Markdown 独自のコードブロック拡張です。通常の Markdown レンダラーでは、info string の一部として扱われます。

~~~md
```ts filename="src/types.ts"
type User = { id: string; name: string };
```
~~~

フェンスの info string の最初の空白区切り token が登録済み directive 名と完全一致する場合、そのフェンスはコードブロックではなく Gloss directive として扱われます。たとえば `details` は directive ですが、`details-foo` や `details.txt` は `details` directive ではありません。

~~~md
```details title="補足"
これは `details` directive です。
```
~~~

directive の本文内に同じフェンス文字の syntax highlight 付きコードブロックを書きたい場合は、外側の directive フェンスを内側のコードフェンスより長くします — §2 と同じフェンス長の規則です。

~~~~md
````details title="TypeScript の例"
```ts
const label = "Workspace name";
```
````
~~~~

---

## 9. GitHub Markdown passthrough

Gloss Markdown は GitHub native の Markdown 記法を再定義しません。Alert、脚注、数式、task list、table、取り消し線、autolink、通常の code highlighting は passthrough Markdown です。これらは Gloss directive ではなく、入力に含まれていてもそのまま Markdown レンダラーに渡されます。

対応状況や細かい規則は GitHub Docs に従います。
