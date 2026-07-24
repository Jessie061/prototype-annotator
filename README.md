# 原型批注 / Prototype Annotator

> 为产品经理和设计师打造的 HTML 原型批注工具，支持在原型页面上添加、查看、管理批注标记，并将批注数据持久化内嵌到 HTML 文件中，便于团队协作与交付。

---

## 中文介绍

### 概述

**原型批注** 是一个面向产品经理和设计师的 Skill 工具，用于在 HTML 原型页面上进行批注标注。无需安装任何额外软件，只需将 Skill 引入项目，即可在原型页面顶部出现悬浮控制栏，支持添加、查看、编辑、删除批注，并将批注数据直接写入 HTML 文件，实现批注与原型的持久化绑定。

### 核心功能

#### 1. 悬浮控制栏与模式切换

在原型页面顶部居中悬浮四个按钮：

- **原型**：默认状态，可正常点击原型的所有交互
- **添加批注**：进入批注模式，原型交互禁用；鼠标移动时高亮预览当前悬停的控件；选择控件并单击后，小红点记录相对于该控件的位置（比例坐标），同时自动弹出批注编辑弹框；支持拖拽小红点微调位置；保存后保持在添加批注模式，可继续添加下一条
- **查看批注**：显示所有已添加的小红点；原型交互正常可用；单击小圆点时，批注弹框紧贴圆点显示，同时自动定位并高亮目标元素；点击批注列表项时，目标元素在原本位置出现并高亮
- **保存到文件**：将批注数据以内嵌 `<script id="anno-data">` 标签形式写入 HTML 文件，实现批注与原型文件的持久化绑定

#### 2. 批注编辑弹框

- 弹框支持拖拽放大/缩小，最大可放大至屏幕 80%；标题栏提供最大化/还原按钮
- 批注内容采用富文本编辑器（contenteditable），直接支持混排文字、表格、图片、超链接
- 富文本工具栏支持：加粗、斜体、下划线、字体颜色（10 个预设色块）、插入表格、插入图片、插入链接
- 支持保存、编辑、删除
- 支持设置批注状态：待处理 / 已确认 / 已解决 / 驳回

#### 3. 批注管理面板

- 侧边栏可展开/收起，显示当前页面所有批注列表
- 支持点击列表项定位到对应圆点位置并高亮
- 支持按状态、关键词搜索筛选

#### 4. 数据持久化与共享

- 批注数据以 JSON 格式存储，结构包含：页面标识、目标元素选择器、相对位置比例、批注内容、状态、作者、时间戳
- 添加/编辑/删除批注时，数据自动保存到 localStorage（作为运行时缓存）
- 持久化到文件需主动点击"保存到文件"按钮：通过 File System Access API 直接写回原 HTML 文件
- 首次保存需选择原文件并授权写入权限，之后可一键保存
- 浏览器不支持 File System Access API 时回退为下载同名 HTML 文件
- 保存后的 HTML 文件发送给他人时，批注自动包含在内，换浏览器/换电脑均可查看
- 图片资源自动转为 Base64 内嵌，确保离线可用

#### 5. 快捷键

| 快捷键 | 功能 |
|--------|------|
| `Ctrl/Cmd + Shift + A` | 快速进入添加批注模式 |
| `Ctrl/Cmd + S` | 保存批注到文件 |
| `Esc` | 退出批注模式或关闭弹框 |
| `Delete` | 删除当前选中的批注点 |

### 技术特性

- **元素选择器唯一性**：采用 `tag.className:nth-of-type(idx)` 格式逐级生成选择器，确保在同级存在多个相同类名元素时仍能唯一匹配，避免圆点错位
- **动态面板适配**：支持动态面板（Tab 切换、弹框等）各状态下的批注独立显示
- **失效状态处理**：若批注绑定的目标元素被删除或结构变更，小红点显示为灰色带问号标识，批注内容仍可查看
- **运行时元素清理**：保存文件时自动清理 Echarts canvas、批注标记容器、弹框状态，避免下次加载时出现重复元素

### 适用场景

- 产品经理对设计稿/原型进行评审批注
- 设计师对交互原型进行修改建议标注
- 开发人员对原型实现进行技术评估标注
- 团队协作中原型需求的沟通与交付

### 浏览器兼容性

- **完整功能**：Chrome、Edge 等 Chromium 内核浏览器（支持 File System Access API）
- **基本功能**：Firefox、Safari 等（保存功能回退为下载文件方式）

---

## English Introduction

### Overview

**Prototype Annotator** is a Skill tool designed for product managers and designers to annotate HTML prototype pages. No additional software installation is required. Once the Skill is introduced into the project, a floating control bar appears at the top of the prototype page, supporting adding, viewing, editing, and deleting annotations, and writing annotation data directly into the HTML file for persistent binding between annotations and prototypes.

### Core Features

#### 1. Floating Control Bar & Mode Switching

Four buttons are suspended at the top center of the prototype page:

- **Prototype**: Default state, all prototype interactions are available
- **Add Annotation**: Enter annotation mode, prototype interactions are disabled; hovering over an element highlights it with a preview; clicking an element records the position (relative coordinates) with a red dot and automatically pops up the annotation editor; supports dragging the red dot for fine-tuning; stays in annotation mode after saving to continue adding
- **View Annotations**: Display all added red dots; prototype interactions remain available; clicking a red dot shows the annotation popover next to it and highlights the target element; clicking a list item locates the target element at its original position
- **Save to File**: Writes annotation data into the HTML file as an embedded `<script id="anno-data">` tag, achieving persistent binding between annotations and the prototype file

#### 2. Annotation Editor

- The dialog supports drag-to-resize, up to 80% of the screen; the title bar provides a maximize/restore button
- The annotation content uses a rich text editor (contenteditable), directly supporting mixed text, tables, images, and hyperlinks
- The rich text toolbar supports: bold, italic, underline, font color (10 preset color blocks), insert table, insert image, insert link
- Supports save, edit, and delete
- Supports setting annotation status: Pending / Confirmed / Resolved / Rejected

#### 3. Annotation Management Panel

- The sidebar can be expanded/collapsed, displaying all annotations for the current page
- Supports clicking list items to locate and highlight the corresponding red dot
- Supports filtering by status and keyword search

#### 4. Data Persistence & Sharing

- Annotation data is stored in JSON format, including: page identifier, target element selector, relative position ratio, annotation content, status, author, timestamp
- When adding/editing/deleting annotations, data is automatically saved to localStorage (as runtime cache)
- Persisting to a file requires actively clicking the "Save to File" button: writes back to the original HTML file via the File System Access API
- The first save requires selecting the original file and granting write permission; subsequent saves can be done with one click
- Falls back to downloading a same-named HTML file when the browser does not support the File System Access API
- When the saved HTML file is sent to others, annotations are automatically included, viewable across browsers and devices
- Image resources are automatically converted to Base64 for offline availability

#### 5. Keyboard Shortcuts

| Shortcut | Function |
|----------|----------|
| `Ctrl/Cmd + Shift + A` | Quickly enter annotation mode |
| `Ctrl/Cmd + S` | Save annotations to file |
| `Esc` | Exit annotation mode or close dialog |
| `Delete` | Delete the currently selected annotation |

### Technical Features

- **Selector Uniqueness**: Selectors are generated level by level in the `tag.className:nth-of-type(idx)` format, ensuring unique matching even when multiple elements with the same class name exist as siblings, avoiding dot misalignment
- **Dynamic Panel Support**: Supports independent display of annotations across dynamic panel states (Tab switching, dialogs, etc.)
- **Invalid State Handling**: If the target element bound to an annotation is deleted or structurally changed, the red dot appears gray with a question mark, and the annotation content remains viewable
- **Runtime Element Cleanup**: When saving the file, automatically cleans up Echarts canvas, annotation marker containers, and dialog states to avoid duplicate elements on next load

### Use Cases

- Product managers annotating design drafts/prototypes for review
- Designers marking modification suggestions on interactive prototypes
- Developers annotating technical assessments of prototype implementations
- Team collaboration for prototype requirement communication and delivery

### Browser Compatibility

- **Full Features**: Chrome, Edge, and other Chromium-based browsers (supporting File System Access API)
- **Basic Features**: Firefox, Safari, etc. (save function falls back to file download)

---

## 安装与使用 / Installation & Usage

### 安装 / Installation

将 `prototype-annotator` 文件夹放到项目的 `.trae/skills/` 目录下：

Place the `prototype-annotator` folder in the project's `.trae/skills/` directory:

```
your-project/
└── .trae/
    └── skills/
        └── prototype-annotator/
            ├── SKILL.md
            └── README.md
```

### 使用 / Usage

1. 在 TRAE 中打开 HTML 原型页面
2. 调用"原型批注" Skill
3. 使用顶部悬浮工具栏进行批注操作
4. 点击"保存到文件"将批注持久化到 HTML 文件

---

## 文件结构 / File Structure

| 文件 / File | 说明 / Description |
|-------------|-------------------|
| `SKILL.md` | Skill 定义文件，包含完整的功能规范 / Skill definition file with complete feature specifications |
| `README.md` | 说明文档（本文件）/ Documentation (this file) |

---

## 许可证 / License

MIT License
