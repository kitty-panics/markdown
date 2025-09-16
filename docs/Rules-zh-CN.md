# 规则

该文档包含所有规则的描述、规则的检查内容以及违反规则的文档示例和示例的更正版本。

## `MD001` - 标题级别每次只能增加一级

标签： `headings`

别名： `heading-increment`

当您跳过 Markdown 文档中的标题级别时，会触发此规则，例如：

```markdown
# Heading 1

### Heading 3

We skipped out a 2nd level heading in this document
```

使用多个标题级别时，嵌套标题每次只能增加一级：

```markdown
# Heading 1

## Heading 2

### Heading 3

#### Heading 4

## Another Heading 2

### Another Heading 3
```

理由：标题代表文档的结构，可能会造成混淆 跳过时 - 尤其是在无障碍场景下。更多信息： [https://www.w3.org/WAI/tutorials/page-structure/headings/](https://www.w3.org/WAI/tutorials/page-structure/headings/) 。

## `MD003` - 标题样式

标签： `headings`

别名： `heading-style`

参数：

*   `style` ：标题样式（ `string` ，默认 `consistent` ，值 `atx` / `atx_closed` / `consistent` / `setext` / `setext_with_atx` / `setext_with_atx_closed` ）

当在同一文档中使用不同的标题样式时，会触发此规则：

```markdown
# ATX style H1

## Closed ATX style H2 ##

Setext style H1
===============
```

要解决此问题，请在整个文档中使用一致的标题样式：

```markdown
# ATX style H1

## ATX style H2
```

`setext_with_atx` 和 `setext_with_atx_closed` 设置允许在具有 setext 样式标题的文档中使用 3 级或更多级的 ATX 样式标题（仅支持 1 级和 2 级标题）：

```markdown
Setext style H1
===============

Setext style H2
---------------

### ATX style H3
```

注意：配置的标题样式可以是需要的特定样式（ `atx` ， setext 可以使用多种标题样式 `setext` 例如， `atx_closed` 、 `setext_with_atx` 、 `setext_with_atx_closed` ）来实现，也可以通过 `consistent` 要求所有标题样式与第一个标题样式匹配。

注意：在文本行正下方放置水平线可以触发此规则，方法是将该文本转换为 2 级 setext 样式标题：

```markdown
A line of text followed by a horizontal rule becomes a heading
---
```

理由：一致的格式使得文档更容易理解。

## `MD004` - 无序列表样式

标签： `bullet` , `ul`

别名： `ul-style`

参数：

*   `style` ：列表样式（ `string` ，默认 `consistent` ，值星 `asterisk` / `consistent` / `dash` / `plus` / `sublist` )

可修复：某些违规行为可以通过工具修复

当文档中用于无序列表项的符号与配置的无序列表样式不匹配时，将触发此规则：

```markdown
* Item 1
+ Item 2
- Item 3
```

要解决此问题，请在整个文档中使用已配置的列表项样式：

```markdown
* Item 1
* Item 2
* Item 3
```

配置的列表样式可以确保所有列表样式都是特定符号（ `asterisk` 、 `plus` 、 `dash` ），确保每个子列表都有与其父列表（ `sublist` ）不同的一致符号，或者确保所有列表样式与第一个列表样式匹配（ `consistent` ）。

例如，以下内容对于 `sublist` 样式有效，因为最外层缩进使用星号，中间缩进使用加号，最内层缩进使用破折号：

```markdown
* Item 1
  + Item 2
    - Item 3
  + Item 4
* Item 4
  + Item 5
```

理由：一致的格式使得文档更容易理解。

## `MD005` - 同一级别的列表项缩进不一致

标签： `bullet` 、 `indentation` 、 `ul`

别名： `list-indent`

可修复：某些违规行为可以通过工具修复

当列表项被解析为处于同一级别，但没有相同的缩进时，会触发此规则：

```markdown
* Item 1
  * Nested Item 1
  * Nested Item 2
   * A misaligned item
```

通常，此规则会因拼写错误而触发。请更正列表的缩进以修复此问题：

```markdown
* Item 1
  * Nested Item 1
  * Nested Item 2
  * Nested Item 3
```

按顺序排列的列表标记通常左对齐，以便所有项目都有相同的起始列：

```markdown
...
8. Item
9. Item
10. Item
11. Item
...
```

此规则还支持列表标记的右对齐，以便所有项目都有相同的结束列：

```markdown
...
 8. Item
 9. Item
10. Item
11. Item
...
```

理由：违反此规则可能会导致内容呈现不正确。

## `MD007` - 无序列表缩进

标签： `bullet` 、 `indentation` 、 `ul`

别名： `ul-indent`

参数：

*   `indent` ：缩进的空格（ `integer` ，默认为 `2` ）
*   `start_indent` ：第一级缩进的空格（当设置了 start\_indented 时）（ `integer` ，默认为 `2` ）
*   `start_indented` ：是否缩进列表的第一级（ `boolean` ，默认为 `false` ）

可修复：某些违规行为可以通过工具修复

当列表项未按配置的空格数（默认值：2）缩进时，将触发此规则。

例子：

```markdown
* List item
   * Nested list item indented by 3 spaces
```

更正后的示例：

```markdown
* List item
  * Nested list item indented by 2 spaces
```

注意：仅当子列表的父列表也都是无序的时，此规则才适用于子列表（否则，有序列表的额外缩进会干扰规则）。

`start_indented` 参数允许将列表的第一级按配置的空格数缩进，而不是从零开始 `start_indent` 参数允许列表的第一级以不同的数字缩进 空格数多于其余部分（未设置 `start_indented` 时将被忽略）。

原理：当列表标记后使用单个空格时，缩进两个空格可以使嵌套列表的内容与父列表内容的开头对齐。缩进四个空格与代码块的格式一致，并且更易于编辑器实现。此外，这可能会与其他需要四个空格缩进的 Markdown 解析器产生兼容性问题。更多信息： [Markdown 样式指南](https://cirosantilli.com/markdown-style-guide#indentation-of-content-inside-lists) 。

注意：有关兼容性信息，请参阅 [Prettier.md](Prettier.md) 。

## `MD009` 尾随空格

标签： `whitespace`

别名： `no-trailing-spaces`

参数：

*   `br_spaces` ：换行空格（ `integer` ，默认为 `2` ）
*   `list_item_empty_lines` ：允许列表项中有空行（ `boolean` ，默认为 `false` ）
*   `strict` ：包括不必要的中断（ `boolean` ，默认为 `false` ）

可修复：某些违规行为可以通过工具修复

任何以意外空格结尾的行都会触发此规则。要解决此问题，请移除行尾的空格。

注意：缩进和围栏代码块中允许使用尾随空格，因为某些语言需要它。

`br_spaces` 参数允许对此规则进行例外处理，即允许使用特定数量的尾随空格，通常用于插入显式换行符。默认值允许使用 2 个空格来表示硬换行符 (
元素）。

注意：必须将 `br_spaces` 设置为值 >= 2 才能使此参数生效。将 `br_spaces` 设置为 1 的行为与设置为 0 相同，即不允许任何尾随空格。

默认情况下，即使空格数量达到允许值，即使没有造成硬换行（例如，在段落末尾），此规则也不会触发。要报告此类情况，请将 `strict` 参数设置为 `true` 。

```markdown
Text text text
text[2 spaces]
```

列表项中的空行通常不需要使用空格缩进，但某些解析器需要这样做。请将 `list_item_empty_lines` 参数设置为 `true` 允许这样做（即使 `strict` 为 `true` ）：

```markdown
- list item text
  [2 spaces]
  list item text
```

理由：除了用于创建换行符外，尾随空格没有任何用途，也不会影响内容的呈现。

## `MD010` 硬标签

标签： `hard_tab` 、 `whitespace`

别名： `no-hard-tabs`

参数：

*   `code_blocks` ：包含代码块（ `boolean` ，默认 `true` ）
*   `ignore_code_languages` ：要忽略的围栏代码语言（ `string[]` ，默认 `[]` )
*   `spaces_per_tab` ：每个硬制表符的空格数（ `integer` ，默认为 `1` ）

可修复：某些违规行为可以通过工具修复

任何包含硬制表符（而非使用空格进行缩进）的行都会触发此规则。要解决此问题，请将所有硬制表符替换为空格。

例子：

```markdown
Some text

	* hard tab character used to indent the list item
```

更正后的示例：

```markdown
Some text

    * Spaces used to indent the list item instead
```

您可以选择为代码块和 spans 排除此规则。为此，请将 `code_blocks` 参数设置为 `false` 。由于 Markdown 工具对制表符的处理可能不一致（例如，使用 4 个空格还是 8 个空格），因此默认包含代码块和 spans。

扫描代码块时（例如，默认情况下，或者 `code_blocks` 为 `true` 时），可以将 `ignore_code_languages` 参数设置为应忽略的语言列表（例如，允许使用硬制表符，但并非强制要求）。这使得文档更容易包含需要硬制表符的语言的代码。

默认情况下，违反此规则的情况会通过将制表符替换为 1 个空格字符来修复。要使用其他数量的空格，请设置“ `spaces_per_tab` 参数设置为所需值。

原因：不同的编辑器对硬制表符的呈现往往不一致，而且比空格更难处理。

## `MD011` 反向链接语法

标签： `links`

别名： `no-reversed-links`

可修复：某些违规行为可以通过工具修复

当遇到看起来像链接的文本，但语法似乎被反转（ `[]` 和 `()` 被反转）时，会触发此规则：

```markdown
(Incorrect link syntax)[https://www.example.com/]
```

要修复此问题，请交换 `[]` 和 `()` ：

```markdown
[Correct link syntax](https://www.example.com/)
```

注意： [Markdown Extra](https://en.wikipedia.org/wiki/Markdown_Extra) 样式的脚注不会触发此规则：

```markdown
For (example)[^1]
```

原因：反向链接不会呈现为可用链接。

## `MD012` 多个连续的空白行

标签： `blank_lines` 、 `whitespace`

别名： `no-multiple-blanks`

参数：

*   `maximum` ：连续空行数（ `integer` ，默认为 `1` ）

可修复：某些违规行为可以通过工具修复

当文档中出现多个连续的空白行时，会触发此规则：

```markdown
Some text here


Some more text here
```

要修复此问题，请删除有问题的行：

```markdown
Some text here

Some more text here
```

注意：如果代码块内有多个连续的空行，则不会触发此规则。

注意： `maximum` 参数可用于配置连续空白行的最大数量。

理由：除了在代码块中，空行没有任何用处，也不会影响内容的呈现。

## `MD013` 线长度

标签： `line_length`

别名： `line-length`

参数：

*   `code_block_line_length` ：代码块的字符数（ `integer` ，默认为 `80` ）
*   `code_blocks` ：包含代码块（ `boolean` ，默认 `true` ）
*   `heading_line_length` ：标题的字符数（ `integer` ，默认值 `80` ）
*   `headings` ：包含标题（ `boolean` ，默认为 `true` ）
*   `line_length` ：字符数（ `integer` ，默认 `80` ）
*   `stern` ：Stern 长度检查（ `boolean` ，默认为 `false` ）
*   `strict` ：严格长度检查（ `boolean` ，默认为 `false` ）
*   `tables` ：包含表（ `boolean` ，默认为 `true` ）

当行长度超过配置的 `line_length` （默认值：80 个字符）时，会触发此规则。要解决此问题，请拆分行 分成多行。要设置不同的标题最大长度，请使用 `heading_line_length` 。要为代码块设置不同的最大长度，请使用 `code_block_line_length`

此规则会在没有超出配置行长度的空格时抛出异常。这样，您可以添加诸如长 URL 之类的内容，而无需强制在中间断开它们。要禁用此异常，请设置 `strict` 参数设置为 `true` ，当任何行过长时都会报告问题。如果要对过长的行发出警告（虽然可以修复），但允许没有空格的长行，请将 `stern` 参数设置为 `true` 。

例如（假设正常行为）：

```markdown
IF THIS LINE IS THE MAXIMUM LENGTH
This line is okay because there are-no-spaces-beyond-that-length
This line is a violation because there are spaces beyond that length
This-line-is-okay-because-there-are-no-spaces-anywhere-within
```

在 `strict` 模式下，上面最后三行都是违规的。在 `stern` 下 模式，上面中间两行都违规了，但是最后一行是可以的。

您可以选择针对代码块、表格或标题排除此规则。为此，请将 `code_blocks` 、 `tables` 或 `headings` 参数设置为 false。

代码块默认包含在此规则中，因为它通常是文档可读性的要求，并且暂时与代码规则兼容。然而，有些语言不适合使用短行。

具有链接/图像引用定义的行和仅具有链接/图像（可能使用（强）强调）的独立行（即不是段落的一部分）始终不受此规则的约束（即使在 `strict` 模式下），因为通常没有办法在不破坏 URL 的情况下拆分此类行。

原因：某些编辑器可能会难以处理过长的行。更多信息： [https://cirosantilli.com/markdown-style-guide#line-wrapping](https://cirosantilli.com/markdown-style-guide#line-wrapping) 。

## `MD014` - 在命令前使用美元符号而不显示输出

标签： `code`

别名： `commands-show-output`

可修复：某些违规行为可以通过工具修复

当代码块中显示需要输入的 shell 命令，并且*所有* shell 命令前面都有美元符号 ($) 时，就会触发此规则：

```markdown
$ ls
$ cat foo
$ less bar
```

在这种情况下，美元符号是不必要的，不应包括在内：

```markdown
ls
cat foo
less bar
```

显示以美元符号开头的命令的输出不会触发此规则：

```markdown
$ ls
foo bar
$ cat foo
Hello world
$ cat bar
baz
```

因为有些命令不产生输出，所以如果*某些命令* 命令没有输出：

```markdown
$ mkdir test
mkdir: created directory 'test'
$ ls test
```

理由：如果美元符号 当不需要时，可以省略。参见 [https://cirosantilli.com/markdown-style-guide#dollar-signs-in-shell-code](https://cirosantilli.com/markdown-style-guide#dollar-signs-in-shell-code) 了解更多信息。

## `MD018` - atx 样式标题上的哈希值后没有空格

标签： `atx` 、 `headings` 、 `spaces`

别名： `no-missing-space-atx`

可修复：某些违规行为可以通过工具修复

当 atx 样式标题中的井号字符后缺少空格时，会触发此规则：

```markdown
#Heading 1

##Heading 2
```

要解决此问题，请用一个空格将标题文本与井号字符分开：

```markdown
# Heading 1

## Heading 2
```

理由：违反此规则可能会导致内容呈现不正确。

## `MD019` - atx 样式标题上的哈希值后有多个空格

标签： `atx` 、 `headings` 、 `spaces`

别名： `no-multiple-space-atx`

可修复：某些违规行为可以通过工具修复

当使用多个空格将 atx 样式标题中的标题文本与井号字符分隔时，将触发此规则：

```markdown
#  Heading 1

##  Heading 2
```

要解决此问题，请用一个空格将标题文本与井号字符分开：

```markdown
# Heading 1

## Heading 2
```

理由：额外的空间没有任何用途，也不会影响内容的呈现。

## `MD020` - 封闭式 atx 样式标题上的哈希值内没有空格

标签： `atx_closed` 、 `headings` 、 `spaces`

别名： `no-missing-space-closed-atx`

可修复：某些违规行为可以通过工具修复

当封闭的 atx 样式标题中的井号字符内缺少空格时，会触发此规则：

```markdown
#Heading 1#

##Heading 2##
```

要解决此问题，请用一个空格将标题文本与井号字符分开：

```markdown
# Heading 1 #

## Heading 2 ##
```

注意：如果标题两侧缺少空格，则此规则将触发。

理由：违反此规则可能会导致内容呈现不正确。

## `MD021` - 封闭式 atx 样式标题上的井号内有多个空格

标签： `atx_closed` 、 `headings` 、 `spaces`

别名： `no-multiple-space-closed-atx`

可修复：某些违规行为可以通过工具修复

当使用多个空格将封闭的 atx 样式标题中的标题文本与井号字符分隔开时，将触发此规则：

```markdown
#  Heading 1  #

##  Heading 2  ##
```

要解决此问题，请用一个空格将标题文本与井号字符分开：

```markdown
# Heading 1 #

## Heading 2 ##
```

注意：如果标题的两侧包含多个空格，则此规则将触发。

理由：额外的空间没有任何用途，也不会影响内容的呈现。

## `MD022` - 标题应由空行包围

标签： `blank_lines` 、 `headings`

别名： `blanks-around-headings`

参数：

*   `lines_above` ：标题上方的空白行（ `integer|integer[]` ，默认为 `1` ）
*   `lines_below` ：标题下方的空行（ `integer|integer[]` ，默认为 `1` ）

可修复：某些违规行为可以通过工具修复

当标题（任何样式）前面或后面没有至少一个空行时，将触发此规则：

```markdown
# Heading 1
Some text

Some more text
## Heading 2
```

要解决此问题，请确保所有标题前后都有一个空行（标题位于文档开头或结尾的情况除外）：

```markdown
# Heading 1

Some text

Some more text

## Heading 2
```

参数 `lines_above` 和 `lines_below` 可用于指定每个标题上方或下方不同数量的空行（包括 `0` ）。如果任一参数使用值 `-1` ，则允许任意数量的空行。要分别自定义每个标题级别上方或下方的行数，请指定一个 `number[]` 其中的值对应于标题级别 1-6（按顺序）。

注意：如果 `lines_above` 或 `lines_below` 配置为需要多个空行，则还应自定义 [MD012/no-multiple-blanks](md012.md) 。此规则*至少*会检查指定数量的空行；任何多余的空行都会被忽略。

理由：除了美观的原因之外，一些解析器（包括 `kramdown` ）不会解析之前没有空行的标题，而是将它们解析为常规文本。

## `MD023` - 标题必须从行首开始

标签： `headings` 、 `spaces`

别名： `heading-start-left`

可修复：某些违规行为可以通过工具修复

当标题缩进一个或多个空格时，会触发此规则：

```markdown
Some text

  # Indented heading
```

要解决此问题，请确保所有标题都从行首开始：

```markdown
Some text

# Heading
```

请注意，块引号等场景会“缩进”行首，因此以下内容也是正确的：

```markdown
> # Heading in Block Quote
```

理由：不是从行首开始的标题将不会被解析为标题，而是会显示为常规文本。

## `MD024` - 具有相同内容的多个标题

标签： `headings`

别名： `no-duplicate-heading`

参数：

*   `siblings_only` ：仅检查兄弟标题（ `boolean` ，默认为 `false` ）

如果文档中有多个标题具有相同的文本，则会触发此规则：

```markdown
# Some text

## Some text
```

要解决此问题，请确保每个标题的内容不同：

```markdown
# Some text

## Some more text
```

如果参数 `siblings_only` 设置为 `true` ，则允许具有不同父级的标题进行重复（这在变更日志中很常见）：

```markdown
# Change log

## 1.0.0

### Features

## 2.0.0

### Features
```

理由：一些 Markdown 解析器根据标题名称为标题生成锚点；具有相同内容的标题可能会导致问题。

## `MD025` 同一文档中有多个顶级标题

标签： `headings`

别名： `single-h1` 、 `single-title`

参数：

*   `front_matter_title` ：用于匹配前言标题的正则表达式（ `string` ，默认为 `^\s*title\s*[:=]` ）
*   `level` ：标题级别（ `integer` ，默认为 `1` ）

当使用顶级标题（文件的第一行是 h1 标题），并且文档中使用多个 h1 标题时，会触发此规则：

```markdown
# Top level heading

# Another top-level heading
```

要解决这个问题，请调整文档结构，使其只有一个 h1 标题作为文档标题。后续标题必须是低级标题（h2、h3 等）：

```markdown
# Title

## Heading

## Another heading
```

注意：在外部添加 h1 的情况下，可以使用 `level` 参数来更改顶级（例如：更改为 h2）。

如果 [YAML](https://en.wikipedia.org/wiki/YAML) 头条内容存在且包含 `title` 属性（通常用于博客文章），则此规则会将其视为顶级标题，并将报告任何后续顶级标题的违规行为。要在头条内容中使用其他属性名称，请通过 `front_matter_title` 参数指定正则表达式的文本。要禁用此规则对头条内容的使用，请 `""` `front_matter_title` 。

原理：顶级标题是文件第一行的 h1，用作文档的标题。如果使用此约定，则文档不能有多个标题，并且整个文档应包含在该标题中。

## `MD026` - 标题中的尾随标点符号

标签： `headings`

别名： `no-trailing-punctuation`

参数：

*   `punctuation` ：标点符号（ `string` ，默认 `.,;:!。，；：！` ）

可修复：某些违规行为可以通过工具修复

任何标题如果以指定的正常或全角标点符号之一作为行中的最后一个字符，就会触发此规则：

```markdown
# This is a heading.
```

要解决此问题，请删除尾随标点符号：

```markdown
# This is a heading
```

注意： `punctuation` 参数可用于指定字符计数 作为标题末尾的标点符号。例如，您可以将其更改为 `".,;:"` 允许以感叹号结尾的标题。 `?` 默认允许，因为它在 FAQ 类文档的标题中很常见。将 `punctuation` 参数设置为 `""` 则允许所有字符，相当于禁用该规则。

注意： [HTML 实体引用](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references)的尾部分号 诸如 `©` 、 `©` 和 `©` 之类的内容将被该规则忽略。

理由：标题并非完整的句子。更多信息： [标题末尾的标点符号](https://cirosantilli.com/markdown-style-guide#punctuation-at-the-end-of-headers) 。

## `MD027` - 块引用符号后有多个空格

标签： `blockquote` 、 `indentation` 、 `whitespace`

别名： `no-multiple-space-blockquote`

参数：

*   `list_items` ：包含列表项（ `boolean` ，默认为 `true` ）

可修复：某些违规行为可以通过工具修复

当块引用（ `>` ）符号后有多个空格时，会触发此规则：

```markdown
>  This is a blockquote with bad indentation
>  there should only be one.
```

要修复此问题，请删除所有多余的空间：

```markdown
> This is a blockquote with correct
> indentation.
```

推断块引用中的预期列表缩进可能具有挑战性；将 `list_items` 参数设置为 `false` 会针对有序和无序列表项禁用此规则。

理由：一致的格式使得文档更容易理解。

## `MD028` - 引用块内的空白行

标签： `blockquote` , `whitespace`

别名： `no-blanks-blockquote`

当两个 blockquote 块之间除了一个空行之外没有任何分隔时，就会触发此规则：

```markdown
> This is a blockquote
> which is immediately followed by

> this blockquote. Unfortunately
> In some parsers, these are treated as the same blockquote.
```

要解决此问题，请确保任何相邻的块引用之间都有一些文本：

```markdown
> This is a blockquote.

And Jimmy also said:

> This too is a blockquote.
```

或者，如果它们应该是相同的引用，则在空白行的开头添加 blockquote 符号：

```markdown
> This is a blockquote.
>
> This is the same blockquote.
```

理由：一些 Markdown 解析器将把由一个或多个空行分隔的两个块引用视为同一个块引用，而其他解析器则将它们视为单独的块引用。

## `MD029` 有序列表项前缀

标签： `ol`

别名： `ol-prefix`

参数：

*   `style` ：列表样式（ `string` ，默认 `one_or_ordered` ，值 `one` / `one_or_ordered` 有序/ `ordered` / `zero` ）

此规则适用于不以“1.”开头或不包含按数字顺序递增的前缀（取决于配置的样式）的有序列表。此外，还支持不太常见的模式，即使用“0.”作为第一个前缀或所有前缀。

如果样式配置为“一”，则有效列表示例：

```markdown
1. Do this.
1. Do that.
1. Done.
```

如果样式配置为“有序”，则有效列表的示例：

```markdown
1. Do this.
2. Do that.
3. Done.
```

```markdown
0. Do this.
1. Do that.
2. Done.
```

当样式配置为“one\_or\_ordered”时，所有三个示例均有效。

如果样式配置为“零”，则有效列表示例：

```markdown
0. Do this.
0. Do that.
0. Done.
```

所有样式的无效列表示例：

```markdown
1. Do this.
3. Done.
```

此规则支持以 0 为前缀的有序列表项以实现统一缩进：

```markdown
...
08. Item
09. Item
10. Item
11. Item
...
```

注意：此规则将报告以下情况的违规行为：两个列表项之间出现缩进不正确的代码块（或类似内容），并将列表“分成”两部分：

````markdown
1. First list

```text
Code block
```

1. Second list
````

修复方法是缩进代码块，以便它按预期成为前面的列表项的一部分：

```markdown
1. First list

   ```text
   Code block
   ```

2. Still first list
```

理由：一致的格式使得文档更容易理解。

## `MD030` - 列表标记后的空格

标签： `ol` 、 `ul` 、 `whitespace`

别名： `list-marker-space`

参数：

*   `ol_multi` ：多行有序列表项的空间（ `integer` ，默认为 `1` ）
*   `ol_single` ：单行有序列表项的空格（ `integer` ，默认 `1` ）
*   `ul_multi` ：多行无序列表项的空间（ `integer` ，默认 `1` ）
*   `ul_single` ：单行无序列表项的空格（ `integer` ，默认 `1` ）

可修复：某些违规行为可以通过工具修复

此规则检查列表标记（例如“ `-` ”，“ `*` ”，“ `+` ”或“ `1.` ”）和列表项文本之间的空格数。

检查的空格数取决于所使用的文档样式，但默认值是任何列表标记后有 1 个空格：

```markdown
* Foo
* Bar
* Baz

1. Foo
1. Bar
1. Baz

1. Foo
   * Bar
1. Baz
```

文档样式可以独立更改无序列表项和有序列表项后的空格数，也可以基于列表中每个项的内容是由单个段落还是多个段落（包括子列表和代码块）组成。

例如， [https://cirosantilli.com/markdown-style-guide#spaces-after-list-marker](https://cirosantilli.com/markdown-style-guide#spaces-after-list-marker) 指定如果列表中的每个项目都应在列表标记后使用 1 个空格 该列表适合一个段落，但要使用 2 或 3 个空格（对于有序 和无序列表）如果有多个内容段落 列表内：

```markdown
* Foo
* Bar
* Baz
```

对比

```markdown
*   Foo

    Second paragraph

*   Bar
```

或者

```markdown
1.  Foo

    Second paragraph

1.  Bar
```

要解决此问题，请确保在所选文档样式的列表标记后使用正确数量的空格。

理由：违反此规则可能会导致内容呈现不正确。

注意：有关兼容性信息，请参阅 [Prettier.md](Prettier.md) 。

## `MD031` - 带围栏的代码块应该用空行包围

标签： `blank_lines` 、 `code`

别名： `blanks-around-fences`

参数：

*   `list_items` ：包含列表项（ `boolean` ，默认为 `true` ）

可修复：某些违规行为可以通过工具修复

当隔离代码块前面或后面没有空行时，将触发此规则：

````markdown
Some text
```
Code block
```

```
Another code block
```
Some more text
````

要解决此问题，请确保所有隔离的代码块前后都有一个空行（除非该块位于文档的开头或结尾）：

````markdown
Some text

```
Code block
```

```
Another code block
```

Some more text
````

将 `list_items` 参数设置为 `false` 以禁用列表项的此规则。 如果需要创建 包含代码围栏的[严格](https://spec.commonmark.org/0.29/#tight)列表。

理由：除了美观原因之外，一些解析器（包括 kramdown）不会解析前后没有空行的隔离代码块。

## `MD032` - 列表应该用空行包围

标签： `blank_lines` 、 `bullet` 、 `ol` 、 `ul`

别名： `blanks-around-lists`

可修复：某些违规行为可以通过工具修复

当列表（任何类型）前面或后面没有空行时，就会触发此规则：

```markdown
Some text
* List item
* List item

1. List item
2. List item
***
```

在上述第一种情况下，文本紧接在无序列表之前。在上述第二种情况下，主题断点紧接在有序列表之后。要修复违反此规则的情况，请确保所有列表前后均留空行（列表位于文档最开头或最结尾的情况除外）：

```markdown
Some text

* List item
* List item

1. List item
2. List item

***
```

请注意，以下情况**并不**违反此规则：

```markdown
1. List item
   More item 1
2. List item
More item 2
```

尽管没有缩进，但文本“更多项目 2”被称为 [惰性延续行](https://spec.commonmark.org/0.30/#lazy-continuation-line)并被视为第二个列表项的一部分。

理由：除了美观原因之外，一些解析器（包括 kramdown）不会解析前后没有空行的列表。

## `MD033` - 内联 HTML

标签： `html`

别名： `no-inline-html`

参数：

*   `allowed_elements` ：允许的元素（ `string[]` ，默认 `[]` ）

每当在 Markdown 文档中使用原始 HTML 时，就会触发此规则：

```markdown
<h1>Inline HTML heading</h1>
```

要解决此问题，请使用“纯”Markdown，而不是包含原始 HTML：

```markdown
# Markdown heading
```

注意：要允许特定的 HTML 元素，请使用 `allowed_elements` 参数。

理由：Markdown 中允许使用原始 HTML，但此规则适用于那些希望其文档仅包含“纯”Markdown 的人，或者那些将 Markdown 文档呈现为 HTML 以外内容的人。

## `MD034` - 使用裸 URL

标签： `links` , `url`

别名： `no-bare-urls`

可修复：某些违规行为可以通过工具修复

只要 URL 或电子邮件地址没有尖括号，就会触发此规则：

```markdown
For more info, visit https://www.example.com/ or email user@example.com.
```

要解决此问题，请在 URL 或电子邮件地址周围添加尖括号：

```markdown
For more info, visit <https://www.example.com/> or email <user@example.com>.
```

如果 URL 或电子邮件地址包含非 ASCII 字符，则可能无法 即使存在尖括号，也会按预期处理。在这种情况下， [百分比编码](https://en.m.wikipedia.org/wiki/Percent-encoding)可用于符合 URL 和电子邮件所需的语法。

注意：要包含裸 URL 或电子邮件而不将其转换为链接，请将其包装在代码跨度中：

```markdown
Not a clickable link: `https://www.example.com`
```

注意：以下场景不会触发此规则，因为它可能是快捷链接：

```markdown
[https://www.example.com]
```

注意：以下语法会触发此规则，因为嵌套链接可能是快捷链接（优先）：

```markdown
[text [shortcut] text](https://example.com)
```

为了避免这种情况，请转义两个内部括号：

```markdown
[link \[text\] link](https://example.com)
```

理由：如果没有尖括号，一些 Markdown 解析器就不会将裸 URL 或电子邮件转换为链接。

## `MD035` - 水平线样式

标签： `hr`

别名： `hr-style`

参数：

*   `style` ：水平规则样式（ `string` ，默认 `consistent` ）

当文档中使用不一致的水平规则样式时，会触发此规则：

```markdown
---

- - -

***

* * *

****
```

要解决此问题，请在各处使用相同的水平规则：

```markdown
---

---
```

配置的样式可以确保所有水平规则使用特定的字符串，或者可以确保所有水平规则与第一个水平规则匹配（ `consistent` ）。

理由：一致的格式使得文档更容易理解。

## `MD036` - 使用强调代替标题

标签： `emphasis` 、 `headings`

别名： `no-emphasis-as-heading`

参数：

*   `punctuation` ：标点符号（ `string` ，默认 `.,;:!?。，；：！？` ）

此检查查找使用强调（即粗体或斜体）文本来分隔章节的情况，而应使用标题：

```markdown
**My document**

Lorem ipsum dolor sit amet...

_Another section_

Consectetur adipiscing elit, sed do eiusmod.
```

要解决此问题，请使用 Markdown 标题而不是强调文本来表示章节：

```markdown
# My document

Lorem ipsum dolor sit amet...

## Another section

Consectetur adipiscing elit, sed do eiusmod.
```

注意：此规则会查找完全由强调文本组成的单行段落。常规文本、多行强调段落或以标点符号（正常或全角）结尾的段落中使用的强调不会触发此规则。与规则 MD026 类似，您可以配置哪些字符会被识别为标点符号。

理由：使用强调而不是标题可以防止工具推断 文档的结构。更多信息： [https://cirosantilli.com/markdown-style-guide#emphasis-vs-headers](https://cirosantilli.com/markdown-style-guide#emphasis-vs-headers) 。

## `MD037` - 强调标记内的空格

标签： `emphasis` 、 `whitespace`

别名： `no-space-in-emphasis`

可修复：某些违规行为可以通过工具修复

当使用强调标记（粗体、斜体），但标记和文本之间有空格时，会触发此规则：

```markdown
Here is some ** bold ** text.

Here is some * italic * text.

Here is some more __ bold __ text.

Here is some more _ italic _ text.
```

要解决此问题，请删除强调标记周围的空格：

```markdown
Here is some **bold** text.

Here is some *italic* text.

Here is some more __bold__ text.

Here is some more _italic_ text.
```

解释：只有当星号/下划线两边没有空格时，才会解析为强调。此规则尝试检测它们被空格包围的位置，但看起来作者有意强调文本。

## `MD038` - 代码跨度元素内的空格

标签： `code` 、 `whitespace`

别名： `no-space-in-code`

可修复：某些违规行为可以通过工具修复

当代码跨度包含开头或结尾反引号旁边带有不必要空格的内容时，会触发此规则：

```markdown
`some text `

` some text`

`   some text   `
```

要解决此问题，请删除开头和结尾的多余空格字符：

```markdown
`some text`
```

注意：规范允许使用单个前导空格*和*尾随空格，并由解析器修剪以支持以反引号开头或结尾的代码跨度：

```markdown
`` `backticks` ``

`` backtick` ``
```

注意：当输入中存在单空格填充时，它将被保留（即使不需要）：

```markdown
` code `
```

注意：规范允许仅包含空格的代码跨度，并且也会保留：

```markdown
` `

`   `
```

理由：违反此规则通常是无意的，并且可能导致内容呈现不正确。

## `MD039` - 链接文本内的空格

标签： `links` 、 `whitespace`

别名： `no-space-in-links`

可修复：某些违规行为可以通过工具修复

链接文本周围有空格的链接会触发此规则：

```markdown
[ a link ](https://www.example.com/)
```

要解决此问题，请删除链接文本周围的空格：

```markdown
[a link](https://www.example.com/)
```

理由：一致的格式使得文档更容易理解。

## `MD040` - 围栏代码块应指定语言

标签： `code` 、 `language`

别名： `fenced-code-language`

参数：

*   `allowed_languages` 语言列表（ `string[]` ，默认 `[]` ）
*   `language_only` ：仅需要语言（ `boolean` ，默认为 `false` ）

当使用隔离代码块但未指定语言时，会触发此规则：

````markdown
```
#!/bin/bash
echo Hello world
```
````

要解决此问题，请向代码块添加语言说明符：

````markdown
```bash
#!/bin/bash
echo Hello world
```
````

要显示不带语法高亮的代码块，请使用：

````markdown
```text
Plain text in a code block
```
````

您可以配置 `allowed_languages` 参数来指定代码块可以使用的语言列表。语言区分大小写。默认值为 `[]` ，表示任何语言说明符均有效。

您可以阻止隔离代码块的信息字符串中出现额外数据。为此，请将 `language_only` 参数设置为 `true` 。

带有前导/尾随空格（例如： `js` ）或其他内容的信息字符串（例如： `ruby startline=3` ）将触发此规则。

理由：指定语言可以通过使用 正确突出显示代码的语法。更多信息： [https://cirosantilli.com/markdown-style-guide#option-code-fenced](https://cirosantilli.com/markdown-style-guide#option-code-fenced) 。

## `MD041` - 文件的第一行应为顶级标题

标签： `headings`

别名： `first-line-h1` 、 `first-line-heading`

参数：

*   `allow_preamble` ：允许第一个标题之前的内容（ `boolean` ，默认值 `false` ）
*   `front_matter_title` ：用于匹配前言标题的正则表达式（ `string` ，默认为 `^\s*title\s*[:=]` ）
*   `level` ：标题级别（ `integer` ，默认为 `1` ）

此规则旨在确保文档具有标题，并在文档的第一行不是顶级（ [HTML](https://en.wikipedia.org/wiki/HTML) `h1` ）标题时触发：

```markdown
This is a document without a heading
```

要解决此问题，请在文档开头添加顶级标题：

```markdown
# Document Heading

This is a document with a top-level heading
```

因为 GitHub 上的项目通常使用图像作为标题 `README.md` ，并且 Markdown 对该格式的支持不够完善，因此本规则也允许使用 HTML 标题。例如：

```markdown
<h1 align="center"><img src="https://placekitten.com/300/150"/></h1>

This is a document with a top-level HTML heading
```

在某些情况下，文档标题前面可能会有类似目录的文本。这对于无障碍访问而言并不理想，但可以通过将 `allow_preamble` 参数设置为 `true` 来实现。

```markdown
This is a document with preamble text

# Document Heading
```

如果 [YAML](https://en.wikipedia.org/wiki/YAML) 头条内容存在且包含 `title` 属性（通常用于博客文章），则此规则不会报告违规。要在头条内容中使用其他属性名称，请通过 `front_matter_title` 参数指定[正则表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions)的文本。要禁用此规则对头条内容的使用，请将 `front_matter_title` 指定为 `""` 。

在从外部添加 `h1` 的情况下，可以使用 `level` 参数来更改顶级标题（例如：更改为 `h2` ）。

原理：顶级标题通常充当文档的标题。更多信息： [https://cirosantilli.com/markdown-style-guide#top-level-header](https://cirosantilli.com/markdown-style-guide#top-level-header) 。

## `MD042` - 无空链接

标签： `links`

别名： `no-empty-links`

当遇到空链接时会触发此规则：

```markdown
[an empty link]()
```

要修复违规，请提供链接的目标：

```markdown
[a valid link](https://example.com/)
```

空片段将触发此规则：

```markdown
[an empty fragment](#)
```

但非空片段不会：

```markdown
[a valid fragment](#fragment)
```

理由：空链接不会指向任何地方，因此不具备链接功能。

## `MD043` - 必需的标题结构

标签： `headings`

别名： `required-headings`

参数：

*   `headings` ：标题列表（ `string[]` ，默认 `[]` ）
*   `match_case` ：匹配标题的大小写（ `boolean` ，默认为 `false` ）

当文件中的标题与传递给规则的标题数组不匹配时，会触发此规则。它可用于强制一组文件采用标准标题结构。

要求恰好具有以下结构：

```markdown
# Heading
## Item
### Detail
```

将 `headings` 参数设置为：

```json
[
    "# Heading",
    "## Item",
    "### Detail"
]
```

允许可选标题，结构如下：

```markdown
# Heading
## Item
### Detail (optional)
## Foot
### Notes (optional)
```

使用特殊值 `"*"` （表示“零个或多个未指定的标题”）或特殊值 `"+"` （表示“一个或多个未指定的标题”），并设置 `headings` 参数为：

```json
[
    "# Heading",
    "## Item",
    "*",
    "## Foot",
    "*"
]
```

要允许单个必需标题随项目名称而变化：

```markdown
# Project Name
## Description
## Examples
```

使用特殊值 `"?"` ，表示“恰好一个未指定的标题”：

```json
[
    "?",
    "## Description",
    "## Examples"
]
```

当检测到错误时，此规则输出第一个有问题的标题的行号（否则，输出文件的最后一个行号）。

请注意，虽然 `headings` 参数为了简单起见使用了“## Text”ATX 标题样式，但文件可以使用任何受支持的标题样式。

默认情况下，文档中标题的大小写不需要与 `headings` 的大小写完全匹配。如果要求大小写完全匹配，请设置 `match_case` 参数为 `true` 。

理由：项目可能希望在一组类似的内容中强制实施一致的文档结构。

## `MD044` - 专有名词应使用正确的大写字母

标签： `spelling`

别名： `proper-names`

参数：

*   `code_blocks` ：包含代码块（ `boolean` ，默认 `true` ）
*   `html_elements` ：包含 HTML 元素（ `boolean` ，默认 `true` ）
*   `names` ：专有名词列表（ `string[]` ，默认为 `[]` ）

可修复：某些违规行为可以通过工具修复

当 `names` 数组中的任何字符串未按指定大小写时，就会触发此规则。此规则可用于强制项目和产品名称采用标准字母大小写。

例如，“JavaScript”语言通常同时使用“J”和 'S' 大写 - 尽管有时 's' 或 'j' 以小写形式出现。 强制正确的大写，在 `names` 数组：

```json
[
    "JavaScript"
]
```

有时，专有名词在特定上下文中的大小写形式会有所不同。在这种情况下，请将两种形式都添加到 `names` 数组中：

```json
[
    "GitHub",
    "github.com"
]
```

将 `code_blocks` 参数设置为 `false` 可针对代码块和 span 禁用此规则。将 `html_elements` 参数设置为 `false` 可针对 HTML 元素和属性禁用此规则（例如，当使用专有名称作为 `a` `href` 或 `img` / `src` 路径的一部分时）。

理由：专有名词的大写不正确通常是一个错误。

## `MD045` - 图像应有替代文本（alt text）

标签： `accessibility` 、 `images`

别名： `no-alt-text`

当图像缺少替代文本（alt text）信息时，此规则会报告违规。

替代文本通常以内联形式指定为：

```markdown
![Alternate text](image.jpg)
```

或者参考语法如下：

```markdown
![Alternate text][ref]

...

[ref]: image.jpg "Optional title"
```

或者使用 HTML 格式：

```html
<img src="image.jpg" alt="Alternate text" />
```

注意：如果使用 [HTML `aria-hidden` 属性](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-hidden)来隐藏辅助技术中的图像，则此规则不会报告违规：

```html
<img src="image.jpg" aria-hidden="true" />
```

有关编写替代文本的指南可从 [W3C](https://www.w3.org/WAI/alt/) 获取， [维基百科](https://en.wikipedia.org/wiki/Alt_attribute)和[其他地方](https://www.phase2technology.com/blog/no-more-excuses) 。

理由：替代文本对于可访问性很重要，它可以向无法看到图像的人描述图像的内容。

## `MD046` - 代码块样式

标签： `code`

别名： `code-block-style`

参数：

*   `style` ：块样式（ `string` ，默认 `consistent` ，值 `consistent` / `fenced` / `indented` ）

当在同一文档中使用不需要的或不同的代码块样式时，会触发此规则。

在默认配置中，此规则报告以下文档的违规行为：

````
Some text.

    # Indented code

More text.

```ruby
# Fenced code
```

More text.
````

要修复违反此规则的行为，请使用一致的样式（缩进或代码围栏）。

配置的代码块样式可以是特定的（ `fenced` ， `indented` ）或者可以要求所有代码块与第一个代码块匹配（ `consistent` ）。

理由：一致的格式使得文档更容易理解。

## `MD047` - 文件应以单个换行符结尾

标签： `blank_lines`

别名： `single-trailing-newline`

可修复：某些违规行为可以通过工具修复

当文件末尾没有换行符时，就会触发此规则。

触发规则的示例：

```markdown
# Heading

This file ends without a newline.[EOF]
```

要修复违规，请在文件末尾添加换行符：

```markdown
# Heading

This file ends with a newline.
[EOF]
```

原因：某些程序在处理不以换行符结尾的文件时会遇到问题。

更多信息： [在文件末尾添加新行有什么意义？](https://unix.stackexchange.com/questions/18743/whats-the-point-in-adding-a-new-line-to-the-end-of-a-file)

## `MD048` - 代码栅栏样式

标签： `code`

别名： `code-fence-style`

参数：

*   `style` ：代码栅栏样式（ `string` ，默认 `consistent` ，值 `backtick` / `consistent` / `tilde` )

当文档中用于围栏代码块的符号与配置的代码围栏样式不匹配时，将触发此规则：

````markdown
```ruby
# Fenced code
```

~~~ruby
# Fenced code
~~~
````

要解决此问题，请在整个文档中使用配置的代码围栏样式：

````markdown
```ruby
# Fenced code
```

```ruby
# Fenced code
```
````

配置的代码围栏样式可以是要使用的特定符号（ `backtick` ， `tilde` ）或者它可以要求所有代码栅栏与第一个代码栅栏匹配（ `consistent` ）。

理由：一致的格式使得文档更容易理解。

## `MD049` - 强调风格

标签： `emphasis`

别名： `emphasis-style`

参数：

*   `style` ：强调样式（ `string` ，默认 `consistent` ，值星 `asterisk` / `consistent` / `underscore` )

可修复：某些违规行为可以通过工具修复

当文档中用于强调的符号与配置的强调样式不匹配时，会触发此规则：

```markdown
*Text*
_Text_
```

要解决此问题，请在整个文档中使用配置的强调样式：

```markdown
*Text*
*Text*
```

配置的强调样式可以是要使用的特定符号（ `asterisk` ， `underscore` ）或者可以要求所有强调都与第一个强调相匹配（ `consistent` ）。

注意：单词内的强调仅限于 `asterisk` ，以避免对包含内部下划线的单词（如 this\_one）产生不必要的强调。

理由：一致的格式使得文档更容易理解。

## `MD050` 强劲风格

标签： `emphasis`

别名： `strong-style`

参数：

*   `style` ：强风格（ `string` ，默认 `consistent` ，值星 `asterisk` / `consistent` / `underscore` )

可修复：某些违规行为可以通过工具修复

当文档中用于强的符号与配置的强样式不匹配时，会触发此规则：

```markdown
**Text**
__Text__
```

要解决此问题，请在整个文档中使用配置的强样式：

```markdown
**Text**
**Text**
```

配置的强样式可以是要使用的特定符号（ `asterisk` ， `underscore` ）或者可以要求所有强匹配第一个强（ `consistent` ）。

注意：单词内的强调仅限于 `asterisk` ，以避免对包含内部下划线的单词（如\_\_this\_\_one）产生不必要的强调。

理由：一致的格式使得文档更容易理解。

## `MD051` - 链接片段应该有效

标签： `links`

别名： `link-fragments`

参数：

*   `ignore_case` ：忽略片段的大小写（ `boolean` ，默认为 `false` ）
*   `ignored_pattern` ：忽略附加片段的模式（ `string` ，默认值为“”）

可修复：某些违规行为可以通过工具修复

当链接片段与文档标题自动生成的任何片段不匹配时，就会触发此规则：

```markdown
# Heading Name

[Link](#fragment)
```

要解决此问题，请更改链接片段以引用现有标题的生成名称（见下文）：

```markdown
# Heading Name

[Link](#heading-name)
```

为了保持一致性，此规则要求片段必须与 [GitHub 标题算法](https://github.com/gjtorikian/html-pipeline/blob/f13a1534cb650ba17af400d1acd3a22c28004c09/lib/html/pipeline/toc_filter.rb)完全匹配，该算法会将字母转换为小写。因此，以下示例将被报告为违规：

```markdown
# Heading Name

[Link](#Heading-Name)
```

要在比较片段与标题名称时忽略大小写， `ignore_case` 参数可以设置为 `true` 。在此配置下，前面的示例不会被报告为违规。

或者，某些平台允许在标题中使用语法 `{#named-anchor}` 来提供特定名称（仅由小写字母、数字、 `-` 和 `_` 组成）：

```markdown
# Heading Name {#custom-name}

[Link](#custom-name)
```

或者，任何带有 `id` 属性的 HTML 标签或带有 `name` 的 `a` 标签 属性可用于定义片段：

```markdown
<a id="bookmark"></a>

[Link](#bookmark)
```

当标题不合适或需要控制片段标识符的文本时， `a` 标签很有用。

[HTML 链接 `#top` 滚动到文档顶部](https://html.spec.whatwg.org/multipage/browsing-the-web.html#scrolling-to-a-fragment) 。此规则允许该语法（为了保持一致性，使用小写）：

```markdown
[Link](#top)
```

此规则还识别 GitHub 用于突出显示的自定义片段语法 [文档中的特定内容](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-a-permanent-link-to-a-code-snippet#linking-to-markdown#linking-to-markdown) 。

例如，以下链接到第 20 行：

```markdown
[Link](#L20)
```

这是从第 19 行到第 21 行的内容链接：

```markdown
[Link](#L19C5-L21C11)
```

一些 Markdown 生成器在构建文档时会动态创建和插入标题，例如通过组合固定前缀（如 `figure-` ）和 递增数字计数器。要忽略此类生成的碎片，请设置 `ignored_pattern` [正则表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions)参数为匹配的模式（例如， `^figure-` ）。

原理：当 Markdown 内容在 GitHub 上显示时，每个标题都会自动创建 [GitHub 章节链接](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#section-links) 。这样可以轻松地直接链接到文档中的不同章节。但是，如果标题被重命名或删除，章节链接也会随之更改。此规则有助于识别文档中损坏的章节链接。

章节链接**不**属于 CommonMark 规范。此规则强制执行 [GitHub 标题算法](https://github.com/gjtorikian/html-pipeline/blob/f13a1534cb650ba17af400d1acd3a22c28004c09/lib/html/pipeline/toc_filter.rb) ，该算法为：将标题转换为小写，删除标点符号，将空格转换为破折号，并根据需要附加一个递增的整数以保证唯一性。

## `MD052` - 参考链接和图像应使用已定义的标签

标签： `images` 、 `links`

别名： `reference-links-images`

参数：

*   `ignored_labels` ：忽略的链接标签（ `string[]` ，默认 `["x"]` ）
*   `shortcut_syntax` ：包含快捷语法（ `boolean` ，默认为 `false` ）

Markdown 中的链接和图片可以在使用时提供链接目标或图片来源，也可以在其他位置定义并使用标签进行引用。引用格式方便保持段落文本整洁，并易于在多个位置重复使用相同的 URL。

参考链接和图片有三种：

```markdown
Full: [text][label]
Collapsed: [label][]
Shortcut: [label]

Full: ![text][image]
Collapsed: ![image][]
Shortcut: ![image]

[label]: https://example.com/label
[image]: https://example.com/image
```

当定义了相应的标签时，链接或图像会正确呈现，但当标签不存在时，则会显示为带括号的文本。默认情况下，此规则会针对“完整”和“折叠”引用语法发出未定义标签的警告，但不会针对“快捷方式”语法发出警告，因为该语法具有歧义性。

文本 `[example]` 可以是快捷方式链接，也可以是括号中的文本“example”，因此默认情况下会忽略“快捷方式”语法。要包含“快捷方式”语法，请将 `include_shortcut` 参数设置为 `true` 。请注意，这样做会对文档中*所有**可能*成为快捷方式的文本发出警告。如果有意使用括号中的文本，可以使用 `\` 字符对括号进行转义： `\[example\]` 。

如果有故意不引用的链接标签，可以通过将 `ignored_labels` 参数设置为要忽略的字符串列表来忽略它们。 此参数的默认值忽略了复选框语法 [GitHub Flavored Markdown 任务列表项](https://github.github.com/gfm/#task-list-items-extension-) ：

```markdown
- [x] Checked task list item
```

## `MD053` - 需要链接和图像参考定义

标签： `images` 、 `links`

别名： `link-image-reference-definitions`

参数：

*   `ignored_definitions` ：忽略的定义（ `string[]` ，默认 `["//"]` ）

可修复：某些违规行为可以通过工具修复

Markdown 中的链接和图片可以在使用时提供链接目标或图片来源，也可以使用标签引用文档中其他位置的定义。后一种引用格式方便保持段落文本整洁，并易于在多个位置重复使用相同的 URL。

由于链接和图像引用定义与使用位置分开，因此在两种情况下不需要定义：

1.  如果标签未被文档中的任何链接或图像引用，则该定义未使用，可以删除。
2.  如果一个标签在文档中被定义多次，则使用第一个定义，其他定义可以删除。

如果任何链接或图像引用具有相应的标签，则此规则会考虑使用引用定义。“完整”、“折叠”和“快捷方式”格式均受支持。

如果存在故意未引用的引用定义，可以通过将 `ignored_definitions` 参数设置为要忽略的字符串列表来忽略它们。此参数的默认值忽略了以下向 Markdown 添加非 HTML 注释的约定：

```markdown
[//]: # (This behaves like a comment)
```

## `MD054` - 链接和图像样式

标签： `images` 、 `links`

别名： `link-image-style`

参数：

*   `autolink` ：允许自动链接（ `boolean` ，默认 `true` ）
*   `collapsed` ：允许折叠参考链接和图像（ `boolean` ，默认 `true` ）
*   `full` ：允许完整的参考链接和图像（ `boolean` ，默认为 `true` ）
*   `inline` ：允许内联链接和图像（ `boolean` ，默认为 `true` ）
*   `shortcut` ：允许快捷方式参考链接和图像（ `boolean` ，默认 `true` ）
*   `url_inline` ：允许 URL 作为内联链接（ `boolean` ，默认为 `true` ）

可修复：某些违规行为可以通过工具修复

Markdown 中的链接和图片可以在使用时提供链接目标或图片来源，也可以使用标签引用文档中其他位置的定义。这三种引用格式方便保持段落文本整洁，并易于在多个位置重复使用相同的 URL。

默认情况下，此规则允许所有链接/图像样式。

将 `autolink` 参数设置为 `false` 会禁用自动链接：

```markdown
<https://example.com>
```

将 `inline` 参数设置为 `false` 会禁用内联链接和图像：

```markdown
[link](https://example.com)

![image](https://example.com)
```

将 `full` 参数设置为 `false` 会禁用完整参考链接和图像：

```markdown
[link][url]

![image][url]

[url]: https://example.com
```

将 `collapsed` 参数设置为 `false` 会禁用折叠的参考链接和图像：

```markdown
[url][]

![url][]

[url]: https://example.com
```

将 `shortcut` 参数设置为 `false` 会禁用快捷方式参考链接和图像：

```markdown
[url]

![url]

[url]: https://example.com
```

要修复违反此规则的情况，请将链接或图片更改为允许的样式。当链接或图片可以转换为 `inline` 样式（首选）或链接可以转换为 `autolink` 样式（不支持图像，且必须是绝对 URL）。此规则*无法*修复需要将链接或图像转换为 `full` 、 `collapsed` 或 `shortcut` 引用样式的情况，因为这涉及到命名引用以及确定在文档中插入的位置。

将 `url_inline` 参数设置为 `false` 可防止使用具有相同绝对 URL 文本/目标且没有标题的内联链接，因为此类链接可以转换为自动链接：

```markdown
[https://example.com](https://example.com)
```

要修复 `url_inline` 违规，请使用更简单的自动链接语法：

```markdown
<https://example.com>
```

理由：一致的格式使文档更易于理解。自动链接简洁，但显示为 URL，可能冗长且令人困惑。内联链接和图像可以包含描述性文本，但在 Markdown 格式中会占用更多空间。参考链接和图像在 Markdown 格式中更易于阅读和操作，但需要单独定义链接参考。

## `MD055` - 桌面管式

标签： `table`

别名： `table-pipe-style`

参数：

*   `style` ：表管样式（ `string` ，默认 `consistent` ，值 `consistent` / `leading_and_trailing` / `leading_only` / `no_leading_or_trailing` / `trailing_only` )

当 [GitHub Flavored Markdown 表](https://github.github.com/gfm/#tables-extension-) 前导和尾随管道字符 ( `|` ) 的使用不一致。

默认情况下（ `consistent` 样式），文档中第一个表格的标题行用于确定文档中每个表格强制执行的样式。您可以使用特定样式（ `leading_and_trailing` 、 `leading_only` 、 `no_leading_or_trailing` 或 `trailing_only` )。

该表的标题行有前导管道和尾随管道，但其分隔符行缺少尾随管道，并且其第一行单元格缺少前导管道：

```markdown
| Header | Header |
| ------ | ------
  Cell   | Cell   |
```

要解决这些问题，请确保每行的开头和结尾都有一个管道字符：

```markdown
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
```

请注意，紧跟在表格后面的文本（即，没有用空行分隔）将被视为表格的一部分（根据规范），并且也可能触发此规则：

```markdown
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
This text is part of the table
```

原因：某些解析器难以处理缺少前导或尾随竖线字符的表。使用前导/尾随竖线字符也有助于提高视觉清晰度。

## `MD056` 表格列数

标签： `table`

别名： `table-column-count`

当 [GitHub Flavored Markdown 表](https://github.github.com/gfm/#tables-extension-) 每行的单元格数量并不相同。

该表的第二个数据行单元格太少，而第三个数据行单元格太多：

```markdown
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
| Cell   |
| Cell   | Cell   | Cell   |
```

要解决这些问题，请确保每行都有相同数量的单元格：

```markdown
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
| Cell   | Cell   |
| Cell   | Cell   |
```

请注意，表格的标题行和分隔符行必须具有相同数量的单元格，否则将不会被识别为表格（根据规范）。

原理：一行中多余的单元格通常不会显示，因此其数据会丢失。一行中缺失的单元格会在表格中留下空洞，暗示存在遗漏。

## `MD058` - 表格周围应有空行

标签： `table`

别名： `blanks-around-tables`

可修复：某些违规行为可以通过工具修复

当表格前面或后面没有空行时，会触发此规则：

```markdown
Some text
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
> Blockquote
```

要修复违反此规则的情况，请确保所有表格前后都有一个空行（除非表格位于文档的最开始或最结束处）：

```markdown
Some text

| Header | Header |
| ------ | ------ |
| Cell   | Cell   |

> Blockquote
```

请注意，紧跟在表格后面的文本（即，没有用空行分隔）将被视为表格的一部分（根据规范），并且不会触发此规则：

```markdown
| Header | Header |
| ------ | ------ |
| Cell   | Cell   |
This text is part of the table and the next line is blank

Some text
```

理由：除了美观的原因之外，一些解析器会错误地解析前后没有空行的表格。

## `MD059` - 链接文本应具有描述性

标签： `accessibility` 、 `links`

别名： `descriptive-link-text`

参数：

*   `prohibited_texts` ：禁止链接文本（ `string[]` ，默认 `["click here","here","link","more"]` ）

当链接包含诸如 `[click here](...)` 或 `[link](...)` 。

链接文本应具有描述性，并能传达链接的目的（例如， `[Download the budget document](...)` 或 `[CommonMark Specification](...)` ）。这对于屏幕阅读器来说尤其重要，因为屏幕阅读器有时会显示没有上下文的链接。

默认情况下，此规则禁止使用少量常见的英语单词/短语。如需自定义单词/短语列表，请将 `prohibited_texts` 参数设置为 `string` `Array` 。

注意：对于英语以外的语言，请使用 `prohibited_texts` 参数自定义该语言的列表。此规则的目标*并非*为每种语言提供翻译。

注意：此规则检查 Markdown 链接；HTML 链接将被忽略。

更多信息： [https://webaim.org/techniques/hypertext/](https://webaim.org/techniques/hypertext/)