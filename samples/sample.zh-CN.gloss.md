# Gloss Markdown — 完整参考文档 {heading color=blue}

```toc title="目录" depth=3
```

本文档涵盖 Gloss Markdown directive 和 GitHub 兼容记法。`.gloss.md` 文件在 GitHub 上以普通 Markdown 形式渲染，在 Gloss 兼容渲染器中则呈现丰富的交互界面。

---

## GitHub Alert

GitHub Alert 不是 Gloss directive。请按 GitHub.com 上的写法直接使用。

> [!NOTE]
> 用于展示信息性内容。GitHub Alert 内部支持 **Markdown** 解析。

> [!TIP]
> 用于有用的提示和最佳实践。

> [!IMPORTANT]
> 用于读者不能忽视的关键信息。

> [!WARNING]
> 用于忽略后可能导致问题的行为说明。

> [!CAUTION]
> 用于不可逆或破坏性操作的警示。

---

## 标题属性

在标题行末尾放置 `heading` 属性块可为其添加背景色。
标题的其他行为均不受影响。

可用颜色: `gray` `blue` `green` `yellow` `red` `purple`

## gray {heading color=gray}
## blue {heading color=blue}
## green {heading color=green}
## yellow {heading color=yellow}
## red {heading color=red}
## purple {heading color=purple}

### 嵌套标题 section

`nest` 属性可在不改变 Markdown 标题级别的情况下，将标题 section 在视觉上加深一层。
下级标题继承父 section 的 nesting level。如果下级标题也带有 `nest`，则再增加一层。

## 无 nest（默认） {heading color=blue}

正文在没有额外视觉嵌套的情况下显示在标题下方。

## nest {heading color=blue nest}

正文会在视觉上加深一层，直到下一个 `##` 或 `#` 标题。

### child nest {heading color=green nest}

这个子 section 会继承父级 nesting level，并再增加一层。

---

## 内联指令

### 徽章

`badge` 渲染彩色内联标签。  
反引号与 `{` 之间不能有空格。

`Stable`{badge color=green} `Beta`{badge color=yellow} `Deprecated`{badge color=red} `Removed`{badge color=gray} `New`{badge color=blue} `Internal`{badge color=purple}

可用颜色: `gray` `blue` `green` `yellow` `red` `purple`

### 键盘快捷键

`kbd` 将文本渲染为 `Ctrl+Shift+P`{kbd} 这样的按键组合形式。

### 小字

`small` 缩小文字。  

`小字`{small} — 普通文字

---

## 脚注

脚注不是 Gloss directive。请按 GitHub.com 上的写法直接使用。

此示例使用 GitHub 风格的脚注[^gfm]。

[^gfm]: GitHub Flavored Markdown——GitHub 使用的 Markdown 方言。

---

## 代码块

### 语法高亮

代码围栏接受语言名称或文件扩展名以启用语法高亮。

```ts
function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

### 文件标签

在语言标识符后附加 `filename="path"` 可在代码块上方显示文件路径标签。

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

## 数学公式

数学公式不是 Gloss directive。请按 GitHub.com 上的写法直接使用。

$$
E = mc^2
$$

$$
\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}
$$

质能等价 $E = mc^2$ 与欧拉恒等式 $e^{i\pi} + 1 = 0$。

---

## 详情（折叠区块）

`details` 创建可折叠区块。  
不指定 `open` 时默认折叠。  
`color` 属性为区块添加颜色。

| 属性 | 值 | 默认 |
| ---- | -- | ---- |
| `title` | 字符串 | `Details` |
| `open` | 布尔标志 | `false` |
| `color` | `gray` `blue` `green` `yellow` `red` `purple` | 无 |

```details title="默认折叠"
点击前此内容处于隐藏状态。
```

```details title="默认展开" open
`open` 标志（`open=true` 的简写）使区块在页面加载时展开。
```

```details title="带颜色" color=blue
`color` 属性为区块添加颜色。
```

---

## 卡片

`card` 渲染带标题的带边框内容块。  
使用 `title` 添加标题栏，`href` 设置链接，`color` 添加颜色。

| 属性 | 值 | 默认 |
| ---- | -- | ---- |
| `title` | 字符串 | 无 |
| `href` | 安全链接目标 | 无 |
| `color` | `gray` `blue` `green` `yellow` `red` `purple` | 无 |

```card title="基础卡片"
带标题栏的简单内容块。
```

```card title="彩色卡片" color=blue
使用 `color` 为卡片添加颜色。
```

```card title="链接卡片" href="https://example.com" color=green
添加 `href` 可使整张卡片变为链接。
```

---

## 标签页

`tabs` 创建标签页容器。  
每个 `tab` 子元素可以指定 `title`；省略时使用 `Tab 1`, `Tab 2`, ...。  
`color` 属性设置活动标签的指示颜色。

| 指令 | 属性 | 值 | 默认 |
| ---- | ---- | -- | ---- |
| `tabs` | `color` | `gray` `blue` `green` `yellow` `red` `purple` | 无 |
| `tab` | `title` | 字符串 | `Tab 1`, `Tab 2`, … |
| `tab` | `color` | 同上 | 继承自 `tabs` |

````tabs
```tab title="标签 A"
第一个标签的内容，支持 Markdown 语法。
```

```tab title="标签 B"
第二个标签的内容。
```

```tab title="标签 C"
第三个标签的内容。
```
````

当标签页正文包含代码块时，内层 fence 会使外面每一层 fence 各加一个反引号（`tabs`：5，`tab`：4，code block：3）：

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

## 步骤

`steps` 渲染有序步骤序列。  
每个 `step` 有正文内容，并且可以指定 `title`；省略时使用 `Step 1`, `Step 2`, ...。

| 指令 | 属性 | 值 | 默认 |
| ---- | ---- | -- | ---- |
| `steps` | `color` | `gray` `blue` `green` `yellow` `red` `purple` | 无 |
| `step` | `title` | 字符串 | `Step 1`, `Step 2`, … |
| `step` | `color` | 同上 | 继承自 `steps` |

````steps
```step title="安装"
运行 `go install github.com/aXisho/glmo@latest` 安装渲染器。
```

```step title="打开文件"
运行 `glmo README.gloss.md` 在浏览器中打开文件。
```

```step title="编辑并保存"
每次保存后浏览器将自动重新加载。
```
````

---

## 网格布局

`grid` 将 `cell` 子元素排列为 CSS 网格。  
`cols` 控制列数，`border` 切换单元格分隔线。

| 指令 | 属性 | 值 | 默认 |
| ---- | ---- | -- | ---- |
| `grid` | `cols` | 正整数 | 单元格数量 |
| `grid` | `border` | `solid` `none` | `solid` |
| `grid` | `color` | `gray` `blue` `green` `yellow` `red` `purple` | 无 |
| `cell` | `title` | 字符串 | 无 |
| `cell` | `color` | 同上 | 继承自 `grid` |
| `cell` | `border` | `solid` `none` | 继承自 `grid` |

### 带边框（默认）

````grid cols=3
```cell
**概述**

关于此主题的简短介绍文字。
```

```cell
**特性**

主要功能及值得深入了解的亮点。
```

```cell
**参考资料**

相关链接、文献及延伸阅读材料。
```
````

### 无边框（`border=none`）

````grid cols=3 border=none
```cell color=blue
**设计**

开箱即用，默认效果美观得体。
```

```cell color=green
**工作流**

自然融入现有的写作与审阅流程。
```

```cell color=purple
**可移植性**

随处可读，在支持的环境中呈现更丰富的效果。
```
````

### 列数默认等于 cell 数量（不指定 `cols`）

`````grid border=none
````cell
**左列**

列表、代码块等各类内容均可正常使用。

- 项目一
- 项目二

```ts
const x = 42;
```
````

````cell
**右列**

不指定 `cols` 时，grid 会按 cell 数量生成列（这里是两列）。行高自动对齐。

> [!TIP]
> 可在 `cell` 内嵌套 GitHub Alert。
````
`````

## 属性语法

所有指令的属性遵循统一的键值格式。

````grid cols=3 border=none
```cell color=blue
**不带引号的值**

`key=value`

适用于不含空格的单词值。
```

```cell color=green
**带引号的值**

`key="long value"`

值中含有空格时必须使用引号。
```

```cell color=purple
**布尔标志**

`open` 或 `open=true`

单独的键名是 `true` 的简写。
```
````

引号内的转义序列: `\"` (字面量引号)、`\\` (反斜杠)。
