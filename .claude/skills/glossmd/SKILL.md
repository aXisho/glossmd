---
name: glossmd
description: Use this skill when the user asks to "read a glossmd file", "write glossmd", "create a gloss markdown document", "edit a gloss markdown file", or wants to view, create, or modify `.gloss.md` files using proper Gloss Markdown syntax.
argument-hint: "[file-path] [instruction]"
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
---

# glossmd

Read and write Gloss Markdown files (`.gloss.md`).

## Arguments

`$ARGUMENTS` receives a file path and optional instruction.

- Read: `path/to/file.gloss.md` — read the file and explain or display its content.
- Write: `path/to/file.gloss.md "instruction"` — create a new file using proper Gloss Markdown syntax.
- Edit: `path/to/file.gloss.md "change instruction"` — edit an existing file using proper Gloss Markdown syntax.

## Gloss Markdown Syntax Reference

### File extension

Gloss Markdown files use the `.gloss.md` extension. They are fully compatible with Markdown and GitHub.

---

### GitHub Markdown passthrough

Gloss Markdown is a GFM extension. GitHub Markdown features that already have GitHub-native syntax are not Gloss directives. Preserve them as Markdown and let the Markdown renderer handle them.

Use this section for GitHub Alerts, footnotes, math, task lists, tables, strikethrough, autolinks, and other GitHub-supported Markdown features unless the main Gloss syntax explicitly defines a Gloss directive for the same construct.

Do not report passthrough notation as a Gloss directive.

```md
> [!NOTE]
> GitHub Alert syntax is passed through.

Here is a footnote reference.[^1]

[^1]: Footnote text.

$$
E = mc^2
$$
```

For GitHub Alerts, use GitHub's standard alert types and formatting. Gloss Markdown does not add custom alert titles or alert directives.

---

### Block directives (fenced code block form)

````md
```details title="Section title"
Collapsible body. **Markdown** is parsed.
```

```details title="Open by default" open
Expanded on load.
```

```card title="Card title"
Card body content.
```

```card title="Linked card" href="https://example.com"
Click to navigate.
```
````

A directive with no body (currently only `toc`) uses the same form with an empty body — no lines between the opening and closing fence.

````md
```toc title="Contents" depth=3
```
````

Standard block directives: `details` `card` `toc`

**Available attributes**

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `details` | `title` | string | `Details` |
| `details` | `open` | `true` / `false`; `open` alone means `true` | `false` |
| `details` | `color` | color value (see Color values) | none |
| `card` | `title` | string | none |
| `card` | `href` | URL | none |
| `card` | `color` | color value (see Color values) | none |
| `toc` | `title` | string | none |
| `toc` | `depth` | 1–6 (heading level) | `3` |

---

### Nested container directives

**tabs:**

`````md
````tabs
```tab title="Tab 1"
Content for tab 1.
```

```tab title="Tab 2"
Content for tab 2.
```
````
`````

**steps:**

`````md
````steps
```step title="Step 1"
Do this first.
```

```step title="Step 2"
Do this next.
```
````
`````

**grid:**

`````md
````grid
```cell title="Item A"
Description of A.
```

```cell title="Item B"
Description of B.
```
````
`````

**Available attributes**

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `tabs` | `color` | color value (see Color values) | none |
| `tab` | `title` | string | `Tab 1`, `Tab 2`, … |
| `tab` | `color` | color value (see Color values) | inherited from `tabs` |
| `steps` | `color` | color value (see Color values) | none |
| `step` | `title` | string | `Step 1`, `Step 2`, … |
| `step` | `color` | color value (see Color values) | inherited from `steps` |
| `grid` | `cols` | positive integer | number of `cell` children |
| `grid` | `border` | `solid`, `none` | `solid` |
| `grid` | `color` | color value (see Color values) | none |
| `cell` | `title` | string | none |
| `cell` | `color` | color value (see Color values) | inherited from `grid` |
| `cell` | `border` | `solid`, `none` | inherited from `grid` |

---

### Inline directives

```md
`Stable`{badge color=green}
`Beta`{badge color=yellow}
`Deprecated`{badge color=red}

`Ctrl+S`{kbd}
`npm run dev`{kbd}

`v1.2.0`{small}
```

Standard inline directives: `badge` `small` `kbd`

**Available attributes**

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `badge` | `color` | color value (see Color values) | none |
| `small` | none | — | — |
| `kbd` | none | — | — |

---

### Heading attributes

Heading attributes are written at the end of an ATX heading line (`# Heading`). Do not use heading attributes with Setext headings (`Heading` followed by `---` or `===`) or with closing `#` sequences such as `## Heading ##`. Use them to add a background color while keeping the heading level, slug, and TOC behavior. Without a Gloss-aware renderer (e.g. on GitHub), the literal `{heading ...}` string remains visible at the end of the heading, since standard Markdown does not support heading attribute syntax.

`nest` applies to the heading's section by one visual level, from that heading until the next heading with the same or higher Markdown heading level. Child headings inherit the parent section's nesting level; if a child heading also has `nest`, it adds one more level.

```md
## ComponentName {heading color=blue}
### props {heading color=green}
#### returns {heading color=gray nest}
```

Available attributes: `color` (color value) and `nest` (`true` / `false`; `nest` alone means `true`; default `false`).

---

### Code blocks with filename label

Add `filename="path"` after the language name to display a file path label above the block.

~~~md
```ts filename="src/types.ts"
type User = { id: string; name: string };
```
~~~

---

### Attribute syntax

```text
key=value
key="value with spaces"
key           # boolean shorthand → key="true"
```

Escape sequences inside quoted values: `\"` `\\`

### Color values

All `color` attributes use this palette:

```text
gray  blue  green  yellow  red  purple
```

---

## Steps

### Reading

1. Extract the file path from `$ARGUMENTS`.
2. Read the file content.
3. Display and explain the content, describing the Gloss Markdown directives used.

### Creating

1. Extract the output path and creation instruction from `$ARGUMENTS`.
2. Compose the document using Gloss Markdown syntax.
3. Write the file.
4. Report which directives were used.

### Editing

1. Extract the file path and change instruction from `$ARGUMENTS`.
2. Read the current content.
3. Apply the changes, keeping Gloss Markdown syntax correct.
4. Report what was changed.

## Notes

- **GFM compatibility** — `.gloss.md` files must remain readable as plain Markdown on GitHub.
- **GitHub Markdown passthrough** — alerts, footnotes, math, task lists, tables, strikethrough, autolinks, and other GitHub-native Markdown features are not Gloss directives.
- **Fence length** — Gloss Markdown follows the CommonMark/GFM fence rule. An opening fence uses at least three matching characters (backticks or tildes), and its closing fence uses the same character with at least the same length. You only need to make the outer fence longer than the inner one when the body contains a fence of the same kind; with no nested fences, three characters are enough.
- **Lowercase directives** — directive names and attribute names are case-insensitive; normalize to lowercase.
- **Case convention** — custom Gloss directives (`toc`, `details`, `badge`, …) use lowercase in examples.
- **No space before `{`** — inline directives must have the backtick and `{` immediately adjacent.
