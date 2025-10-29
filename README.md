# Codocation

> **Note:** This repository does not contain source code.  
> It serves as a **[public issue tracker](https://github.com/codocation/codocation/issues)** and **[release hub](https://github.com/codocation/codocation/releases)** for Codocation IDE.

<p align="center">
  <a href="https://github.com/codocation/codocation/releases/latest"><img src="https://img.shields.io/github/v/release/codocation/codocation?style=flat-square&label=version" alt="Latest Release"></a>
  <img src="https://img.shields.io/badge/platform-macOS%20%7C%20Windows%20%7C%20Linux-blue?style=flat-square" alt="Platforms">
  <a href="https://github.com/codocation/codocation/issues"><img src="https://img.shields.io/github/issues/codocation/codocation?style=flat-square" alt="Issues"></a>
</p>

Professional tool for creating and editing technical documentation. Standalone IDE built on IntelliJ Platform. Pure JVM architecture with **Flexmark + Prism.js**.

**Benefits:**
- **100% Cross-platform** — runs on macOS, Windows, Linux with no native dependencies
- **Zero Setup** — everything is bundled, no additional tools required
- **Single Source of Truth** — identical rendering in IDE, website, and PDF
- **Real-time Preview** — instant updates as you type (~300ms)
- **Syntax Highlighting** — Prism.js for all major languages

---

## Live Preview

Real-time Markdown rendering alongside your editor.

- **Bidirectional Scroll Sync** — scroll in editor → preview follows, and vice versa
- **Documentation Hot Reload** — preview updates instantly as you edit
- **Pin Mode** — freeze preview on one file while editing another
- **Diagrams** — Mermaid support rendered client-side
- **Math** — KaTeX for LaTeX formulas

---

## Table of Contents

Visual structure editor for your documentation.

- **Drag & Drop** — reorganize topics and groups intuitively
- **Multi-level Nesting** — unlimited hierarchy depth
- **Smart Drop Zones** — visual feedback shows exactly where items will land
- **Undo/Redo** — full support for all TOC operations, even when file isn't open
- **H1 Fallback** — automatically uses first heading from file if no title specified

---

## Multi-Module Support

Work with multiple documentation projects simultaneously.

- **Auto-discovery** — automatically finds all `codocation.xml` configs in workspace
- **Module Selector** — switch between modules from the toolbar
- **Independent Instances** — each module has its own TOC, locales, and settings

---

## Localization

Built-in multi-language documentation support.

- **Single Structure** — one `tree.xml` for all languages
- **File Naming Convention** — `overview.md`, `overview.ru.md`, `overview.de.md`
- **Configurable Fallback** — per-locale control over fallback behavior
- **Locale Switcher** — preview any language instantly
- **Partial Translations** — gracefully handle incomplete translations

---

## PDF Export

Professional PDF generation with full control.

- **Clickable TOC** — table of contents with page numbers
- **Page Numbering** — "Page X of Y" format, configurable position
- **Orientation** — portrait or landscape per document
- **Group Headers** — optional section headers in output
- **Embedded Images** — all images included in PDF
- **Unicode Support** — full Cyrillic, CJK, and special characters

---

## Static Site Generation

One-click documentation website.

- **ZIP Export** — ready to deploy anywhere
- **Template System** — customizable HTML/CSS/JS
- **Asset Bundling** — all resources included
- **BASE_PATH Support** — works in subdirectories

---

## Standalone CLI

**Fat JAR** for PDF and HTML generation without IDE. Perfect for CI/CD pipelines.

---

## Navigation Tool Window

Two-tab interface for documentation navigation.

**TOC Tab:**
- Visual tree of your documentation structure
- Single-click for preview, double-click to open
- Context menu for quick actions
- Preview Tab mode — navigate without cluttering editor tabs

**Sources Tab:**
- File tree filtered to documentation content
- Copy/paste support
- Context menu with New File, Delete, etc.

---

## Editor Integration

IntelliJ-grade editing experience for documentation.

- **Editor-only Mode** — no built-in Markdown split view, use Codocation Preview instead
- **Code Extensions Annotator** — syntax highlighting for fenced code blocks
- **MDX Support** — JSX in Markdown
- **AsciiDoc Support** — additional markup format

No config files to create manually. No terminal commands. No YAML to edit.

The IDE guides you through setup with a wizard. Your docs structure is visualized in the TOC panel — drag and drop to reorganize.

---

## Feedback & Issues

Found a bug? Have a feature idea? This is the place.

**Before opening an issue:**
- Check [existing issues](https://github.com/codocation/codocation/issues) — it might already be reported
- Try the [latest release](https://github.com/codocation/codocation/releases) — it might be fixed

**Feature requests welcome.** Describe the problem you're solving, not just the solution.

→ [Report a Bug](https://github.com/codocation/codocation/issues/new?template=bug_report.md)  
→ [Request a Feature](https://github.com/codocation/codocation/issues/new?template=feature_request.md)  
→ [Ask a Question](https://github.com/codocation/codocation/discussions)

---

## Download

→ [All Releases](https://github.com/codocation/codocation/releases)
