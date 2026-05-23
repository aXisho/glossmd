# Gloss Markdown

[English](./README.md) | [日本語](./README.ja.md) | [简体中文](./README.zh-CN.md)

**A human- and AI-friendly GFM extension format for people who manage Markdown on GitHub**

Gloss Markdown is an easy-to-use, highly readable extension format layered on GitHub Flavored Markdown (GFM). It is designed primarily for people who manage AI-written documentation on GitHub. The source remains readable in standard Markdown tools, while dedicated Gloss Markdown tools can upgrade it into richer, easier-to-scan documentation.

Markdown is friendly to both humans and LLMs. HTML is powerful, but it is too much for README files and specs, and it does not fit well with GitHub-managed documentation. Gloss Markdown sits between GFM and HTML. It provides a small, predictable directive set that is just right for documentation managed on GitHub.

~~~md
> [!WARNING]
> **Production data** — this migration changes saved records.

````tabs
```tab title="CLI"
glmo docs/ --watch
```

```tab title="GitHub"
Install GlossView for GitHub chrome extension and open any `.gloss.md` file.
```
````

This API is `Stable`{badge color=green}. Press `Ctrl + S`{kbd} to save.
~~~

GitHub-readable notation. Better readability. Fewer tokens.

## Why Gloss Markdown

- **Rich documentation UI** - Provides structured documentation elements such as details, tabs, cards, and badges.
- **Readable without a dedicated viewer** - `.gloss.md` files are still readable as ordinary Markdown on GitHub, VS Code, terminals, and code review tools.
- **Small vocabulary** - A simple specification with only details, cards, tabs, steps, grids, table of contents, badges, keyboard keys, and a few inline helpers.
- **Consistent AI output** - Fewer formatting choices reduce LLM output variance and help generate polished, unified documentation.
- **Low token usage** - Directives are compact Markdown patterns, not verbose HTML or JSX components.
- **Safe rendering model** - Gloss Markdown is data-only, so AI output can be rendered safely.
- **Dedicated renderers** - Use `GlossView for GitHub`, a Chrome extension for GitHub, and `glmo`, a fork of [k1LoW/mo](https://github.com/k1LoW/mo), for local viewing.

## Concept

AI-generated documentation usually falls into one of two shapes:

| Approach | Problem |
| -------- | ------- |
| Plain Markdown | Easy to edit, but limited in expressiveness, and long LLM-generated documents are not very scannable. |
| HTML | Rich, but too much for most documentation. It cannot be previewed naturally on GitHub, and its high freedom makes consistent formatting harder to maintain. |

Gloss Markdown adds an inline gloss layer on top of GitHub Flavored Markdown. This layer is lightweight enough to avoid becoming noise during code review or fallback rendering, but structured enough for Gloss Markdown renderers to turn it into moderately rich, readable documentation UI such as tabs, grids, and badges.

> [!NOTE]
> Gloss Markdown does not replace GitHub Flavored Markdown.
> It adds a small, safe vocabulary to GitHub Flavored Markdown.

## File Extension

Gloss Markdown files use the `.gloss.md` extension. They remain readable as ordinary Markdown on GitHub, VS Code, terminals, and code review tools even without a dedicated Gloss Markdown viewer.

## Quick Start

### GitHub-Compatible Notation

GitHub Alerts, footnotes, and math are not Gloss directives. Use them the same way they are used on GitHub.com.

```md
> [!TIP]
> **Component API** — use this when documenting props, examples, and migration notes for a UI component.
```

### Blocks

Block directives use fenced code blocks. A directive with no body (such as `toc`) uses the same form with an empty body.

~~~md
```details title="Install"
npm install @acme/ui
```
~~~

Standard block directives:

```text
details  card  toc
```

### Containers

Nested structures such as tabs and steps are expressed with a longer outer fence and normal inner fences.

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

Standard container pairs:

| Container | Child |
| --------- | ----- |
| `tabs` | `tab` |
| `steps` | `step` |
| `grid` | `cell` |

### Inline Directives

Inline directives are written as an inline code span followed by an attribute block.

```md
The Button API is `Stable`{badge color=green}.
Run `npm run dev`{kbd} to start the local server.
```

Standard inline directives:

```text
badge  small  kbd
```

Footnotes and math can also be used the same way they are used on GitHub.com.

### Heading Attributes

Heading attributes add a background color to a heading.

```md
## Button {heading color=blue}
### Props {heading color=green}
```

## Tooling

| Tool | Purpose |
| ---- | ------- |
| [glmo](https://github.com/aXisho/glmo) | Local browser viewer for `.md` and `.gloss.md` files. It is explicitly a fork of [k1LoW/mo](https://github.com/k1LoW/mo), extended with Gloss Markdown rendering. |
| [GlossView for GitHub](https://github.com/aXisho/glossview-for-github) | Chrome extension that renders Gloss Markdown directives on GitHub file pages. |

## License

[MIT License](./LICENSE)

Gloss Markdown is designed as an extension format layered on GFM/CommonMark-style Markdown. The upstream CommonMark and GFM specifications are separately licensed under Creative Commons Attribution-ShareAlike 4.0 International; see [Third-Party Notices](./THIRD_PARTY_NOTICES.md).
