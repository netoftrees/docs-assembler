# Changelog

All notable changes to the Docs Assembler extension are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).



### Upcoming
- Video tutorials (intro video)
- Help files
- Move or copy a section of a branch
- Make section of branch a new map
- GitLab Pages integration
- Port from database version
  - Projects
  - Search
  - Impact
- Light theme



## [0.9.44] - 2026-07-24

### Added
- **Variable Reference Commenting** - in `.tsstp` Markdown files, prefixing a variable reference with `¬` (e.g., `¬{{::alias.variableName}}`) now comments out the reference.


## [0.9.43] - 2026-06-21

### Changed
- **Control Panel** - edited help text.



## [0.9.42] - 2026-06-18

### Changed
- **Webview Heading Line Height** - adjusted CSS line height for headings in webview editors for improved readability.



## [0.9.41] - 2026-06-12

### Changed
- **README Structure** - minor reordering of sections for improved readability.

### Fixed
- **Shared Step Filepath** - corrected an issue where the path of a shared step was blank in the lens.



## [0.9.40] - 2026-06-11

### Changed
- **README Structure** - minor reordering of sections for improved readability.



## [0.9.39] - 2026-06-10

### Changed
- **README Structure** - minor reordering of sections for improved readability.



## [0.9.38] - 2026-06-10

### Changed
- **README Structure** - minor reordering of sections for improved readability.



## [0.9.37] - 2026-06-10

### Changed
- **README Video** - replaced walkthrough with new "Referenced, Not Copied" demo video; added JSDelivr 4K fallback for China accessibility.



## [0.9.36] - 2026-05-30

### Added
- **Initialisation progress** - added progress information message.



## [0.9.35] - 2026-05-25

### Fixed
- **Progress Dialogue Cancellation State** - added `_isCancelled` flag so `isCancelled()` remains accurate after the dialog is cleaned up.



## [0.9.34] - 2026-05-24

### Added
- **Community Health Files** - added `SECURITY.md`, `SUPPORT.md`, and `FAQ.md` to improve contributor and user onboarding.

### Fixed
- **Webview View Asset Path** - corrected asset path to resolve missing images.



## [0.9.33] - 2026-05-23

### Fixed
- **Version Comparison Type Error** - corrected utility to use the `.version` string from `semver.coerce` instead of passing the `SemVer` object directly.



## [0.9.32] - 2026-05-22

### Added
- **Automatic Update Notifications** - checks `updates.json` on activation to compare installed extension and template resource versions against the distribution repository, surfacing a control view with actions to update the extension or refresh resources when either is outdated.



## [0.9.26] - 2026-05-19

### Added
- **Remote Guide Loading** - added proxy pattern documentation to the Display Tool Contract for cross-origin fragments and asset proxying.
- **Cross-Origin Note** - updated README.md (English and Chinese) to clarify remote map references across repos and domains.

### Security
- **Proxy Guidelines** - documented allowlist validation, path validation, rate limiting, and Content-Type preservation for proxy implementations.



## [0.9.24] - 2026-05-17

### Fixed
- **Duplicated menu item** - `New step file` appeared twice in the Explorer context menu .



## [0.9.22] - 2026-05-16

### Changed
- **README Reorganisation** - minor restructuring of some sections for improved readability.

### Fixed
- **Compiler View Icons** - fixed missing file-type icons in the compiler view caused by a font file not loading.



## [0.9.20] - 2026-05-15

### Changed
- **README Reorganisation** - restructured sections for improved readability.
- **README Section Rewrite** - rewrote one section for clarity.

### Fixed
- **README Layout Inconsistencies** - corrected formatting, heading levels, and spacing irregularities throughout the README.



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
