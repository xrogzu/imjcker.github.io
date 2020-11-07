---
layout: post
title: markdown all in one
category: 笔记
tags: markdown
---

# 2018-01-12-markdown

**超链接**

```text
[百年孤独](http://blog.imjcker.com)

<https://www.github.com/imjcker>
```

[百年孤独](http://blog.imjcker.com)

[https://github.com/imjcker](https://github.com/imjcker)

**列表**

```text
1. 有序列表项 1

2. 有序列表项 2

3. 有序列表项 3
```

1. 有序列表项 1
2. 有序列表项 2
3. 有序列表项 3

```text
* 无序列表项 1

* 无序列表项 2

* 无序列表项 3
```

* 无序列表项 1
* 无序列表项 2
* 无序列表项 3

```text
- [x] 任务列表 1
- [ ] 任务列表 2
```

* [x] 任务列表 1
* [ ] 任务列表 2

**强调**

```text
~~删除线~~

**加黑**

*斜体*
```

~~删除线~~

**加黑**

_斜体_

**标题**

```text
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

Tips: `#` 与标题中间要加空格。

**表格**

```text
| HEADER1 | HEADER2 | HEADER3 | HEADER4 |
| ------- | :------ | :-----: | ------: |
| content | content | content | content |
```

| HEADER1 | HEADER2 | HEADER3 | HEADER4 |
| :--- | :--- | :---: | ---: |
| content | content | content | content |

1. :----- 表示左对齐
2. :----: 表示中对齐
3. -----: 表示右对齐

**代码块**

```python
print 'Hello, World!'
```

**图片**

```text
![本站favicon](./images/icon.jpg)
```

![&#x672C;&#x7AD9;favicon](https://github.com/imjcker/imjcker.github.io/tree/c1b5e714314fd24c00cbfce8106ec78265e25d67/_posts/images/icon.jpg)

**锚点**

```text
* [目录](#目录)
```

* [目录](markdown.md#目录)

**表情**Emoji

🐫 😊 😄

**脚标**Footnotes

This is a text with footnote.

**mermaid**

 sequenceDiagram Alice--&gt;&gt;John: Hello John, how are you? John--&gt;&gt;Alice: Great!

```text
sequenceDiagram

    Alice-->>John: Hello John, how are you?

    John-->>Alice: Great!
```

**序列图**sequence

```text
Andrew->China: Says Hello
Note right of China: China thinks\nabout it
China-->Andrew: How are you?
Andrew->>China: I am good thanks!
China-->Andrew: what is wrong with you?
```

**流程图**flowchart

```text
st=>start: Start
e=>end
op1=>operation: My Operation
sub1=>subroutine: My Subroutine
cond=>condition: Yes
or No?
io=>inputoutput: catch something...

st->op1->cond
cond(yes)->io->e
cond(no)->sub1(right)->op1
```

**数学公式**mathjax

When $$(a \ne 0)$$, there are two solutions to $$(ax^2 + bx + c = 0)$$ and they are

$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$

water : H~2~O
