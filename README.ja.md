# Gloss Markdown

[English](./README.md) | [日本語](./README.ja.md) | [简体中文](./README.zh-CN.md)

**GitHub で Markdown を管理する人のための、Human / AI フレンドリーな GFM 拡張フォーマット**

Gloss Markdown は、GitHub Flavored Markdown (GFM) の上に重ねる、扱いやすく視認性の高い拡張書式です。主に AI が書いたドキュメントを GitHub 上で管理する人のために作られています。ソースは標準的な Markdown ツールでも読むことができ、Gloss Markdown 専用ツールを使うとよりリッチで見やすいドキュメントにアップグレードされます。

Markdown は人間にも LLM にも扱いやすい形式です。HTML は強力ですが、README、Spec には過剰であり、GitHub で管理するドキュメントには適しません。Gloss Markdown の表現力は GFM と HTML の中間にあります。Gloss Markdown は GitHub 上で管理されるドキュメントにとってちょうどよい、小さく予測可能な directive セットです。

~~~md
> [!WARNING]
> **本番データ** — この移行は保存済みレコードを変更します。

````tabs
```tab title="CLI"
glmo docs/ --watch
```

```tab title="GitHub"
GlossView for GitHub Chrome extension をインストールし、任意の `.gloss.md` ファイルを開きます。
```
````

この API は `Stable`{badge color=green} です。保存するには `Ctrl + S`{kbd} を押します。
~~~

GitHub 上でも読める記法。よりよい視認性。少ない token。

## なぜ Gloss Markdown か

- **リッチなドキュメント UI** - details、tabs、cards、badges などの構造化されたドキュメント要素を提供します。
- **専用ビューアなしでも読める** - `.gloss.md` ファイルは GitHub、VS Code、terminal、code review ツール上で普通の Markdown として読めます。
- **小さな語彙** - details、card、tabs、steps、grid、table of contents、badge、keyboard key、少数のインラインヘルパーのみのシンプルな仕様です。
- **AI 出力が揃いやすい** - 書式の選択肢が少ないため、LLM の出力のブレが小さくなり、整った統一感のあるドキュメントを生成できます。
- **少ないトークン消費** - directive はコンパクトな Markdown パターンです。冗長な HTML や JSX component ではありません。
- **安全なレンダリングモデル** - Gloss Markdown は data-only なので、AI 出力を安全にレンダリングできます。
- **専用レンダラー** - GitHub 向けに Chrome 拡張 `GlossView for GitHub`、ローカル向けに [k1LoW/mo](https://github.com/k1LoW/mo) fork の `glmo` が用意されています。

## コンセプト

AI 生成ドキュメントは、だいたい次のどちらかに寄りがちです。

| アプローチ | 問題 |
| ---------- | ---- |
| Plain Markdown | 編集しやすいものの、表現力に乏しく、LLM が生成する長文ドキュメントの視認性は低いです。 |
| HTML | リッチですが、多くのドキュメントには過剰です。GitHub 上でプレビューが出来ません。自由度が高いため統一感のあるフォーマットを維持するのも大変です。 |

Gloss Markdown は、GitHub Flavored Markdown の上に inline gloss layer を追加します。このレイヤーはコードレビューや fallback rendering ではノイズにならない程度の軽量な表記で表され、Gloss Markdown 用のレンダラーが tabs、grids、badges などの程よくリッチで見やすい documentation UI に変換できる程度に構造化されています。

> [!NOTE]
> Gloss Markdown は GitHub Flavored Markdown の置き換えではありません。
> GitHub Flavored Markdown に小さく安全な語彙を追加するものです。

## ファイル拡張子

Gloss Markdown のファイルは `.gloss.md` 拡張子を使います。専用の Gloss Markdown ビューワーがない場合でも、GitHub、VS Code、terminal、code review ツール上で普通の Markdown として読めます。

## クイックスタート

### GitHub 互換記法

GitHub Alert、脚注、数式は Gloss directive ではありません。GitHub.com と同様に使えます。

```md
> [!TIP]
> **Component API** — UI コンポーネントの props、使用例、移行メモを書くときに使います。
```

### ブロック

Block directive は fenced code block を使います。本文を持たない directive（`toc` など）は、同じ形式で本文を空にして書きます。

~~~md
```details title="インストール"
npm install @acme/ui
```
~~~

標準 block directive:

```text
details  card  toc
```

### コンテナ

tabs や steps のようなネストされた構造は、長い outer fence と通常の inner fence で表現します。

~~~md
````tabs
```tab title="TypeScript"
type ButtonProps = {
  variant: "primary" | "secondary";
  disabled?: boolean;
};
```

```tab title="JavaScript"
const props = {
  variant: "primary",
  disabled: false,
};
```
````
~~~

標準 container pair:

| Container | Child |
| --------- | ----- |
| `tabs` | `tab` |
| `steps` | `step` |
| `grid` | `cell` |

### インライン directive

Inline directive は inline code span に attribute block を続けて書きます。

```md
Button API は `Stable`{badge color=green} です。
ローカルサーバーは `npm run dev`{kbd} で起動します。
```

標準 inline directive:

```text
badge  small  kbd
```

脚注や数式も GitHub.com と同様に使用できます。

### 見出し属性

見出し属性は見出しに背景色を付けます。

```md
## Button {heading color=blue}
### Props {heading color=green}
```

## ツール

| ツール | 用途 |
| ---- | ------- |
| [glmo](https://github.com/aXisho/glmo) | `.md`, `.gloss.md` ファイル向けのローカル browser viewer。[k1LoW/mo](https://github.com/k1LoW/mo) の fork であることを明示し、Gloss Markdown rendering を追加しています。 |
| [GlossView for GitHub](https://github.com/aXisho/glossview-for-github) | GitHub file page 上で Gloss Markdown directive を render する Chrome extension。 |

## ライセンス

[MIT License](./LICENSE)

Gloss Markdown は GFM / CommonMark 互換の拡張フォーマットとして設計されています。上流の CommonMark 仕様および GFM 仕様は、それぞれ Creative Commons Attribution-ShareAlike 4.0 International でライセンスされています。詳細は [Third-Party Notices](./THIRD_PARTY_NOTICES.md) を参照してください。
