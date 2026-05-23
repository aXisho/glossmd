# Gloss Markdown — Notation Guide

[English](./syntax.md) | [日本語](./syntax.ja.md) | [简体中文](./syntax.zh-CN.md)

> [!NOTE]
> **Contents**
> 1. File extension
> 2. Block directives (fenced form)
> 3. Nested container directives
> 4. Inline directives (`` `text`{name attrs} `` form)
> 5. Heading attributes
> 6. Attribute syntax
> 7. Identifier rules
> 8. Syntax highlighting
> 9. GitHub Markdown passthrough

Gloss Markdown is based on GitHub Flavored Markdown (GFM). It adds a lightweight **inline gloss layer** on top of GFM. This guide explains that notation. Every directive form is designed so that standard Markdown renderers such as GitHub and VS Code Preview can still read the meaning, while Gloss Markdown-aware renderers can produce richer output.

GitHub.com-specific Markdown features such as Alerts, footnotes, math, task lists, tables, strikethrough, autolinks, and ordinary code highlighting are not defined as Gloss Markdown notation. If they appear in the input, they are not interpreted as Gloss directives; they are left for the Markdown renderer to handle.

> [!NOTE]
> **About the examples in this guide**
> Code blocks that *show* Gloss Markdown source are fenced with tildes (`~~~md`) so that the inner backtick fences stay intact. In actual `.gloss.md` files, use backtick fences (`` ``` `` or `` ```` ``) as usual.

---

## 1. File extension

Gloss Markdown files use the `.gloss.md` extension. The notation is layered on Markdown that remains valid GFM, so standard renderers such as github.com can still show the source as ordinary Markdown.

Without a dedicated viewer, Gloss directives mainly appear as ordinary code blocks or text.

---

## 2. Block directives (fenced form)

Block directives are written as **fenced code blocks** whose info string contains the directive name and attributes.

~~~md
```details color=blue title="Note"
Write the body here. **Markdown** is parsed.
```
~~~

- Opening fence: the info string holds `NAME` and optional `ATTRS`. The directive name comes first, followed by attributes.
- **Fence length:** Gloss Markdown follows the CommonMark/GFM fence rule. An opening fence uses at least three matching characters (backticks or tildes), and its closing fence uses the same character with at least the same length. You only need to make the outer fence longer than the inner one when the body contains a fence of the same kind; with no nested fences, three characters are enough.
- In a Gloss-aware renderer, the body between the fences is parsed as Markdown. In a plain Markdown viewer, the body is displayed as a code block, so the grouping and scope remain visible.

A directive with no body (currently only `toc`) uses the same form with an empty body — no lines between the opening and closing fence.

~~~md
```toc title="Contents" depth=3
```
~~~

Standard directives written in this form:

```text
details  card  toc
```

The following fence info names are reserved for Gloss Markdown directives. When the first whitespace-separated token of a fence info string is one of these names, Gloss-aware renderers treat the fence as a directive instead of a normal code block.

```text
details  card  toc  tabs  tab  steps  step  grid  cell
```

**Available attributes**

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `details` | `title` | string | `Details` |
| `details` | `open` | boolean (§6.3) | `false` |
| `details` | `color` | color value (§6.4) | none |
| `card` | `title` | string | none |
| `card` | `href` | link target (§6.5) | none |
| `card` | `color` | color value (§6.4) | none |
| `toc` | `title` | string | none |
| `toc` | `depth` | 1–6 (heading level) | `3` |

> [!NOTE]
> **Why fenced code blocks?**
> In a plain Markdown reader, the directive appears as a framed code block, so the directive boundary — start, end, and body — remains visible.

---

## 3. Nested container directives

Directives with typed children (`tabs` has `tab`, `steps` has `step`, `grid` has `cell`) nest one set of fences inside another. The same fence-length rule from §2 applies: when nested fences use the same fence character, the outer fence must be longer than the inner one.

So when the children carry no inner fences, three backticks for each child and four for the container is the minimum. If a child's body contains a code block using the same fence character, that code block stays at three, the child fence grows to four, and the container fence to five.

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

Standard container / child pairs:

| Container | Child |
| --------- | ----- |
| `tabs` | `tab` |
| `steps` | `step` |
| `grid` | `cell` |

Use child directives (`tab`, `step`, `cell`) inside their matching container.

**Available attributes**

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `tabs` | `color` | color value (§6.4) | none |
| `tab` | `title` | string | auto-numbered (`Tab 1`, `Tab 2`, …) |
| `tab` | `color` | color value (§6.4) | inherited from parent `tabs` |
| `steps` | `color` | color value (§6.4) | none |
| `step` | `title` | string | auto-numbered (`Step 1`, `Step 2`, …) |
| `step` | `color` | color value (§6.4) | inherited from parent `steps` |
| `grid` | `cols` | positive integer | number of `cell` children |
| `grid` | `color` | color value (§6.4) | none |
| `grid` | `border` | `solid`, `none` | `solid` |
| `cell` | `title` | string | none |
| `cell` | `color` | color value (§6.4) | inherited from parent `grid` |
| `cell` | `border` | `solid`, `none` | inherited from parent `grid` |

When a `title` is omitted, the renderer fills it in by position (`Tab 1`, `Step 2`, …).

---

## 4. Inline directives (`` `text`{name attrs} `` form)

Inline directives are notation for decorating short text inside a sentence. Wrap the visible text in backticks, then immediately follow it with `{directive attributes}`.

~~~md
This API is `Stable`{badge color=green}.
Press `Ctrl + S`{kbd} to save.
Updated: `2025-01-01`{small}.
~~~

The first word inside `{ … }` is the directive name. Anything after it is treated as an attribute, such as `color=green`.

Write the backtick and `{` together.

```md
`Stable`{badge color=green}
```

If you insert a space, it is not an inline directive.

```md
`Stable` {badge color=green}
```

Inline directives must close on the same line. The directive text cannot contain backticks. If you write only backticks without `{...}`, it is treated as normal inline code. In a plain Markdown viewer, the text shows as inline code followed by the literal `{...}`.

Standard inline directives:

```text
badge  small  kbd
```

**Available attributes**

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `badge` | `color` | color value (§6.4) | none |
| `small` | none | - | - |
| `kbd` | none | - | - |

---

## 5. Heading attributes

Heading attributes are written at the end of an ATX heading line. An ATX heading is the `# Heading` form. Setext headings (`Heading` followed by `---` or `===`) do not support heading attributes.

~~~md
## Button {heading color=blue}
### Props {heading color=green nest}
~~~

Use the form without a closing `#` sequence. In other words, write `## Button {heading color=blue}`, not `## Button ## {heading color=blue}`.

Only the `{heading ATTRS}` form is recognized as Gloss Markdown. Attribute blocks without `heading`, such as `{color=blue}`, are treated as ordinary heading text.

The attribute block applies to the whole heading. In plain Markdown renderers, it appears as trailing text while the heading text itself remains normal Markdown text. Without a Gloss-aware renderer (e.g. on GitHub), the literal `{heading ...}` string remains visible at the end of the heading, since standard Markdown does not support heading attribute syntax.

The `nest` attribute shifts a heading's section one visual level deeper:

- **Effect:** the heading and its body are indented by one visual level, without changing the Markdown heading level.
- **Scope:** the shift starts at that heading and continues until the next heading with the same or higher Markdown heading level.
- **Inheritance:** child headings inherit the parent's visual nesting level. A child with its own `nest` adds one more level.

**Available attributes**

| Attribute | Values | Default |
| --------- | ------ | ------- |
| `color` | color value (§6.4) | none |
| `nest` | boolean (§6.3) | `false` |

---

## 6. Attribute syntax

The attribute syntax is shared by all directive forms (§2–§4) and heading attributes (§5).

### 6.1 Key-value pairs

```text
key=value
key="value with spaces"
```

- Unquoted values continue until the next whitespace or the end of the attribute list.
- Quoted values are delimited by `"` and must close on the same line.
- In inline directives and heading attributes, attribute values inside `{...}` cannot contain `}`. In fenced directives, `}` is treated as an ordinary character in an attribute value.
- Unknown attributes are ignored.
- An invalid value is treated as the attribute's default. If the attribute has no default, it is treated as if it were not specified.

### 6.2 Escape sequences (inside quoted values)

| Sequence | Meaning |
| -------- | ------- |
| `\"` | Literal double quote |
| `\\` | Literal backslash |

### 6.3 Boolean values

Boolean attributes only accept `true` or `false`. Write boolean values in lowercase. Values such as `True`, `0`, and `yes` are invalid.

Only boolean attributes may omit `=value`. A boolean attribute key without `=value` is treated as `key="true"`.

If a non-boolean attribute omits `=value`, that attribute is invalid and is treated as if it were not specified.

~~~md
```details title="Details" open
This section is expanded when the page loads.
```
~~~

### 6.4 Color values

All `color` attributes use the same palette.

```text
gray  blue  green  yellow  red  purple
```

The exact rendered colors are implementation details of each renderer.

### 6.5 Link targets (`href`)

The `href` attribute accepts only the following link target forms:

- `http://...` and `https://...` URLs
- fragment links such as `#anchor`
- root-relative paths such as `/docs/guide.md`
- dot-relative paths such as `./guide.md` and `../guide.md`
- bare relative paths without a URI scheme, such as `foo.md` and `docs/guide.md`

These forms are **rejected** as invalid values:

- dangerous schemes such as `javascript:`, `data:`, and `vbscript:`
- protocol-relative URLs such as `//example.com`
- any other URI scheme not listed above

As with any invalid value (§6.1), a rejected `href` is treated as unspecified, so no link is generated.

---

## 7. Identifier rules

| Identifier | Allowed characters | Example |
| ---------- | ------------------ | ------- |
| Directive name | `[A-Za-z][A-Za-z0-9-]*` | `tabs`, `details` |
| Attribute name | `[A-Za-z][A-Za-z0-9-]*` | `color`, `title`, `border` |

Names are **case-insensitive**. `Details`, `details`, and `DETAILS` are all treated as the same directive.

Gloss Markdown tools normalize directive names and attribute names to lowercase before rendering.

By convention, custom Gloss directives (`toc`, `details`, `badge`, …) are written in **lowercase**. GitHub-native Markdown features keep GitHub's own notation.

---

## 8. Syntax highlighting

Gloss Markdown does not add a separate directive for syntax highlighting. Use normal Markdown code fences and put the language name in the fence info string.

~~~md
```ts
type User = {
  id: string;
  name: string;
};
```
~~~

Renderer support determines which language names are highlighted. Common names such as `ts`, `typescript`, `js`, `javascript`, `json`, and `bash` are expected to work in Gloss-aware renderers; unsupported names fall back to a normal code block.

### 8.1 File name labels (Gloss extension)

Add `filename="path"` **after the language name** to display a file path label above the block in Gloss-aware renderers. The language name must come first in the info string, as in any highlighted code block; `filename` follows it. This is not a standard GFM feature; it is a Gloss Markdown code block extension. Standard Markdown renderers treat it as part of the fence info string.

~~~md
```ts filename="src/types.ts"
type User = { id: string; name: string };
```
~~~

If the first whitespace-separated token of a fence info string exactly matches a registered directive name, the fence is treated as a Gloss directive, not as a highlighted code block. For example, `details` is a directive, but `details-foo` and `details.txt` are not `details` directives.

~~~md
```details title="Note"
This is a `details` directive.
```
~~~

To put a syntax-highlighted code block using the same fence character inside a directive body, make the outer directive fence longer than the inner code fence — the same fence-length rule as §2.

~~~~md
````details title="TypeScript example"
```ts
const label = "Workspace name";
```
````
~~~~

---

## 9. GitHub Markdown passthrough

Gloss Markdown does not redefine GitHub-native Markdown notation. Alerts, footnotes, math, task lists, tables, strikethrough, autolinks, and ordinary code highlighting are passthrough Markdown. They are not Gloss directives; if they appear in the input, Gloss Markdown leaves them for the Markdown renderer to handle.

Support and detailed rules follow GitHub Docs.
