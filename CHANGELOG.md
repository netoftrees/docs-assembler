# Changelog

All notable changes to the Docs Assembler extension are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Video tutorials (intro video)
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



## [0.9.18] - 2026-05-14

### Added
- **Display Tool Contract & Localization** - added comprehensive README guidance on translation and multi-language support for display-tool and renderer authors.

### Changed
- **Stability Messaging** - updated the control-view notice from early-release caution to: "Docs Assembler is stable for daily use. We recommend testing thoroughly before deploying mission-critical documentation."

### Fixed
- **Repository Link** - replaced broken Gitee link with the correct GitHub URL in README.md.



## [0.9.16] - 2026-05-13

### Added
- **Post‑initialisation Feedback** - `末_FEEDBACK.md` is now fetched from the docs-assembler-template distribution and displayed after a repository is initialised.

### Fixed
- **Resource Download Fallback** - if the latest resource package is not yet cached on jsDelivr, the downloader now automatically retrieves the previous version instead of failing.



## [0.9.12] - 2026-05-06

### Added

#### Editor & Language Support
- **Map JSON Editor** - dedicated editor for `.tsmap` files with structured editing.
- **Diff Map JSON Editor** - side-by-side diff view for map JSON changes.
- **Maps Explorer** - tree view for browsing and navigating maps within the workspace.
- **Markdown IntelliSense, Diagnostics & Grammars** - full language support for step and variable references in Markdown, including TextMate and semantic token highlighting.
- **JSON Step (Video Step) Support** - IntelliSense, diagnostics, TextMate and semantic token support for JSON-based video steps.
- **Map Hyperlinks** - clickable navigation between maps and referenced content.
- **Helicopter View** - high-level overview of map structure and relationships.
- **Keyboard Shortcuts** - shortcut keys for common map and publish operations.

#### Maps & Document Structure
- **Map Folders** - organize maps into folder hierarchies.
- **Publish for Referenced and Nested Maps** - full support for publishing maps that reference or nest other maps.
- **Publish for Ancillaries** - support for ancillary (expandable/collapsible) step sections during publish.
- **Map and Guide Stacks** - visual indicators showing whether a map or guide is published, modified, or never published.
- **Reference Guides in Remote Banks** - ability to reference and pull guides from remote repositories.

#### Publishing & Integration
- **Publish Listed Maps** - selective publishing of maps marked for release.
- **GitHub Pages Integration** - direct publishing to GitHub Pages with Jekyll Markdown output.
- **Updated GitHub Pages Demo** - new demo site showcasing referenced and nested map examples with ancillaries.
- **Repo Initialisation, Resources Update and Migration** - automated setup and migration tools for new and existing repositories.

#### Refactoring & Asset Management
- **Relative URL Correction** - moving or copying map folders automatically corrects relative URLs.
- **Asset Reference Updates** - moving, cloning, or copy-pasting map folders adjusts all relative URLs.
- **Asset Rename Propagation** - moving or renaming map asset files automatically updates references and map hyperlinks.

---

*Note: Earlier patch releases between the initial launch and 0.9.12 are not individually documented here; their changes have been rolled into the 0.9.12 release notes above.*
