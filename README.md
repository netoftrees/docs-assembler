# Docs Assembler - Treat Documentation Like Code

[中文文档](#中文文档) | [Chinese Documentation](#中文文档)

This is an experimental port from a c# server/database application - to a GitHub repo/vscode extension. A concept driven by transformational conversations with a robotics firm - [HAL Robotics](https://hal-robotics.com).  

# Documentation Assembler

### We think in webs, but we're forced to write in lines

Human knowledge branches, loops, and layers - a living web of what-ifs and dependencies. Traditional documents force this into linear prose, losing the very structure that makes expertise usable.    
Docs Assembler bridges that gap. It is a way to give form to the networks in your mind - for others to follow and learn from your thinking, or extend and refine it with their own.

 > **To truly understand how it works, we encourage you to explore the [Live Demo](https://docsassemblerdemo.netoftrees.com/).**  
 >It was built with this version of the extension, which we've released for hands-on experimentation.  
 >***Please note: This extension is an early release with known limitations. We advise caution for critical work until the production-ready version is released later this year.***

### *Solve documentation hell,*

- **Sprawling, duplicated content** across guides, manuals, and wikis.
- **Bug-prone updates** make an edit in one place, miss it in ten others.
- **Brittle, unmanageable docs** that can't handle complex, branching scenarios.

### *by applying the principles of software engineering to the text,*

Docs Assembler is a VS Code extension that lets you build documentation systems with **modular, reusable components**. Think of it like **classes for your content**.

- **Maps (.tsmap files)**: Self-contained documentation modules that can be nested and composed, just like classes. Encapsulate text, or decision trees.
- **Variables**: Define reusable text snippets. Change one, update everywhere.
- **Inheritance & Composition**: Build complex guides from simple, reusable blocks. A change in a base propagates to all guides that use it.
- **Compile to Docs**: Assemble these components on-the-fly into flawless, context-aware Markdown or HTML.

### *and maintain a single source of truth.*



## Designed for Developers, by Developers

The design of Docs Assembler was driven by a fundamental need from teams like [HAL Robotics](https://hal-robotics.com): to have a powerful system that *respects developer workflows and sovereignty*. This isn't just a platform; it's a philosophy built on core principles that will feel immediately right:

*   **Familiarity & Control:** The entire system is built on the tools you already know and trust. Your documentation lives in Git repos, right alongside your code. Content is written in Markdown files, editable in any editor. Structure is defined in JSON files you can view and edit manually.
*   **Absolute Ownership:** You have physical possession of your documentation. It's your Markdown and JSON in your repo. You are never trapped in a subscription or held ransom by a SaaS platform.
*   **Zero Lock-In:** This is a critical feature. There are no consequences if you stop using Docs Assembler. Since it publishes to standard Markdown, you can uninstall the extension and your documentation is still perfectly usable, editable, and ready for any other static site generator. Your content is always yours.
*   **Seamless Publishing:** It's designed to publish directly to [GitHub Pages](https://docs.github.com/en/pages), a platform developers already understand. Even Liquid scripts embedded in your Markdown work as expected.
*   **Built to Scale:** Like classes break down massive software systems, Docs Assembler's modules are designed to decompose enormous documentation sets into manageable, distributable units that different teams can own.
*   **Engineering Rigour:** It brings true software tooling to docs, with IntelliSense for variables, full validation before publish, and automatic handling of relative URLs that are defined as variables. This ensures any local files referenced by these URL variables are automatically discovered and copied to the publish folder, preventing broken links and ensuring robustness.



## Live Demo

- **Live Demo GitHub Pages Site**: [See it in action](https://docsassemblerdemo.netoftrees.com/)  
    - Publish currently targets GitHub Pages, producing Jekyll Markdown.  
    - After a Publish run a Git Commit and Push for GitHub Pages to make changes live. 

- **GitHub Pages Repository**: [Explore the repo](https://github.com/CompositeFlows/DocsAssemblerDemo/)  
    - The sample maps are located in the `/tsmaps/` folder and published guides in `/docs/`.  

- **Frontend Renderer**: [TypeScript Source Code](https://github.com/CompositeFlows/fragment-renderer)  
    - Client-side application that renders the published content in the live demo.

- **Sandbox Repository**: [Sandbox repo](https://github.com/CompositeFlows/DocsAssemblerDemoSandbox/)
    - All-in-one repository with docs, FragmentRenderer, and Jekyll setup
    - Supports full local workflow: assemble docs → publish → Jekyll build → local preview
    - Everything needed to develop, test, and preview documentation changes locally
    - Instructions on running repo locally with Jekyll coming soon.



## Contact us
Email us with bugs or feedback. We don't collect analytics or track usage. We only know you're using Docs Assembler if you tell us.
[team@netoftrees.com](mailto:team@netoftrees.com)



### After an extension update clear vscode history:  
- Open the Command Palette: _Cmd+Shift+P_  
- Type: _Clear Editor History_



## Quick walkthrough
Using a fork of the HAL Robotics documentation repo, initialised to use maps - [HAL.Documentation.maps](https://github.com/CompositeFlows/HAL.Documentation.maps).   
It serves as a good example for simple use cases - as there are only shared variables and steps, no linked maps. 
#### BE AWARE: This is not the current version of HAL Robotics documentation - [see below](#for-up-to-date-information-on-hal-robotics)

[<img src="./assets/Walkthrough-thumbnail.png">](https://vimeo.com/1013352380?share=copy#t=0)


### Maps
- The main building block is a **map**.
- It is a **json** file with a **.tsmap** extension.
- Functions similarly to a **class** in software.
- [Maps are built to scale up easily](#built-to-handle-both-complexity-and-scale)


#### Switching between **Map Editor** and **Map Json Editor**:

![Docs Assembler map json editor gif](./assets/DocsAssemblerJsonDec24.gif)



#### Switching between **Maps Diff** and **Maps Json Diff**:

![Docs Assembler diff map json gif](./assets/DocsAssemblerDiff.gif)



### Steps
- A **map** is a section of documentation divided into **steps**.
- Each **step** links to a **markdown file** with the step's documentation text.
- **Markdown file** can be shared between multiple **steps**.
- **Markdown files** are editable with the **Visual Studio Code** markdown editor.

![Docs Assembler steps gif](./assets/DocsAssemblerSteps.gif)



### Maps can reference other maps
- A **map** referenced within another **map**, appears as a single **step**.
- If a referenced **map** has **exits**, other **maps** or **steps** will need chained onto those **exits**.
- **Validation** prevents circular references.

![Docs Assembler charts gif](./assets/DocsAssemblerCharts.gif)



### Variables
- **Variables** define reusable **markdown text**.
- **Variables** that define relative links are adjusted to be always be valid for the published document they are used in.

![Docs Assembler variables gif](./assets/DocsAssemblerVariables.gif)



### Variables can reference other variables
- A **variable's** markdown text can reference other **variables**.
- **Validation** prevents circular references.

![Docs Assembler variables nested gif](./assets/DocsAssemblerNestedVariables.gif)



### Compile to docs
On **publish**, the **Docs Assembler** reads the **maps** selected for **publish** - it **validates** and assembles all referenced **maps**, **markdown files**, expands any **variables**, copies over referenced **assets**, and **compiles** the resuls into **markdown** or **html** files to the **publish** folder in your **repo**. 

![Docs Assembler publish gif](./assets/DocsAssemblerPublish.gif)



### Compare published to live
Use the **compare view** to view changes between published files and the **docs** folder files.

![Docs Assembler compare gif](./assets/DocsAssemblerCompare.gif)



### Move published to live
If the changes are as expected, click-move the published files to the **docs** folder. If you use **GitHub Pages**,  **docs** would be the root folder.

![Docs Assembler live gif](./assets/DocsAssemblerLive.gif)



### Built to handle both complexity and scale
- At its simplest, a **map** has a single **step** and **markdown file**.
- A slightly more complex **map** would be a single pathway of **steps**, like a book or manual.
- At its most complicated, a **map** is a **decision tree** of **steps**, many pointing to other **maps**, which in turn point to other **maps** etc. The expanded result could be enormous, and impossible to buildor maintain without breaking it down into manageable, discrete, reusable, units. Just like we do in code with **classes**.


## Example of published output


![netoftrees C# server/database applications gif](./assets/netoftreesCsharp.gif)


#### Image above
- This shows the c# server/database application, where **steps** are stored in a database.
    - It has all the **ancillaries** expanded. 
    - When all **ancillaries** are collapsed the **guide** shows enough information for an expert to complete the task. 
    - If a user expands an **ancillary**, they insert more **steps** on a topic. 
    - **Ancillaries** can be nested - so users can drill down.
    - With all possible **ancillaries** expanded, all the **steps** for completing a task, as a novice, are laid out.
- Reusing **maps** makes it straight forward to build and maintain **guides** that a user can tailor to their skill set.



#### Image below
- This shows the editor for the c# server/database application.
    - With the referenced maps, including nested ones, used to build the guide shown above.
    - Most will be reused in other guides.

![netoftrees C# server/database applications gif](./assets/netoftreesCsharpMaps.gif)



### Released
- 0.9.11
    - Map folders
    - Markdown Intellisense, diagnostics and TextMate and semantic grammars for steps and variables
    - Publish listed maps
    - Moving or copying map folder corrects relative urls
    - Map json editor
    - Diff map json editor
    - Maps explorer
    - Map hyper links
    - GitHub Pages integration
    - Publish for referenced and nested maps
    - Publish for ancillaries
    - Update GitHub Pages site with a new demo showing referenced and nested map example using ancillaries
    - Map and guide stacks - published, modified or never published.
    - Move, clone, copy + paste of map folders adjusts relative urls.
    - Move, rename of map asset files updates references and map hyperlinks.
    - Shortcut keys 
    - json step (video step) - json intellisense, diagnostics, textMate and semantic tokens
    - Reference guides in remote banks
    - Helicopter view
    - Repo initialisation, resources update and migration



### Next
- Video tutorials
    - Intro video
- Help files



### Upcoming
- Move or copy a section of a branch
- Make section of branch a new map
- GitLab Pages integration
- Port from database version
    - Projects
    - Search
    - Impact
- Light theme



### Notes

- ***Encapsulation*** - Wrapping a segment of the documentation within a single **map**.
- ***Inheritance*** - Deriving a new **map** from an existing **map** (parent).
- ***Polymorphism*** - Grouping **maps** as members of a common superclass (e.g., tiger, lion => cats).
- ***Abstraction*** - Hiding complex documentation details within a **map** and exposing that **map's interface** to other **maps** as a single **step**.
- ***Composition*** - Composing a **map** of one or more other **maps**.



### Links

[team@netoftrees.com](mailto:team@netoftrees.com)  
[www.netoftrees.com](https://www.netoftrees.com/)  
[x.com/docsassembler](https://x.com/docsassembler)  



### For up to date information on HAL Robotics:

documentation:
[docs.hal-robotics.com](https://docs.hal-robotics.com/)  
documentation repo:
[github.com/HALRobotics/HAL.Documentation](https://github.com/HALRobotics/HAL.Documentation)  
website:
[hal-robotics.com](https://hal-robotics.com/)

---

---

# 中文文档

[英文文档](#docs-assembler---treat-documentation-like-code) | [English Documentation](#docs-assembler---treat-documentation-like-code)

# Docs Assembler - 像写代码一样写文档

这是从 C# 服务端/数据库应用移植而来的实验性项目，转为 GitHub 仓库 + VS Code 扩展。概念源于与机器人公司 [HAL Robotics](https://hal-robotics.com) 的一次突破性交流。

# 文档组装器

### 我们以网状思考，却被逼着线性书写

人类知识会分支、循环、分层——一张由假设与依赖构成的活网。传统文档却强行塞进线性文字，丢掉了让知识真正可用的结构。  
Docs Assembler 填补了这个缺口。它让你头脑中的网络具象化——让他人能够沿着你的思路学习，或用他们自己的思考去扩展、完善你的思路。

> **想真正理解它如何运作，建议先探索[在线演示](https://docsassemblerdemo.netoftrees.com/)。**
> 该演示即由此版本扩展构建，现已开放体验。
> ***注意：本扩展为早期版本，存在已知限制。正式版将于今年晚些时候发布，在此之前请勿用于关键工作。***

### *终结文档地狱，*

- 内容四处扩散、重复——指南、手册、Wiki 里到处都是。
- 更新极易出错：改一处，漏十处。
- 文档脆弱、不可维护，无法应对复杂分支场景。

### *将软件工程原则应用于文本，*

Docs Assembler 是一个 VS Code 扩展，让你用**模块化、可复用组件**构建文档系统。可以理解为**内容的类**。

- **映射（.tsmap 文件）**：自包含的文档模块，可嵌套、可组合，就像类。封装文本或决策树。
- **变量**：定义可复用文本片段。改一处，全局更新。
- **继承与组合**：用简单可复用块搭建复杂指南。基类改动自动传播到所有引用处。
- **编译为文档**：实时组装组件，输出无误、上下文感知的 Markdown 或 HTML。

### *维护单一事实来源。*

## 为开发者设计，由开发者打造

Docs Assembler 的设计源于 [HAL Robotics](https://hal-robotics.com) 等团队的核心需求：一套强大、**尊重开发者工作流与主权**的系统。这不仅是一个平台，更是一套基于以下原则的哲学：

- **熟悉与掌控**：系统基于你已熟悉和信任的工具。文档与代码共存于 Git 仓库。内容用 Markdown 编写，任何编辑器均可编辑。结构用 JSON 定义，可手动查看和修改。
- **绝对所有权**：你实际持有自己的文档。它们是你仓库里的 Markdown 和 JSON。永远不会被订阅绑架，或被 SaaS 平台勒索。
- **零锁定**：这是关键特性。停止使用 Docs Assembler 毫无代价。因为它发布为标准 Markdown，直接卸载扩展即可——文档仍完全可用、可编辑，可直接用于任何其他静态站点生成器。你的内容永远属于你。
- **无缝发布**：设计上直接发布到 [GitHub Pages](https://docs.github.com/zh/pages)，开发者已熟悉的平台。Markdown 中嵌入的 Liquid 脚本也能正常工作。
- **为规模而生**：正如类将大型软件系统拆分为小块，Docs Assembler 的模块将海量文档集分解为可管理、可分发的小单元，不同团队可分别维护。
- **工程级严谨**：为文档带来真正的开发工具——变量有 IntelliSense，发布前完整校验，自动处理作为变量定义的相对 URL。这些 URL 引用的本地文件会被自动发现并复制到发布文件夹，防止链接失效，确保健壮性。

## 在线演示

- **GitHub Pages 演示站点**：[查看实际效果](https://docsassemblerdemo.netoftrees.com/)
    - 目前发布目标为 GitHub Pages，生成 Jekyll Markdown。
    - 发布后需执行 Git Commit 和 Push，GitHub Pages 才会生效。
- **GitHub Pages 仓库**：[探索仓库](https://github.com/CompositeFlows/DocsAssemblerDemo/)
    - 示例映射位于 `/tsmaps/` 文件夹，发布的指南在 `/docs/`。
- **前端渲染器**：[TypeScript 源码](https://github.com/CompositeFlows/fragment-renderer)
    - 客户端应用，用于在在线演示中渲染已发布内容。
- **沙盒仓库**：[沙盒仓库](https://github.com/CompositeFlows/DocsAssemblerDemoSandbox/)
    - 包含文档、FragmentRenderer 和 Jekyll 配置的一体化仓库。
    - 支持完整本地工作流：组装文档 → 发布 → Jekyll 构建 → 本地预览。
    - 开发、测试和预览文档变更所需的一切。
    - 本地使用 Jekyll 的说明即将发布。

## 联系我们

发现 Bug 或有反馈，请发邮件。我们不收集分析数据，不追踪使用情况。只有你主动告知，我们才知道你在使用 Docs Assembler。
[team@netoftrees.com](mailto:team@netoftrees.com)

### 扩展更新后请清除 VS Code 历史记录：
- 打开命令面板：_Cmd+Shift+P_
- 输入：_Clear Editor History_

## 快速入门

以 HAL Robotics 文档仓库的分支为例，已初始化为使用映射：[HAL.Documentation.maps](https://github.com/CompositeFlows/HAL.Documentation.maps)。
该示例适合展示简单用法——只有共享变量和步骤，没有级联映射。
**注意：这不是 HAL Robotics 当前版本的文档——详见下文[关于 HAL Robotics 的最新信息](#关于-hal-robotics-的最新信息)**

[<img src="./assets/Walkthrough-thumbnail.png">](https://vimeo.com/1013352380?share=copy#t=0)

### 映射
- 主要构建块是**映射**。
- 它是一个带 **.tsmap** 后缀的 **json** 文件。
- 功能类似软件中的**类**。
- [映射设计上易于扩展](#既能处理复杂也能应对规模)

#### 在**映射编辑器**与**映射 JSON 编辑器**之间切换：

![Docs Assembler map json editor gif](./assets/DocsAssemblerJsonDec24.gif)

#### 在**映射差异视图**与**映射 JSON 差异视图**之间切换：

![Docs Assembler diff map json gif](./assets/DocsAssemblerDiff.gif)

### 步骤
- **映射**是由**步骤**组成的文档段落。
- 每个**步骤**指向一个包含该步骤文档文本的 **markdown 文件**。
- **markdown 文件**可被多个**步骤**复用。
- **markdown 文件**可用 **VS Code** 的 markdown 编辑器编辑。

![Docs Assembler steps gif](./assets/DocsAssemblerSteps.gif)

### 映射可引用其他映射
- 在另一**映射**中引用的**映射**，显示为单个**步骤**。
- 若被引用的**映射**有**出口（exits）**，需将其他**映射**或**步骤**链接到这些**出口**上。
- **校验**阻止循环引用。

![Docs Assembler charts gif](./assets/DocsAssemblerCharts.gif)

### 变量
- **变量**定义可复用的 **markdown 文本**。
- 定义相对链接的变量，在发布到目标文档时自动调整，确保链接有效。

![Docs Assembler variables gif](./assets/DocsAssemblerVariables.gif)

### 变量可引用其他变量
- **变量**的 markdown 文本可引用其他**变量**。
- **校验**阻止循环引用。

![Docs Assembler variables nested gif](./assets/DocsAssemblerNestedVariables.gif)

### 编译为文档
**发布**时，**Docs Assembler** 读取选中的**映射**，**校验**并组装所有引用的**映射**和 **markdown 文件**，展开变量，复制引用的资源文件，最终将结果**编译**为 **markdown** 或 **html** 文件，输出到仓库中的**发布**文件夹。

![Docs Assembler publish gif](./assets/DocsAssemblerPublish.gif)

### 对比已发布与线上版本
使用**对比视图**查看发布文件与 **docs** 文件夹中文件的差异。

![Docs Assembler compare gif](./assets/DocsAssemblerCompare.gif)

### 将发布内容移至线上
若变更符合预期，点击移动按钮，将发布文件移至 **docs** 文件夹。若使用 **GitHub Pages**，**docs** 即为根文件夹。

![Docs Assembler live gif](./assets/DocsAssemblerLive.gif)

### 既能处理复杂也能应对规模
- 最简单的**映射**：单个**步骤**和一个 **markdown 文件**。
- 稍复杂：单一路径的**步骤**序列，如一本书或手册。
- 最复杂：**映射**是一个**决策树**，多个**步骤**指向其他**映射**，这些映射又指向更多映射……展开后的结果极为庞大，若不拆分为可管理、离散、可复用的单元，几乎无法构建或维护。正如代码中用**类**做拆分。

## 发布输出示例

![netoftrees C# server/database applications gif](./assets/netoftreesCsharp.gif)

#### 上图说明
- 展示 C# 服务端/数据库应用，其中**步骤**存储在数据库中。
    - 所有**附件（ancillaries）**已展开。
    - 所有附件**折叠**时，**指南**呈现的信息足以让专家完成任务。
    - 用户展开**附件**后，会插入该主题的更多**步骤**。
    - **附件**可嵌套，用户可层层深入。
    - 所有可能的**附件**全部展开时，新手完成任务的**所有步骤**都会呈现。
- 复用**映射**可轻松构建和维护**指南**，用户可根据自身技能水平定制显示内容。

#### 下图说明
- 展示 C# 服务端/数据库应用的编辑器界面。
    - 显示构建上述指南所用的引用映射（含嵌套映射）。
    - 大多数映射也会在其他指南中复用。

![netoftrees C# server/database applications gif](./assets/netoftreesCsharpMaps.gif)

### 已发布版本
- 0.9.11
    - 映射文件夹
    - Markdown IntelliSense、诊断、TextMate 和语义语法高亮（针对步骤和变量）
    - 发布列出的映射
    - 移动或复制映射文件夹时自动矫正相对 URL
    - 映射 JSON 编辑器
    - 映射 JSON 差异编辑器
    - 映射资源管理器
    - 映射超链接
    - GitHub Pages 集成
    - 支持引用和嵌套映射的发布
    - 支持附件的发布
    - 更新 GitHub Pages 站点，新增展示引用和嵌套映射（含附件）的示例
    - 映射和指南分组：已发布、已修改、从未发布
    - 移动、克隆、复制粘贴映射文件夹时自动调整相对 URL
    - 移动、重命名映射资源文件时自动更新引用和映射超链接
    - 快捷键
    - json 步骤（视频步骤）—— json 智能提示、诊断、TextMate 和语义令牌
    - 引用远程仓库中的指南
    - 概览视图
    - 仓库初始化、资源更新和迁移

### 下一步
- 视频教程
    - 介绍视频
- 帮助文件

### 即将推出
- 移动或复制分支中的一段
- 将分支中的某一段独立为新映射
- GitLab Pages 集成
- 从数据库版本移植
    - 项目
    - 搜索
    - 影响分析
- 浅色主题

### 备注

- ***封装*** —— 将文档段落包装在单个**映射**中。
- ***继承*** —— 从现有**映射**（父级）派生新**映射**。
- ***多态*** —— 将不同**映射**归为同一超类成员（例如：虎、狮 => 猫科）。
- ***抽象*** —— 将复杂文档细节隐藏在**映射**内部，仅向其他**映射**暴露该**映射的接口**（作为单个**步骤**）。
- ***组合*** —— 用一个或多个其他**映射**构成**映射**。

### 链接

[team@netoftrees.com](mailto:team@netoftrees.com)  
[www.netoftrees.com](https://www.netoftrees.com/)  
[x.com/docsassembler](https://x.com/docsassembler)  

### 关于 HAL Robotics 的最新信息

文档：
[docs.hal-robotics.com](https://docs.hal-robotics.com/)  
文档仓库：
[github.com/HALRobotics/HAL.Documentation](https://github.com/HALRobotics/HAL.Documentation)  
官网：
[hal-robotics.com](https://hal-robotics.com/)  

