# White Paper: Predictive Pre‑fetching for Real‑Time High‑Quality Adaptation of Guides in Synthetic Languages

**For Display Tool Authors**  
*Target AI: DeepSeek API  -  high quality, low cost, large context window*

---

## Abstract

Guides assembled from reusable fragments (Markdown or HTML) often need to be presented in synthetic languages such as Russian, Arabic, Polish, or German. In these languages, grammatical features  -  gender, case, number, tense, aspect  -  depend on the full narrative path, not just the current fragment. The same sub‑guide may require different inflections when used in different parent contexts (e.g., a “patient preparation” step in a maternity ward vs. a prostate cancer ward).

This white paper presents **predictive pre‑fetching with full‑path context**: the display tool sends the entire narrative history (root → current fragment) to DeepSeek, which then **adapts** (translates or rewrites) the new fragment naturally  -  without manual grammatical state extraction. While the user reads the current fragment, all child fragments are pre‑adapted in parallel. The result: perceived real‑time delivery (<10 ms after click) with DeepSeek‑level quality, for any content  -  whether translating from English or natively re‑using fragments already written in a synthetic language.

A critical limitation is discussed: AI adaptation **must not be used autonomously for safety‑critical instructions** (satellite launches, medical devices). For such use cases, the system is a productivity aid for human expert translators or technical writers.

---

## 1. The Problem: Grammatical Context + Reuse + Latency

A **guide** is a tree of **fragments** (short Markdown/HTML texts). Readers navigate by clicking from fragment to fragment. When the target language is synthetic, grammatical features depend on the entire narrative path. Two challenges arise:

- **Translation from English**  -  English has little inflection, but Russian, Arabic, etc., require gender, case, number, tense, aspect that must be inferred from the path.
- **Native authoring in a synthetic language**  -  A writer creates a fragment in Russian for one context (e.g., masculine singular formal). That same fragment, when reused in a different parent path (e.g., feminine plural informal), must change its inflections. Doing this manually defeats reusability.

Traditional solutions:

- **On‑the‑fly adaptation**  -  call an LLM for each fragment. Quality is high, but users wait 5 - 15 seconds per click.
- **Deterministic state extraction**  -  manually track gender, number, etc. This fails for creative content, idiomatic expressions, and requires extensive language‑specific code.
- **Pre‑adapt all paths**  -  impossible for large guides due to exponential growth.

We need an approach that works for **any** English or synthetic source content, requires no language‑specific rules, hides latency, and is affordable.

---

## 2. The Solution: Full‑Path Context + Predictive Pre‑fetching

### 2.1 Core principle

**Give DeepSeek the complete narrative history (in the source language) up to the current fragment.** The model then adapts the *new* fragment  -  either translating it from English or rewriting it to fit the required grammatical context  -  using the full context to resolve gender, tense, aspect, and idioms  -  without any manual state extraction.

### 2.2 Step‑by‑step flow

1. **User selects a guide.**  
   - The display tool sends the root fragment’s source text (no prior context) to DeepSeek.  
   - Result is cached and displayed.

2. **While the user reads the current fragment (5 - 30 seconds):**  
   - The display tool identifies all child fragments from the current node.  
   - For each child, it constructs the **full hypothetical source path**:  
     `(root + ... + current + child)` source text.  
   - It sends parallel adaptation requests to DeepSeek, each requesting adaptation of the *child fragment* only (the context is provided for continuity).  
   - Each adapted result is cached with a key `hash(full_source_path) + target_language` (or same language if rewriting).

3. **User clicks a child fragment.**  
   - The adaptation is already in cache (server, CDN, or browser).  
   - Display tool serves it in <10 ms.

4. **Repeat**  -  after serving the new fragment, pre‑fetch its children.

### 2.3 Why this works for all content

| Challenge | How full‑path context solves it |
|-----------|--------------------------------|
| Gender of “the user” not specified in English | DeepSeek infers from prior mention (e.g., “Alice” → feminine). |
| Native Russian fragment needs different case when reused | Full path shows the required grammatical role (e.g., nominative vs. accusative). |
| Tense shifts across fragments | Full timeline gives correct past/present/future. |
| Aspect choice (perfective/imperfective) | Model reads narrative flow and chooses appropriately. |
| Idioms and fixed expressions | DeepSeek preserves or adjusts idioms correctly. |
| Pronouns (“he”, “she”, “they”) | Resolved naturally from preceding text. |
| Creative or literary style | Preserved because model adapts as a human would. |

**No manual lexicon, no heuristics, no language‑specific code  -  works for any source language.**

### 2.4 Prompt templates

#### For translation (English → synthetic language):

```
System: You are a professional translator. Translate the following English narrative to [target language].
Preserve style, tone, idioms, and all grammatical relationships.

Previous context (English):
[Full English of root → parent fragments]

New text to translate (English):
[Current fragment English]

Output ONLY the translation of the new text.
```

#### For native rewriting (synthetic source → same synthetic language, different context):

```
System: You are a professional technical writer. The following narrative is in [language]. Rewrite the last fragment so that it fits the grammatical context (gender, case, number, tense, aspect) of the previous narrative. Do not change the meaning or style.

Previous context ([language]):
[Full source text of root → parent fragments]

New fragment to adapt ([language]):
[Current fragment source text]

Output ONLY the adapted version of the new fragment.
```

**Example:** A Russian sub‑guide written for a masculine singular formal context when reused in a feminine plural informal context  -  the AI will change verb endings, adjectives, and pronouns accordingly.

---

## 3. Caching and Pre‑fetching Architecture

### 3.1 Cache layers

| Layer | Location | Purpose |
|-------|----------|---------|
| **Server cache** | Redis or in‑memory (128 GB typical) | Stores adaptations keyed by `hash(full_source_path):target_lang`. |
| **CDN / edge cache** | Cloudflare, Fastly | Reduce latency for global users. |
| **Browser cache** | `Cache API` or HTTP cache | Pre‑loaded via `rel=preload` or 103 Early Hints. |

### 3.2 Pre‑fetching implementation (pseudocode)

```python
async def on_fragment_rendered(fragment_id, full_source_path, source_lang, target_lang):
    children = get_child_fragments(fragment_id)
    tasks = []
    for child in children:
        child_path = full_source_path + child.source_text
        cache_key = hash(child_path) + target_lang
        if not cache.exists(cache_key):
            tasks.append(adapt_async(child_path, child.source_text, source_lang, target_lang))
    await asyncio.gather(*tasks)

async def adapt_async(full_path, new_text, src_lang, tgt_lang):
    prompt = build_prompt(full_path, new_text, src_lang, tgt_lang)
    response = await deepseek_api.acreate(prompt)
    cache.set(cache_key, response.adapted_text)
```

### 3.3 Mobile and slow networks

- Use **stale‑while‑revalidate**: serve an older cached version while fetching a fresh one.
- Pre‑fetch only critical children first; queue less likely branches.
- Compress adaptations over the wire (gzip/brotli).

---

## 4. Hallucination Risks and Mitigations

### 4.1 What “hallucination” means in adaptation

| Type | Example (English → Russian) | Likelihood (DeepSeek) |
|------|----------------------------|------------------------|
| Omission | Skipping a sentence or clause | <1% |
| Addition | Inserting extra words | ~1 - 2% |
| Distortion | Changing meaning (“click” → “hold”) | ~2 - 3% for technical text |
| Gender/agreement error | Wrong verb form due to ambiguous referent | 5 - 10% if context unclear |
| Idiom mistranslation | Literal translation of idiom | 5 - 15% for rare idioms |

### 4.2 Estimated hallucination rates by content type

| Content type | Visible error rate | Production acceptable? |
|--------------|--------------------|------------------------|
| Technical documentation (short, imperative) | 2 - 4% | ✅ Yes (with human review for critical steps) |
| User guides with variable referents | 5 - 8% | ⚠️ Use as assistive tool |
| Narratives, stories, film scripts | 10 - 15% | ❌ Not for final delivery without editing |

### 4.3 Critical safety disclaimer

> **For safety‑critical instructions (satellite launch procedures, medical device operation, nuclear facility control, aviation checklists, etc.):**
>
> - **Do not use AI‑only adaptation**  -  no LLM is reliable enough for autonomous use in such contexts. A single hallucination could be catastrophic.
> - **Acceptable workflow:** Use the system as a **productivity aid for human expert translators or writers**. The AI produces a first draft; a qualified human (native speaker of the target language + domain expert) must verify and approve every instruction before use.
> - **For extremely high‑stakes scenarios,** use formal verification (translation memory with 100% exact match from pre‑approved, human‑validated phrases) instead of generative AI.

### 4.4 Practical mitigations for non‑critical content

- **Glossary**  -  forces correct terminology.
- **Style brief**  -  guides formality and aspect.
- **Keep fragments short** (≤100 words)  -  reduces omission risk.
- **Confidence check**  -  use a small local NLI model to compare source and adapted text; if confidence low, re‑adapt or flag for human review.
- **Human override**  -  allow locale‑specific `.tsfrg` files to replace AI adaptations for critical fragments.

---

## 5. Cost and Performance with DeepSeek

- **Input cost:** $0.14 per million tokens.
- **Output cost:** $0.28 per million tokens.
- A typical fragment (100 words) uses ~130 tokens for context + 130 for output ≈ 260 tokens → **~$0.00007 per adaptation**.
- Pre‑fetching adds parallel requests, but only for branches users actually encounter.
- For a 10,000‑fragment guide adapted into 3 languages (or rewritten for 3 contexts), one‑time pre‑fetch cost is under $50.

**Latency:** API call takes 5 - 15 seconds, but hidden by user reading time. Cache hits are <10 ms.

---

## 6. Integration with the Display Tool Contract

The companion `DISPLAY_TOOLS.md` defines the full contract, including:
- Full‑path context as the recommended method.
- Predictive pre‑fetching as required.
- Hallucination risk table and safety disclaimer.
- Glossary and style brief patterns.

This white paper extends that contract by explicitly covering **native authoring in synthetic languages**  -  a crucial use case for teams writing directly in Russian, Arabic, Polish, etc., who need to reuse fragments across dramatically different grammatical contexts (e.g., maternity ward vs. prostate cancer ward documentation).

---

## 7. Implementation Checklist for Display Tool Authors

- [ ] Store fragments in a tree structure (guide) with parent/child relationships, keeping original source text.
- [ ] On user navigation, compute the full source path by concatenating fragment texts (in the original language).
- [ ] Call DeepSeek API with the appropriate prompt template (translation or rewriting)  -  full path as context, current fragment as new text.
- [ ] Cache adaptations in Redis with key `hash(full_source_path):target_lang` (for translation) or `hash(full_source_path):same_lang` (for rewriting).
- [ ] After serving a fragment, pre‑fetch all child fragments (parallel API calls, same caching).
- [ ] Optionally use HTTP/103 Early Hints or `<link rel=preload>` to push adaptations to browser.
- [ ] For mobile, implement stale‑while‑revalidate and offline storage (IndexedDB).
- [ ] Add a glossary and style brief for domain‑specific terminology.
- [ ] For safety‑critical guides, include a human‑verification workflow and never use AI output directly.

---

## 8. Conclusion

Predictive pre‑fetching with full‑path context adaptation delivers **DeepSeek‑quality grammatical adaptation** for synthetic languages  -  whether translating from English or natively rewriting fragments to fit new contexts  -  at perceived real‑time speeds. The system works for any content, requires no language‑specific rules, and is affordable and easy to implement.

This approach solves the long‑standing problem of reusing fragments in highly inflected languages: a sub‑guide written once can be automatically adjusted for a maternity ward, a prostate cancer ward, or any other parent context, simply by providing the full narrative path as context.

However, AI adaptation is not infallible. For mission‑critical instructions, this system is a **tool for human experts**, not an autonomous replacement. Used responsibly, it can dramatically reduce the cost and time of producing high‑quality multilingual and multicontext documentation.

---

*This white paper is part of the Docs Assembler ecosystem. For questions or contributions, [**open an issue on the repository**](https://github.com/netoftrees/docs-assembler/issues).*