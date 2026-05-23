# Gloss Markdown — Complete Reference {heading color=blue}

```toc title="Contents" depth=3
```

A comprehensive reference for Gloss Markdown directives and GitHub-compatible notation. `.gloss.md` files render as plain Markdown on GitHub and as a rich UI in Gloss-aware renderers.

---

## GitHub Alerts

GitHub Alerts are not Gloss directives. They can be used the same way they are used on GitHub.com.

> [!NOTE]
> Use for informational content. **Markdown** is parsed inside GitHub Alerts.

> [!TIP]
> Use for helpful hints and best practices.

> [!IMPORTANT]
> Use for critical information the reader must not miss.

> [!WARNING]
> Use for behavior that could cause problems if ignored.

> [!CAUTION]
> Use for irreversible or destructive operations.

---

## Heading Attributes

Place a `heading` attribute block at the end of a heading to add a background color.
All other heading behavior is unchanged.

Available colors: `gray` `blue` `green` `yellow` `red` `purple`

## gray {heading color=gray}
## blue {heading color=blue}
## green {heading color=green}
## yellow {heading color=yellow}
## red {heading color=red}
## purple {heading color=purple}

### Nested Heading Sections

The `nest` attribute shifts a heading section right by one visual level without changing the Markdown heading level.
Child headings inherit the parent section's nesting level. If a child heading also has `nest`, it adds one more level.

## no nest (default) {heading color=blue}

Body text appears beneath the heading with no extra visual nesting.

## nest {heading color=blue nest}

Body text shifts right by one visual level until the next `##` or `#` heading.

### child nest {heading color=green nest}

This child section inherits the parent nesting and adds one more visual level.

---

## Inline Directives

### Badge

`badge` renders a colored inline label.  
No space between the closing backtick and `{`.

`Stable`{badge color=green} `Beta`{badge color=yellow} `Deprecated`{badge color=red} `Removed`{badge color=gray} `New`{badge color=blue} `Internal`{badge color=purple}

Available colors: `gray` `blue` `green` `yellow` `red` `purple`

### Keyboard Shortcut

`kbd` renders text as a key combination, like `Ctrl+Shift+P`{kbd}.

### Small Text

`small` renders text smaller.  

`Small text`{small} — normal text

---

## Footnotes

Footnotes are not Gloss directives. They can be used the same way they are used on GitHub.com.

This sample uses a GitHub-style footnote[^gfm].

[^gfm]: GitHub Flavored Markdown — the Markdown dialect used on GitHub.

---

## Code Blocks

### Syntax Highlighting

Code fences accept a language name or file extension for syntax highlighting.

```ts
function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

### File Label

Add `filename="path"` after the language identifier to display a file path label above the block.

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

## Math

Math is not a Gloss directive. It can be used the same way it is used on GitHub.com.

$$
E = mc^2
$$

$$
\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}
$$

The mass–energy equivalence $E = mc^2$ and Euler's identity $e^{i\pi} + 1 = 0$.

---

## Details

`details` creates a collapsible section.  
Without `open` the section is collapsed by default.  
The `color` attribute adds color.

| Attribute | Values | Default |
| --------- | ------ | ------- |
| `title` | string | `Details` |
| `open` | boolean flag | `false` |
| `color` | `gray` `blue` `green` `yellow` `red` `purple` | none |

```details title="Collapsed by default"
This content is hidden until clicked.
```

```details title="Expanded by default" open
The `open` flag (shorthand for `open=true`) expands the section on load.
```

```details title="With color" color=blue
The `color` attribute adds color.
```

---

## Card

`card` renders a bordered content block with a title.  
Use `title` for a header, `href` to add a link, and `color` to add color.

| Attribute | Values | Default |
| --------- | ------ | ------- |
| `title` | string | none |
| `href` | safe link target | none |
| `color` | `gray` `blue` `green` `yellow` `red` `purple` | none |

```card title="Basic card"
A simple content block with a title bar.
```

```card title="Colored card" color=blue
Use `color` to add color to the card.
```

```card title="Linked card" href="https://example.com" color=green
Add `href` to make the entire card a link.
```

---

## Tabs

`tabs` creates a tabbed container.  
Each `tab` child can set a `title`; omitted titles default to `Tab 1`, `Tab 2`, and so on.  
The `color` attribute sets the active-tab indicator color.

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `tabs` | `color` | `gray` `blue` `green` `yellow` `red` `purple` | none |
| `tab` | `title` | string | `Tab 1`, `Tab 2`, … |
| `tab` | `color` | same as above | inherited from `tabs` |

````tabs
```tab title="Tab A"
Content for the first tab. Markdown is supported here.
```

```tab title="Tab B"
Content for the second tab.
```

```tab title="Tab C"
Content for the third tab.
```
````

When a tab's body contains a code block, that inner fence makes each surrounding fence grow by one (`tabs`: 5, `tab`: 4, code block: 3):

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

## Steps

`steps` renders a numbered step sequence.  
Each `step` has body text and can set a `title`; omitted titles default to `Step 1`, `Step 2`, and so on.

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `steps` | `color` | `gray` `blue` `green` `yellow` `red` `purple` | none |
| `step` | `title` | string | `Step 1`, `Step 2`, … |
| `step` | `color` | same as above | inherited from `steps` |

````steps
```step title="Install"
Run `go install github.com/aXisho/glmo@latest` to install the renderer.
```

```step title="Open a file"
Run `glmo README.gloss.md` to open the file in the browser.
```

```step title="Edit and save"
The browser reloads automatically on every save.
```
````

---

## Grid

`grid` lays out `cell` children in a CSS grid.  
Use `cols` to control the column count and `border` to toggle cell dividers.

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `grid` | `cols` | positive integer | number of cells |
| `grid` | `border` | `solid` `none` | `solid` |
| `grid` | `color` | `gray` `blue` `green` `yellow` `red` `purple` | none |
| `cell` | `title` | string | none |
| `cell` | `color` | same as above | inherited from `grid` |
| `cell` | `border` | `solid` `none` | inherited from `grid` |

### With borders (default)

````grid cols=3
```cell
**Overview**

A brief introduction to the topic being documented here.
```

```cell
**Features**

Key capabilities and what makes it worth exploring further.
```

```cell
**Resources**

Links, references, and additional reading material.
```
````

### Without borders (`border=none`)

````grid cols=3 border=none
```cell color=blue
**Design**

Thoughtful defaults that look good out of the box.
```

```cell color=green
**Workflow**

Fits naturally into existing writing and review flows.
```

```cell color=purple
**Portability**

Readable everywhere, enhanced where supported.
```
````

### Columns default to the cell count (no `cols`)

`````grid border=none
````cell
**Left column**

Lists, code blocks, and other elements all work as expected.

- Item one
- Item two

```ts
const x = 42;
```
````

````cell
**Right column**

With no `cols`, the grid uses one column per cell — here, two. Heights match row by row.

> [!TIP]
> GitHub Alerts can be nested inside `cell` blocks.
````
`````

## Attribute Syntax

All directive attributes follow a consistent key–value format.

````grid cols=3 border=none
```cell color=blue
**Unquoted value**

`key=value`

For single-word values with no spaces.
```

```cell color=green
**Quoted value**

`key="long value"`

Required when the value contains spaces.
```

```cell color=purple
**Boolean flag**

`open` or `open=true`

Bare key is shorthand for `true`.
```
````

Escape sequences inside quoted strings: `\"` (literal quote), `\\` (backslash).
