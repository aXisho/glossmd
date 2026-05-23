# Gloss Markdown — 记法指南

[English](./syntax.md) | [日本語](./syntax.ja.md) | [简体中文](./syntax.zh-CN.md)

> [!NOTE]
> **目录**
> 1. 文件扩展名
> 2. 块级 directive（fence 形式）
> 3. 嵌套容器 directive
> 4. 行内 directive（`` `text`{name attrs} `` 形式）
> 5. 标题属性
> 6. 属性语法
> 7. 标识符规则
> 8. 语法高亮
> 9. GitHub Markdown passthrough

Gloss Markdown 以 GitHub Flavored Markdown (GFM) 为基础，并在其上添加轻量的 **inline gloss layer**。本指南说明如何书写这些记法。每一种 directive 形式都经过选择，使源码在普通 Markdown viewer（GitHub、VS Code preview 等）中仍然有意义，同时 Gloss-aware renderer 可以生成更丰富的输出。

GitHub.com 特有的 Markdown 功能（例如 Alert、脚注、数学公式、task list、table、strikethrough、autolink、普通 code highlighting）不作为 Gloss Markdown 的自定义记法定义。如果输入中包含这些内容，Gloss Markdown 不会把它们解释为 Gloss directive，而是交由 Markdown renderer 原样处理。

> [!NOTE]
> **关于本规范中的示例**
> 本文中用于*展示* Gloss Markdown 源码的代码块使用 tildes（`~~~md`）包裹，以便保留内部的反引号 fence。在实际 `.gloss.md` 文件中，正常使用反引号 fence（`` ``` `` 或 `` ```` ``）。

---

## 1. 文件扩展名

Gloss Markdown 文件使用 `.gloss.md` 扩展名。其记法叠加在仍然有效的 GFM Markdown 之上，因此 github.com 等普通 Markdown renderer 仍然可以把源码作为普通 Markdown 显示。

没有专用 viewer 时，Gloss directive 主要会显示为普通 code block 或文本。

---

## 2. 块级 directive（fence 形式）

Block directive 写作 **fenced code block**，其 info string 携带 directive 名称和 attribute。

~~~md
```details color=blue title="补充"
在这里写正文。**Markdown** 会被解析。
```
~~~

- Opening fence：info string 中包含 `NAME` 和可选的 `ATTRS`。directive 名称在前，后面跟 attribute。
- **fence 长度：** Gloss Markdown 遵循 CommonMark/GFM 的 fence 规则。开始 fence 使用至少三个相同字符（反引号或波浪线），结束 fence 使用相同字符，且长度不少于开始 fence。只有当 body 中包含同种 fence 时才需要让外层 fence 比内层更长；没有嵌套 fence 时，三个字符就够了。
- Fence 之间的 body 会被 Gloss-aware renderer 作为 Markdown 解析。普通 Markdown viewer 会将 body 显示为 code block，这仍然能在视觉上传达 grouping 和 scope。

没有 body 的 directive（目前只有 `toc`）使用同样的形式，但 body 为空 —— opening fence 和 closing fence 之间没有任何行。

~~~md
```toc title="目录" depth=3
```
~~~

使用这种形式的标准 directive：

```text
details  card  toc
```

下列 fence info 名称保留给 Gloss Markdown directive 使用。当 fence info string 的第一个以空白分隔的 token 是这些名称之一时，Gloss-aware renderer 会将该 fence 作为 directive 处理，而不是普通 code block。

```text
details  card  toc  tabs  tab  steps  step  grid  cell
```

**可用 attribute**

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `details` | `title` | string | `Details` |
| `details` | `open` | boolean（§6.3） | `false` |
| `details` | `color` | color 值（§6.4） | none |
| `card` | `title` | string | none |
| `card` | `href` | 链接目标（§6.5） | none |
| `card` | `color` | color 值（§6.4） | none |
| `toc` | `title` | string | none |
| `toc` | `depth` | 1–6（heading level） | `3` |

> [!NOTE]
> **为什么使用 fenced code block？**
> 普通 Markdown reader 会看到一个带边界的 code block，因此 directive 的开始、结束和 body 仍然清晰可见。

---

## 3. 嵌套容器 directive

包含 typed children 的 directive（`tabs` 包含 `tab`，`steps` 包含 `step`，`grid` 包含 `cell`）将一组 fence 嵌套在另一组之内。适用与 §2 相同的 fence 长度规则：使用同一种 fence 字符嵌套时，outer fence 必须比 inner fence 更长。

因此当 child 不含内部 fence 时，每个 child 三个反引号、container 四个反引号即为最小值。如果某个 child 的 body 包含使用同种 fence 字符的 code block，则该 code block 保持三个，child fence 增长到四个，container fence 增长到五个。

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

标准 container/child pairs：

| Container | Child |
| --------- | ----- |
| `tabs` | `tab` |
| `steps` | `step` |
| `grid` | `cell` |

Child directive（`tab`, `step`, `cell`）应写在对应的 container 内部。

**可用 attribute**

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `tabs` | `color` | color 值（§6.4） | none |
| `tab` | `title` | string | 自动编号（`Tab 1`, `Tab 2`, …） |
| `tab` | `color` | color 值（§6.4） | 从父级 `tabs` 继承 |
| `steps` | `color` | color 值（§6.4） | none |
| `step` | `title` | string | 自动编号（`Step 1`, `Step 2`, …） |
| `step` | `color` | color 值（§6.4） | 从父级 `steps` 继承 |
| `grid` | `cols` | positive integer | `cell` children 的数量 |
| `grid` | `color` | color 值（§6.4） | none |
| `grid` | `border` | `solid`, `none` | `solid` |
| `cell` | `title` | string | none |
| `cell` | `color` | color 值（§6.4） | 从父级 `grid` 继承 |
| `cell` | `border` | `solid`, `none` | 从父级 `grid` 继承 |

省略 `title` 时，renderer 会按出现顺序自动填充（`Tab 1`, `Step 2`, …）。

---

## 4. 行内 directive（`` `text`{name attrs} `` 形式）

Inline directive 用来装饰句子中的短文本。把要显示的文字写在反引号中，然后在后面紧跟 `{directive attributes}`。

~~~md
此 API 为 `Stable`{badge color=green}。
按 `Ctrl + S`{kbd} 保存。
更新: `2025-01-01`{small}。
~~~

`{ … }` 中的第一个词是 directive 名称。后面的内容是 attribute，例如 `color=green`。

反引号和 `{` 要连在一起写：

```md
`Stable`{badge color=green}
```

不要在中间加空格：

```md
`Stable` {badge color=green}
```

Inline directive 必须在同一行内闭合。directive text 不能包含反引号。如果只写反引号而没有 `{...}`，它就是普通 inline code。在普通 Markdown viewer 中，会显示为 inline code 后面紧跟字面量 `{...}`。

标准 inline directive：

```text
badge  small  kbd
```

**可用 attribute**

| Directive | Attribute | Values | Default |
| --------- | --------- | ------ | ------- |
| `badge` | `color` | color 值（§6.4） | none |
| `small` | none | - | - |
| `kbd` | none | - | - |

---

## 5. 标题属性

标题属性写在 ATX 标题行的末尾。ATX 标题是 `# 标题` 这种形式。Setext 标题（`标题` 的下一行写 `---` 或 `===` 的形式）不支持标题属性。

~~~md
## Button {heading color=blue}
### Props {heading color=green nest}
~~~

不要与末尾 closing `#` sequence 形式一起使用。也就是说，写 `## Button {heading color=blue}`，不要写 `## Button ## {heading color=blue}`。

只有 `{heading ATTRS}` 形式会被识别为 Gloss Markdown。省略 `heading` 的属性块（例如 `{color=blue}`）会被当作普通标题文本处理。

属性块作用于整个标题。在普通 Markdown renderer 中，它会显示为标题末尾的文本，而标题正文自身仍保持普通 Markdown 文本。由于标准 Markdown（包括 GitHub）不支持标题属性语法，因此在没有 Gloss renderer 的情况下，`{heading ...}` 字符串会原样保留在标题末尾。

`nest` attribute 会将该标题的 section 在视觉上加深一层。

- **效果：** 不改变 Markdown heading level，将该标题及其正文在视觉上缩进一层。
- **作用范围：** 从该标题开始，直到下一个相同或更高 Markdown heading level 的标题之前。
- **继承：** 下级标题继承父级的视觉 nesting level；如果下级标题也带有 `nest`，则再增加一层。

**可用 attribute**

| Attribute | Values | Default |
| --------- | ------ | ------- |
| `color` | color 值（§6.4） | none |
| `nest` | boolean（§6.3） | `false` |

---

## 6. 属性语法

Attribute grammar 被所有 directive 形式（§2–§4）和标题属性（§5）共享。

### 6.1 键值对

```text
key=value
key="value with spaces"
```

- 未加引号的值一直读取到下一个 whitespace 或 attribute list 末尾。
- 加引号的值由 `"` 分隔，必须在同一行关闭。
- 在 inline directive 和标题属性的 `{...}` 内，attribute value 不能包含 `}`。在 fence 形式的 directive 中，`}` 会作为 attribute value 中的普通字符处理。
- 未知 attribute 会被忽略。
- 无效值会按该 attribute 的默认值处理。如果该 attribute 没有默认值，则按未指定该 attribute 处理。

### 6.2 转义序列（quoted value 内）

| Sequence | Meaning |
| -------- | ------- |
| `\"` | literal double-quote |
| `\\` | literal backslash |

### 6.3 布尔值

Boolean attribute 的值只能是 `true` 或 `false`。请使用小写。`True`、`0`、`yes` 等都是无效值。

只有 boolean attribute 可以省略 `=value`。没有 `=value` 的 boolean attribute key 被视为 `key="true"`。

如果非 boolean attribute 省略 `=value`，该 attribute 指定无效，并按未指定该 attribute 处理。

~~~md
```details title="详情" open
页面加载时此区块默认展开显示。
```
~~~

### 6.4 color 值

所有 `color` attribute 使用同一个调色板。

```text
gray  blue  green  yellow  red  purple
```

实际渲染颜色由各 renderer 的实现决定。

### 6.5 链接目标（`href`）

`href` attribute 只接受下列链接目标形式：

- `http://...` 和 `https://...` URL
- `#anchor` 这样的 fragment link
- `/docs/guide.md` 这样的 root-relative path
- `./guide.md` 和 `../guide.md` 这样的 dot-relative path
- 不带 URI scheme 的裸相对路径，例如 `foo.md` 和 `docs/guide.md`

下列形式会作为无效值被**拒绝**：

- `javascript:`、`data:`、`vbscript:` 等危险 scheme
- `//example.com` 这样的 protocol-relative URL
- 上面没有列出的其他 URI scheme

与任何无效值（§6.1）一样，被拒绝的 `href` 会按未指定处理，因此不会生成链接。

---

## 7. 标识符规则

| Identifier | Allowed characters | Example |
| ---------- | ------------------ | ------- |
| Directive name | `[A-Za-z][A-Za-z0-9-]*` | `tabs`, `details` |
| Attribute name | `[A-Za-z][A-Za-z0-9-]*` | `color`, `title`, `border` |

名称是 **case-insensitive**。`Details`、`details`、`DETAILS` 都表示同一个 directive。

Gloss Markdown tools 会在渲染前把 directive 名称和 attribute 名称规范化为 lowercase。

按照惯例，Gloss 自定义 directive（`toc`、`details`、`badge` 等）使用**小写**。GitHub native Markdown 功能遵循 GitHub 自身的记法。

---

## 8. 语法高亮

Gloss Markdown 不提供单独的 syntax highlighting directive。和普通 Markdown 一样，在 code fence 的 info string 中写语言名。

~~~md
```ts
type User = {
  id: string;
  name: string;
};
```
~~~

具体哪些语言名会被高亮，取决于使用的 renderer。`ts`、`typescript`、`js`、`javascript`、`json`、`bash` 等常见名称通常可以工作；不支持的语言名会退回为普通 code block。

### 8.1 文件名标签（Gloss 扩展）

**在语言名后面**加 `filename="path"`，Gloss-aware renderer 可以在 block 上方显示文件名标签。和任何带高亮的 code block 一样，语言名必须写在 info string 的开头，`filename` 跟在其后。这不是 GFM 标准功能，而是 Gloss Markdown 的 code block 扩展。普通 Markdown renderer 会把它当作 fence info string 的一部分处理。

~~~md
```ts filename="src/types.ts"
type User = { id: string; name: string };
```
~~~

如果 fence info string 的第一个 whitespace 分隔 token 与已注册的 directive 名称完全一致，该 fence 会被当作 Gloss directive，而不是 syntax-highlighted code block。例如，`details` 是 directive，但 `details-foo` 和 `details.txt` 不是 `details` directive。

~~~md
```details title="补充"
这是一个 `details` directive。
```
~~~

如果要在 directive body 中放入使用同种 fence 字符、带 syntax highlighting 的 code block，请让外层 directive fence 比内部 code fence 更长 —— 与 §2 相同的 fence 长度规则。

~~~~md
````details title="TypeScript 示例"
```ts
const label = "Workspace name";
```
````
~~~~

---

## 9. GitHub Markdown passthrough

Gloss Markdown 不重新定义 GitHub native Markdown 记法。Alert、脚注、数学公式、task list、table、strikethrough、autolink 和普通 code highlighting 都是 passthrough Markdown。它们不是 Gloss directive；如果输入中包含这些内容，会交由 Markdown renderer 原样处理。

支持情况和详细规则以 GitHub Docs 为准。
