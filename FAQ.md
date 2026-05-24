# Frequently Asked Questions

## General

### What is Docs Assembler?

Docs Assembler is a free VS Code extension that lets you build documentation using modular, reusable components - like **code classes for your content**. Instead of writing long linear documents that duplicate information across guides, you create small **maps** (`.tsmap` JSON files) that can call each other, nest inside one another, and adapt to the reader's situation. It publishes to plain Markdown, so there is zero lock-in.

### Who is it for?

Teams who maintain complex, branching, or multi-path documentation - especially where content is reused across different guides, audiences, or contexts. It was born from conversations with [HAL Robotics](https://hal-robotics.com), but applies to any domain: software docs, troubleshooting guides, operational procedures, training materials, and more.

### Is it free?

Yes. The VS Code extension is free and open source under the MIT License. We also offer a paid [Docs Rescue service](#what-is-docs-rescue) for teams who want us to build their first project for them.

### Does it collect any data?

**No.** Docs Assembler collects no analytics, usage data, or crash reports. We cannot see your downloads, location, or how you use the tool. The only way we know you're using it is if you tell us - by starring the repo, opening an issue, or sending an email.

---

## Getting Started

### How do I install it?

**From VS Code Marketplace:**
1. Open the Extensions panel (`Cmd+Shift+X` / `Ctrl+Shift+X`)
2. Search "Docs Assembler"
3. Click Install

**Manual install (if no marketplace access):**
1. Download the latest `.vsix` from [GitHub Releases](https://github.com/netoftrees/docs-assembler/releases/latest)
2. In the Extensions panel, click the three dots → `Install from VSIX...`
3. Select the downloaded file

### Where do I start?

1. **Watch the walkthrough** linked in the README - it shows the core concepts in under 10 minutes
2. **Explore the [Live Demo](https://netoftrees.com/docs-assembler-demo/)** - see a real published guide
3. **Clone the [Template Repository](https://github.com/netoftrees/docs-assembler-template/)** - it has everything you need to develop, test, and preview locally
4. **Read the [Demo Repository](https://github.com/netoftrees/docs-assembler-demo/)** - see how the demo maps are structured

### Do I need to know Jekyll or GitHub Pages?

No. Docs Assembler publishes to standard Markdown. If you want to use GitHub Pages, the template repo includes Jekyll setup. If you use another static site generator (Hugo, Docusaurus, MkDocs, etc.), the Markdown output works there too.

---

## Concepts

### What is a map?

A **map** (`.tsmap` file) is a self-contained documentation module - like a **class** in software. It contains **steps** that link to Markdown files, and can reference other maps. Maps can be nested, reused, and composed into larger guides.

### What is a step?

A **step** is a unit of documentation within a map. Each step links to a Markdown file. Multiple steps can share the same Markdown file. Steps can have **ptions** that branch to other steps or maps, creating decision trees.

### What are variables?

**Variables** let you define reusable text snippets (product names, URLs, common phrases) in one place and reference them everywhere. Change the variable once, and every guide that uses it updates instantly. Variables live in `.tsvar` files and support IntelliSense and validation.

### What are ancillaries?

**Ancillaries** are expandable detail sections attached to a step. They let you hide complexity while keeping it accessible - readers who need detail can expand it; those who don't, won't be overwhelmed. Ancillaries can be nested.

### What is the "helicopter view"?

A high-level overview of your map structure and relationships - useful for understanding how maps connect and where content is reused.

---

## Publishing & Output

### What does "publish" mean?

Publishing assembles your maps into clean Markdown or HTML files. It:
- Validates all references (no broken links, no circular dependencies)
- Expands all variables
- Copies referenced assets to the output folder
- Produces standard Markdown ready for any static site generator

### Where does the output go?

To a `pub/` folder in your repo. From there, you can move files to `docs/` in the compare view with a button click, (for GitHub Pages) or wherever your site generator expects them.

### Can I publish to something other than GitHub Pages?

Yes. The output is plain Markdown. Use it with any static site generator or hosting platform.

### What is the "compare view"?

It shows differences between your published output and the live `docs/` folder, so you can review changes before making them public.

---

## Lock-in & Ownership

### What happens if I stop using Docs Assembler?

**Nothing bad.** Your content is plain Markdown and JSON in your Git repo. Uninstall the extension and your docs still work in any editor and any static site generator. There is no proprietary format, no subscription, no platform dependency.

### Can I edit files without the extension?

Yes. Maps are JSON files; steps are Markdown files. Any text editor works. The extension adds IntelliSense, validation, and visual editors - but your content is never trapped inside it.

### Do I need a special server or backend?

No. Docs Assembler runs entirely in VS Code. Publishing is local. GitHub Pages (or any static host) serves the output. There is no SaaS platform, no account required, no API keys.

---

## Localisation & Translation

### Does Docs Assembler translate my content?

**No.** Docs Assembler is an authoring tool. It produces source content in whatever language you write in. Translation and grammatical adaptation happen at the **display layer** - when a reader views your published guide through a display tool.

### How does localisation work for high-inflection languages?

For languages like Russian, Arabic, or Polish where grammar changes with context, display tools use **full-path context adaptation**. The display tool sends the entire narrative history to a high-quality LLM (like DeepSeek), which adapts each fragment to match the grammatical context. See the [Display Tool Contract](./DISPLAY_TOOLS.md) and [Predictive Pre-fetching White Paper](./PREDICTIVE_PREFETCH_WHITEPAPER.md) for the full architecture.

### Can I lock specific translations?

Yes - coming soon. Display tools will be able to use locale-specific `.tsfrg` files (e.g., `ru/onboarding.tsfrg`) to override AI translation with human-verified text for critical fragments.

---

## Troubleshooting

### The extension isn't showing up in VS Code

- Ensure you installed the `.vsix` correctly (Extensions panel → three dots → Install from VSIX)
- Check that the file downloaded fully - `.vsix` files are ~3.4 MB
- Try reloading the VS Code window (`Cmd+Shift+P` → `Developer: Reload Window`)

### After updating the extension, things look wrong

Clear the VS Code editor history:
1. `Cmd+Shift+P` (or `Ctrl+Shift+P`)
2. Type: `Clear Editor History`
3. Press Enter

---

## Roadmap & Contributing

### What's coming next?

See the [CHANGELOG](./CHANGELOG.md) for released features and the **Upcoming** section for what's planned. Current priorities include:
- Video tutorials
- Lock specific translations
- GitLab Pages integration
- Light theme
- Port of database-version features (Projects, Search, Impact)

### How can I contribute?

- **Star the repo** - it helps others find us
- **Open issues** - bugs, feature requests, or unclear documentation
- **Start discussions** - share your use case or ask questions
- **Build a display tool** - see the [Display Tool Contract](./DISPLAY_TOOLS.md) and open an issue to align on boundaries first

### What is "Docs Rescue"?

A fixed-price service where we audit your existing documentation, rebuild your most critical guide as a Docs Assembler project, and hand you a fully working Git repo. You own everything and edit it yourself going forward. [Email us](mailto:team@netoftrees.com) for details. Open-source maintainers of non-commercial projects get adjusted rates.

---

## Still have questions?

- [Open a discussion](https://github.com/netoftrees/docs-assembler/discussions) - the team and community monitor these
- [Email us](mailto:team@netoftrees.com) - we read everything, typically respond within 2 business days
