# Display Tool Contract

> **Document Status:** This contract describes the intended architecture and boundaries for Docs Assembler display tools. It has been revised to incorporate full‑path context translation, predictive pre‑fetching, and explicit safety guidelines. Sections marked 🚧 represent planned future enhancements.
>
> **Related Document:** For an in‑depth explanation of the technical rationale and implementation examples, see the [Predictive Pre‑fetching White Paper](./PREDICTIVE_PREFETCH_WHITEPAPER.md).

Docs Assembler intentionally keeps the authoring layer simple. Complexity that does not belong in a text editor  -  rendering, security, localization, search indexing, state management, and liability enforcement  -  is pushed to the display tool.

This document describes the boundaries and known challenges so display tool authors can build correctly.

---

## Terminology

- **Guide**  -  a tree of fragments (nodes). Each guide has a root fragment.
- **Fragment**  -  a single piece of content (Markdown or HTML). Fragments may have child fragments (branching).
- **Path**  -  the sequence of fragments from the root to the current fragment.

---

## Localization & Multi‑Language Display

Docs Assembler supports translation into any language. For **synthetic languages** (Russian, Arabic, Polish, German, etc.), grammatical features such as gender, case, number, tense, and aspect depend on the full narrative context. The display tool must ensure that translated fragments read naturally when assembled from a path.

### Recommended Approach: Full‑Path Context Translation

**Principle:** When translating a fragment, provide the entire narrative context in the **target language** (the text already shown to the user) plus the original source text if needed. Use a high‑quality LLM (e.g., DeepSeek) to translate only the new fragment, relying on the model’s natural language understanding to handle all grammatical agreement.

This method applies equally to **translating from English** and to **natively authoring in a synthetic language**  -  any fragment can be adapted to its parent path’s grammatical context by providing the full path as context. For example, a sub‑guide written in Russian for a “patient preparation” step will automatically change its case, tense, and gender when reused in a maternity ward guide versus a prostate cancer ward guide, because the full path context (the Russian text already displayed) gives the AI the required grammatical state.

**Advantages:**
- Works for any content (technical, creative, conversational).
- No manual extraction of gender, number, tense, or aspect.
- No language‑specific heuristics or lexicons required.
- Preserves idioms, style, and narrative flow.
- Maintains grammatical consistency with already‑displayed text.

**Prompt template for translation (English → Russian):**

```
System: You are a professional translator. Translate the following English text to Russian.
Preserve style, tone, idioms, and all grammatical relationships.
The Russian text already shown to the user is provided as context so you can match its gender, case, tense, and aspect.

Previous context (Russian, already displayed):
[Full Russian translation of root → parent fragments]

New text to translate (English):
[Current fragment English]

Output ONLY the Russian translation of the new text.
```

**For native rewriting (synthetic source → same language, different context):**

```
System: You are a professional technical writer. The following narrative is in [language]. Rewrite the last fragment so that it fits the grammatical context (gender, case, number, tense, aspect) of the previous narrative. Do not change the meaning or style.

Previous context ([language]):
[Full source text of root → parent fragments]

New fragment to adapt ([language]):
[Current fragment source text]

Output ONLY the adapted version of the new fragment.
```

### Optional Advanced Technique: Deterministic State Extraction (Not Recommended for General Use)

For very high‑volume, repetitive technical documentation, a display tool *may* implement manual grammatical state tracking (as described in earlier contract versions). However, this approach fails for creative content, requires significant language‑specific code, and is error‑prone. Authors are strongly encouraged to use the full‑path context method instead.

---

## Predictive Pre‑fetching (Required Optimisation)

To achieve real‑time perception, display tools **must** implement predictive pre‑fetching:

- After serving the current fragment (translated), identify all child fragments.
- For each child, construct the hypothetical full target‑language path (current target‑language context + child’s source text or child’s source text to be translated).
- Send parallel adaptation requests to the LLM API (with the prompt template above).
- Cache the results (server‑side, CDN, or browser) keyed by `hash(full_source_path):target_lang` (for translation) or `hash(full_source_path):same_lang` (for rewriting).
- When the user clicks a child, serve the pre‑fetched adaptation from cache (<10 ms).

If the user clicks before pre‑fetching completes (rare, given reading time), fall back to an on‑the‑fly API call with a small loading indicator.

**Caching notes:**
- Bust cache when the source text changes or the glossary updates.
- Use `stale-while-revalidate` for mobile networks.
- For offline reading, pre‑fetch entire subtrees when network is available.

---

## Grammatical State Propagation (Deprecated for General Use)

Earlier versions of this contract described a manual grammatical state object. That approach is **no longer recommended** for most display tools because it cannot handle general English content reliably. It remains documented here for legacy implementations but will be moved to an appendix in future versions.

For new display tools, use full‑path context translation as described above.

---

## The Glossary Pattern (Still Required)

Even with full‑path context translation, a glossary is essential to eliminate domain‑specific ambiguity. The display tool must accept a `glossary.yaml` that maps source terms to approved translations per locale. The glossary is passed to the translation API (e.g., in the system prompt or as a separate parameter).

**Example `glossary.yaml`:**

```yaml
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

style_brief:
  ru: "Use formal imperative verbs ending in -те. Prefer perfective aspect for single actions."
  fr: "Use formal 'vous' for instructions."
```

The style brief provides additional guidance (formality, aspect preference, etc.).

---

## Human Override 🚧

If a locale‑specific `.tsfrg` file exists (e.g., `ru/onboarding.tsfrg`), the display tool must use it directly and skip AI translation for that fragment. This allows teams to lock critical translations after human verification.

For safety‑critical guides (see below), **all fragments must be human‑verified**  -  either via `.tsfrg` or through a separate approval workflow. AI‑only translation is never acceptable for instructions where a mistake could cause harm or loss.

---

## Hallucination Risks and Safety

AI translation models (including DeepSeek, GPT‑4, etc.) can produce **hallucinations**: omissions, additions, distortions, or grammatical errors. The rate depends on content type:

| Content type | Estimated error rate |
|--------------|--------------------|
| Technical documentation (short, imperative) | 2 - 4% |
| User guides with variable referents | 5 - 8% |
| Narratives, scripts | 10 - 15% |

For **non‑critical** documentation (user manuals, help guides, marketing), these rates are acceptable when combined with human review of the most important fragments.

### ⚠️ CRITICAL SAFETY DISCLAIMER

> **For safety‑critical instructions (satellite launch procedures, medical device operation, nuclear facility control, aviation checklists, etc.):**
>
> - **Do not use AI‑only translation**  -  no LLM is reliable enough for autonomous use in such contexts. A single hallucination could be catastrophic.
> - **Acceptable workflow:** Use the display tool as a **productivity aid for human expert translators**. The AI produces a first draft; a qualified human (native speaker of the target language + domain expert) must verify and approve every instruction before use.
> - **For extremely high‑stakes scenarios,** use formal verification (translation memory with 100% exact match from pre‑approved, human‑validated phrases) instead of generative AI.

Display tool authors **must** include this disclaimer in their user documentation if the tool is intended for any application where translation errors could lead to physical, financial, or reputational harm.

---

## Caching (General)

Cache translations by a hash of the **full source path** (not just the fragment text). Bust the cache only when the source fragment, glossary, or style brief changes.

For shared hosting, use a distributed cache (Redis, Memcached). For a single‑user display tool, in‑memory caching is sufficient.

---

## Authoring Guidelines (Optional but Recommended)

To reduce the risk of ambiguity and hallucinations, authors are encouraged to:

- Keep fragments short (≤100 words).
- Use **imperative voice** for instructions (“Click the Save button”).
- Introduce characters with clear pronouns early (“Alice opens the door. She sees…”).
- Avoid long‑distance dependencies across many fragments (prefer to repeat the subject occasionally).

These guidelines improve translation quality for any LLM.

---

## Known Challenges for Display Tool Authors

### State & Navigation

- **Path state on refresh:** The full source path must be URL‑serializable (e.g., `?path=root,step1,step2`). For deep paths exceeding URL limits (2KB), use a GUID‑backed share link with server‑side storage.
- **Pre‑fetching on slow networks:** Use `stale-while-revalidate` and limit the number of parallel pre‑fetch requests (e.g., max 5).

### Rendering & Content

- **Markdown flavour parity:** Authors may use GFM; display tools should handle tables, task lists, and fenced code blocks consistently.
- **HTML fragments:** If fragments contain HTML, the translation API must preserve tag structure. Use the glossary to protect tag content from translation.

---

## Remote Guide Loading & Same-Origin Policy

Docs Assembler guides may reference fragments hosted on different origins - for example, a guide on `docs.example.com` may embed a sub-guide from `support.partner.org`. Browsers enforce the Same-Origin Policy and CORS; servers do not. Display tools therefore **must** route cross-origin fragment requests through a server-side proxy or edge function.

### Recommended proxy pattern

1. The display tool running on `site-a.com` needs a fragment from `site-b.com`.
2. It requests the fragment via its own proxy endpoint:
   `GET https://site-a.com/api/fragment-proxy?url=https://site-b.com/guides/fragment.md`
3. The proxy validates the target URL against an allowlist of trusted guide banks, fetches the raw fragment server-side, and returns it to the browser with appropriate `Access-Control-Allow-Origin` headers.
4. The browser receives the fragment as if it were same-origin.

### Nested references

If a proxied fragment contains references to additional remote fragments (e.g., `site-b.com` references `site-c.com`), the display tool must route **all** subsequent fragment requests through the same proxy. The proxy does not need to parse or rewrite Markdown; the display tool simply prefixes every fragment URL before fetching.

### Asset proxying

Fragments often reference images, diagrams, videos, or other assets stored alongside the source Markdown in the remote repository. If a fragment from `site-b.com` contains a relative asset URL such as `./assets/diagram.png`, the browser will fail to load it directly due to the same CORS restrictions.

Display tools must also route **asset requests** through the proxy:

- **Relative asset URLs** in a proxied fragment must be resolved to absolute URLs and prefixed through the proxy:
  `https://site-a.com/api/fragment-proxy?url=https://site-b.com/assets/diagram.png`
- **Absolute asset URLs** pointing to other trusted guide origins must likewise be routed through the proxy.
- The proxy should serve assets with correct MIME types and cache headers. Binary assets (images, videos, PDFs) do not require Markdown parsing - only safe URL validation and transparent byte forwarding.

The display tool is responsible for resolving relative URLs to absolute before proxying. The proxy validates the resolved URL and streams the response.

### Security requirements for proxy implementations

- **Allowlist validation:** Only proxy known documentation domains or guide banks. Never accept arbitrary URLs.
- **Path validation:** Ensure requested URLs match expected fragment or asset patterns (e.g., `*.md`, `*.png`, `*.jpg`, `*.svg`, `*.mp4`, `/guides/*`, `/fragments/*`, `/assets/*`).
- **Rate limiting:** Prevent abuse and unexpected cost.
- **No credential forwarding:** Do not forward browser cookies or auth headers to remote sites.
- **Content-Type preservation:** Return the correct MIME type for proxied assets so browsers render images, videos, and other media correctly.

For static hosting such as GitHub Pages, the proxy must be hosted separately (e.g., Cloudflare Worker, Netlify Edge Function, AWS Lambda, or a lightweight VPS).

---

## Contributing Display Tools

We welcome contributions toward reference display tool implementations (web viewer, CLI renderer, static exporter) that demonstrate the **full‑path context translation + predictive pre‑fetching** pattern. Please open an issue to discuss the contract before submitting a new implementation.

---

*This contract is maintained by the Docs Assembler project. Last revised: 2026.*