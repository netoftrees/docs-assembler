# Display Tool Contract

> **Document Status:** This contract describes the intended architecture and boundaries for Docs Assembler display tools. Some sections—particularly those marked 🚧 below—represent the planned roadmap and design direction rather than currently implemented features. They are included so that authors and contributors can build toward a consistent future state.

Docs Assembler intentionally keeps the authoring layer simple. Complexity that does not belong in a text editor - rendering, security, localization, search indexing, state management, and liability enforcement - is pushed to the display tool.

This document describes the boundaries and known challenges so display tool authors can build correctly.

## Localization & Multi-Language Display

Docs Assembler's authoring model is naturally suited to **analytic and low-inflection languages** (English, Chinese, etc.), where grammar does not create cross-fragment agreement chains. **Synthetic and high-inflection languages** (Russian, Arabic, Polish, German, Basque, etc.) require display-layer assembly because the same reusable sub-guide may need different gender, case, number, or tense depending on the parent path that calls it.

This applies in two scenarios:
- **Native authoring:** A Russian author writes guides in Russian that reuse sub-guides across different parent contexts.
- **Translation:** An English source guide is translated into Russian, Arabic, Polish, or French, where the target language requires grammatical features (case, gender, number, aspect) that English does not encode.

### The Assembly-First Pattern

Variables exist in maps and variable files. During publish, all variables, relative paths, and imports are resolved. Steps with no branching are merged until a branch occurs. The result is a simple JSON tree of Markdown fragments.

A static site generator (e.g., Jekyll) converts these Markdown fragments to HTML fragments. It is those HTML fragments that are passed over the wire to the reader's machine, where the display tool assembles them into the final view.

For high-inflection languages, display tools should assemble the full path from root to current step *before* rendering or translating. This gives the rendering engine (or AI translator) full anaphoric context and allows sub-guide reuse without requiring authors to embed grammar logic in their Markdown.

### Grammatical State Propagation

For high-inflection languages, the display tool must carry a small **grammatical state object** as it walks the tree. This state is typically 0.5–2 KB for technical documentation and rarely exceeds 5 KB even for complex narratives.

The state object should include:

```json
{
  "path": ["root","onboarding","step_3","sub_security","step_1"],
  "bindings": {
    "user": {"name":"Alice","gender":"feminine","number":"singular"},
    "role": {"name":"Administrator","gender":"feminine","number":"singular"},
    "device": {"name":"Monitor","gender":"masculine","animacy":"inanimate"}
  },
  "discourse": {
    "topic": "user",
    "formality": "formal",
    "mood": "imperative"
  },
  "glossary_hash": "a1b2c3d4"
}
```

**Key principles:**
- **Track only active referents.** The display tool should maintain the discourse stack deterministically - dropping referents that have not appeared in recent fragments. This is best implemented as deterministic code (e.g., an LRU cache of the last N mentioned entities), not delegated to an AI prompt. While you could architect a prompt that asks the AI to return updated state alongside adapted text, this introduces non-determinism: the AI may hallucinate, drop, or misgender referents. The state object should be owned by the display tool; the AI should consume it, not produce it.
- **Post state with each request.** If the display tool is a stateless web app, this object should be cached in `sessionStorage` and posted back to the server when retrieving the next fragment.
- **Compress over the wire.** JSON with repeated keys compresses to roughly 30 % of raw size; even complex narrative states stay well under typical HTTP header budgets.

### The Glossary Pattern

Display tools should accept a product glossary (e.g., `glossary.yaml`) that maps source terms to approved translations per locale. This is passed to the translation API to eliminate domain-specific ambiguity.

**Example `glossary.yaml`:**

```yaml
# glossary.yaml
# Product-specific terminology for Docs Assembler guides
# Each term maps to approved translations per locale

terms:
  - id: abort
    context: "Force quit or emergency stop of a process"
    translations:
      ru: "аварийно завершить"
      fr: "arrêt d'urgence"
      pl: "przerwanie awaryjne"
      ar: "إنهاء طارئ"

  - id: run_script
    context: "Execute a command or script"
    translations:
      ru: "выполнить"
      fr: "exécuter"
      pl: "uruchomić"
      ar: "تشغيل"

  - id: review
    context: "One-time verification step"
    translations:
      ru: "проверить"
      fr: "vérifier"
      pl: "sprawdzić"
      ar: "التحقق"

  - id: review_ongoing
    context: "Ongoing monitoring or repeated checking"
    translations:
      ru: "проверять"
      fr: "surveiller"
      pl: "monitorować"
      ar: "مراقبة"

  - id: user
    context: "Generic UI reference to a person"
    translations:
      ru: "пользователь"
      fr: "utilisateur"
      pl: "użytkownik"
      ar: "المستخدم"

  - id: user_account
    context: "The account entity, not the person"
    translations:
      ru: "учётная запись"
      fr: "compte"
      pl: "konto"
      ar: "الحساب"

  - id: settings
    context: "Application configuration"
    translations:
      ru: "настройки"
      fr: "paramètres"
      pl: "ustawienia"
      ar: "الإعدادات"

style_brief:
  ru: "Use formal imperative verbs ending in -те. Prefer perfective aspect for single actions. Use 'вы' (formal you) throughout."
  fr: "Use formal 'vous' for instructions. Prefer infinitive form for UI labels."
  pl: "Use formal 'Państwo' or second-person plural for instructions."
  ar: "Use formal Modern Standard Arabic. Avoid colloquial forms."
```

### Caching

Cache translations by a hash of the **template text** (unresolved Markdown), not the resolved string. Bust the cache only when the template or glossary changes.

### Authoring Guidelines (Optional but Recommended)

Authors can minimize residual risk by favoring **imperative voice** directed at the reader (*"Click the Save button"*) over third-person narration that refers to variables from parent maps. Imperatives avoid the cross-fragment agreement chains that make reuse difficult in high-inflection languages.

### Human Override 🚧

> **Planned:** This mechanism is not yet implemented. It is included in the contract to guide future display-tool development.

If a locale-specific `.tsfrg` file exists (e.g., `ru/onboarding.tsfrg`), the display tool should use it directly and skip AI translation for that fragment. This lets teams refine critical paths without rebuilding the authoring model.

---

## Known Challenges for Display Tool Authors

### State & Navigation
- **State on refresh:** Path state must be URL-serializable. For deep paths exceeding URL limits, use a GUID-backed share link with server-side storage; fall back to `sessionStorage` for offline tools.
- **Query string limits:** ~2K characters is the safe cross-browser limit. Hybrid strategy: query string for short paths, `sessionStorage` for long paths, GUID-backed links for sharing.

### Rendering & Content
- **Markdown flavor parity:** Authors may use GFM; display tools should handle tables, task lists, and fenced code blocks consistently.


## Contributing Display Tools

We welcome contributions toward reference display tool implementations (web viewer, CLI renderer, static exporter) that demonstrate the assembly-first and glossary patterns. Please open an issue to discuss the contract before submitting a new implementation.
