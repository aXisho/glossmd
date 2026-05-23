---
name: md-to-glossmd
description: Use this skill when the user asks to "convert markdown to glossmd", "transform md to gloss markdown", "upgrade markdown with glossmd directives", or wants plain Markdown rewritten using Gloss Markdown syntax.
argument-hint: "[file-or-content]"
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
---

# md-to-glossmd

Convert plain Markdown to Gloss Markdown format.

## Arguments

`$ARGUMENTS` receives a file path or a description of what to convert.
- File path: Read the file, apply conversions, and write the result as a `.gloss.md` file.
- Direct content: Convert the provided content and output it.

## Conversion Rules

### 1. Callouts (GitHub Alert → Gloss Markdown callout)

GitHub Alert syntax is already valid Gloss Markdown callout syntax — no change needed. The `[!TYPE]` line must have nothing after it; the type's default title is used automatically.

```md
> [!NOTE]
> [!TIP]
> [!IMPORTANT]
> [!WARNING]
> [!CAUTION]
```

### 2. Code blocks → details / card

Long explanatory code blocks or collapsible sections become `details`:

````md
```details title="Installation"
npm install @acme/ui
```
````

Standalone information panels become `card`:

````md
```card title="Prerequisites"
- Node.js 18+
- pnpm 8+
```
````

A card with a link destination uses `href`:

````md
```card title="API Reference" href="https://example.com/api"
Full API documentation.
```
````

### 3. Parallel alternatives → tabs

Patterns showing the same thing in multiple languages or environments become a `tabs` container:

`````md
````tabs
```tab title="TypeScript"
// TypeScript code
```

```tab title="JavaScript"
// JavaScript code
```
````
`````

### 4. Numbered procedures → steps

Ordered lists describing sequential steps become `steps`:

`````md
````steps
```step title="Install dependencies"
npm install
```

```step title="Create config file"
cp .env.example .env
```
````
`````

### 5. Side-by-side items → grid

Items meant to appear side by side become `grid`:

`````md
````grid
```cell title="Fast"
Improved processing speed.
```

```cell title="Safe"
Type-safe implementation.
```
````
`````

### 6. Table of contents → toc (empty-body block directive)

A table of contents section at the top of a document becomes `toc`:

````md
```toc title="Contents" depth=3
```
````

### 7. Inline decorations

| Before | After |
| ------ | ----- |
| Status string to highlight | `` `Stable`{badge color=green} `` |
| Keyboard shortcut | `` `Ctrl+S`{kbd} `` |
| Small supplementary text | `` `v1.2.0`{small} `` |

Badge colors: `gray` `blue` `green` `yellow` `red` `purple`

### 8. Heading attributes

For API docs or sections that benefit from color-coded headings:

```md
## Button {heading color=blue}
### Props {heading color=green}
```

### 9. Code blocks with filename label

When a code block represents a specific file, add `filename="path"` after the language name:

~~~md
```ts filename="src/types.ts"
type User = { id: string; name: string };
```
~~~

### 10. Math

Gloss Markdown adds no directive for math — use GitHub's standard math syntax. Block: `$$ … $$`. Inline: `$ … $`.

~~~md
$$
E = mc^2
$$

The equation $E = mc^2$ relates mass and energy.
~~~

## Conversion Principles

1. **Preserve GFM compatibility** — the output `.gloss.md` must still read naturally as plain Markdown on GitHub.
2. **Minimal conversion** — only convert where there is a clear benefit; do not force every element into a directive.
3. **No semantic change** — the meaning and information of the document must remain identical after conversion.
4. **Output extension is `.gloss.md`**.

## Steps

1. Read the input file or inspect the provided content.
2. Identify conversion candidates by applying the rules above.
3. Produce the converted content.
4. If a file path was given, write the result as a `.gloss.md` file.
5. Report a summary of what was converted.
