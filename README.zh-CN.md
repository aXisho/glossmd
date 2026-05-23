# Gloss Markdown

[English](./README.md) | [日本語](./README.ja.md) | [简体中文](./README.zh-CN.md)

**面向在 GitHub 上管理 Markdown 的人，Human / AI 友好的 GFM 扩展格式**

Gloss Markdown 是一种叠加在 GitHub Flavored Markdown (GFM) 之上的易用、可读性高的扩展格式。它主要面向在 GitHub 上管理 AI 生成文档的人。源码仍然可以在标准 Markdown 工具中阅读；使用 Gloss Markdown 专用工具时，则可以升级为更丰富、更易读的文档。

Markdown 对人类和 LLM 都很友好。HTML 很强大，但对于 README 和 Spec 来说过于沉重，也不适合在 GitHub 上管理的文档。Gloss Markdown 的表达力位于 GFM 和 HTML 之间。它为 GitHub 上管理的文档提供一组小而可预测、刚好够用的 directive。

~~~md
> [!WARNING]
> **生产数据** — 此迁移会修改已保存的记录。

````tabs
```tab title="CLI"
glmo docs/ --watch
```

```tab title="GitHub"
安装 GlossView for GitHub Chrome extension，然后打开任意 `.gloss.md` 文件。
```
````

此 API 为 `Stable`{badge color=green}。按 `Ctrl + S`{kbd} 保存。
~~~

在 GitHub 上仍可阅读的记法。更好的可读性。更少的 token。

## 为什么选择 Gloss Markdown

- **丰富的文档 UI** - 提供 details、tabs、cards、badges 等结构化文档元素。
- **无需专用 viewer 也可阅读** - `.gloss.md` 文件在 GitHub、VS Code、terminal 和 code review 工具中仍然可以作为普通 Markdown 阅读。
- **小型词汇表** - 只有 details、card、tabs、steps、grid、table of contents、badge、keyboard key 和少量 inline helper，规格简单。
- **AI 输出更一致** - 格式选择更少，可以减少 LLM 输出的波动，生成更整齐、更统一的文档。
- **更少 token 消耗** - directive 是紧凑的 Markdown pattern，不是冗长的 HTML 或 JSX component。
- **安全的渲染模型** - Gloss Markdown 是 data-only，因此可以更安全地渲染 AI 输出。
- **专用 renderer** - 面向 GitHub 提供 Chrome extension `GlossView for GitHub`；本地则提供 [k1LoW/mo](https://github.com/k1LoW/mo) fork 的 `glmo`。

## 核心理念

AI 生成文档通常会偏向以下两种形式之一：

| Approach | Problem |
| -------- | ------- |
| Plain Markdown | 容易编辑，但表达力有限，LLM 生成的长文档可读性和可扫描性较低。 |
| HTML | 表现力强，但对多数文档来说过于沉重。它无法在 GitHub 上自然 preview，而且自由度太高，难以维持统一格式。 |

Gloss Markdown 在 GitHub Flavored Markdown 之上添加一个 inline gloss layer。这个 layer 足够轻量，在 code review 和 fallback rendering 中不会成为噪音；同时又足够结构化，可以让 Gloss Markdown renderer 将其转换为 tabs、grids、badges 等适度丰富、易读的 documentation UI。

> [!NOTE]
> Gloss Markdown 不是 GitHub Flavored Markdown 的替代品。
> 它为 GitHub Flavored Markdown 添加了一组小而安全的词汇。

## 文件扩展名

Gloss Markdown 文件使用 `.gloss.md` 扩展名。即使没有专用的 Gloss Markdown viewer，也可以在 GitHub、VS Code、terminal 和 code review 工具中作为普通 Markdown 阅读。

## 快速开始

### GitHub 兼容记法

GitHub Alert、脚注和数学公式不是 Gloss directive。请按 GitHub.com 上的写法直接使用。

```md
> [!TIP]
> **Component API** — 用于编写 UI 组件的 props、示例和迁移说明。
```

### 块级 directive

Block directive 使用 fenced code block。没有 body 的 directive（例如 `toc`）使用同样的形式，但 body 为空。

~~~md
```details title="安装"
npm install @acme/ui
```
~~~

标准 block directive:

```text
details  card  toc
```

### 容器 directive

tabs 和 steps 这样的嵌套结构使用更长的 outer fence 和普通 inner fence 表达。

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

标准 container pair:

| Container | Child |
| --------- | ----- |
| `tabs` | `tab` |
| `steps` | `step` |
| `grid` | `cell` |

### 行内 directive

Inline directive 写作 inline code span 后跟 attribute block。

```md
Button API 是 `Stable`{badge color=green}。
运行 `npm run dev`{kbd} 启动本地服务器。
```

标准 inline directive:

```text
badge  small  kbd
```

脚注和数学公式也可以按 GitHub.com 上的写法直接使用。

### 标题属性

标题属性为标题添加背景色。

```md
## Button {heading color=blue}
### Props {heading color=green}
```

## 工具

| 工具 | 用途 |
| ---- | ------- |
| [glmo](https://github.com/aXisho/glmo) | 面向 `.md`, `.gloss.md` 文件的本地 browser viewer。它明确是 [k1LoW/mo](https://github.com/k1LoW/mo) 的 fork，并扩展了 Gloss Markdown rendering。 |
| [GlossView for GitHub](https://github.com/aXisho/glossview-for-github) | 在 GitHub file page 上渲染 Gloss Markdown directive 的 Chrome extension。 |

## 许可证

[MIT License](./LICENSE)

Gloss Markdown 设计为叠加在 GFM / CommonMark 风格 Markdown 之上的扩展格式。上游 CommonMark 规范和 GFM 规范分别以 Creative Commons Attribution-ShareAlike 4.0 International 授权。详情请参阅 [Third-Party Notices](./THIRD_PARTY_NOTICES.md)。
