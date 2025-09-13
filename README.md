# Unifea — Featural Auxiliary Script

**Version:** v0.1 (Draft)  
**Date:** 2025‑09‑13  
**Editors:** Naren Yellavula
**Status:** Draft for experimentation. Suitable for prototypes and tooling; not yet proposed for Unicode/ISO‑15924.

---

## Abstract
Unifea is a **featural, language‑agnostic auxiliary script** that encodes speech as bundles of articulatory features rather than as historical letter shapes. Each grapheme is built from **place**, **manner**, and **voicing** (for consonants), and **frontness**, **height**, and **rounding** (for vowels), plus **modifiers** for prosody and secondary articulation. Unifea defines two complementary representations:

- **Radical form (analytic):** human‑readable feature symbols (e.g., `○ + ┃ + •`).
- **Visual form (composed):** tiled glyphs that *combine* features without plus signs, suitable for reading and typesetting.

This v0.1 spec defines the model, shape rules, draft encoding, and conformance guidance.

---

## 1. Scope & Goals
**Goals (G):**
- G1 — **Learnable:** visual logic mirrors articulation (feature‑to‑shape mapping is mnemonic).
- G2 — **Universal coverage:** scalable to the IPA inventory; practical defaults for common phonemes.
- G3 — **Digital‑native:** deterministic composition, font‑friendly, searchable and encodable.
- G4 — **Flexible layout:** supports syllable blocks, ligatures, and multiple writing directions.
- G5 — **Accessibility:** high contrast, diacritic consistency, future Braille mapping.

**Non‑goals (NG):**
- NG1 — Replacing native scripts.
- NG2 — Phonemic decisions per language; Unifea focuses on **phonetic** representation (with optional language presets).

---

## 2. Grapheme Model
Unifea text is a sequence of **graphemes**. Each grapheme is either a **consonant (C)**, **vowel (V)**, or **diphthong (D)** (rendered as V→V). Graphemes accept **modifiers** (prosodic or secondary articulation).

### 2.1 Consonants (C)
A consonant grapheme is an ordered feature triple:
```
PLACE  →  MANNER  →  VOICING  [→ MODIFIERS*]
```
- **Place:** { bilabial ○, labiodental ⊂, dental ¦, alveolar |, retroflex ⌐, palatal ∧, postalveolar ∧, velar ⋀, uvular ∪, pharyngeal ˬ, glottal ˽ }
- **Manner:** { stop ┃, fricative ~, nasal ∩, affricate ›, approximant ⌣, lateral ⌢, tap ≡, trill ≡≡ }
- **Voicing:** { voiceless ∅, voiced • }
- **Common secondary modifiers:** aspiration ʰ, labialization ʷ, palatalization ʲ, pharyngealization ˤ.

### 2.2 Vowels (V)
A vowel grapheme is an ordered triple:
```
FRONTNESS  →  HEIGHT  →  ROUNDING  [→ MODIFIERS*]
```
- **Frontness:** { front ◐, central ◑, back ◒ }
- **Height:** { close ▲, mid △, open ▽ }, with **near** values as doubled/stepped marks: near‑close ▲△, open‑mid △▽, near‑open ▽▽.
- **Rounding:** { unrounded ∅, rounded ○ }.

### 2.3 Diphthongs (D)
A diphthong is a two‑vowel sequence rendered **V₁ → V₂** (arrow between two vowel tiles). Tone and stress apply to the **syllable**; length may apply to the nucleus.

### 2.4 Modifiers (M)
Available across C/V (some more relevant to one class):
- **Length:** ː (long)
- **Nasalization:** ̃ (primarily on vowels)
- **Stress:** ˈ (primary), ˌ (secondary)
- **Secondary articulation on C:** ʰ, ʷ, ʲ, ˤ
- **(Optional extension)** Tone marks (high ˉ, low ˍ, rising ˊ, falling ˋ) — **informative** in v0.1

---

## 3. Shape Rules (Visual Form)
Visual form composes glyphs **without plus signs**. Radicals are an analysis/debug view only.

### 3.1 Consonant Tile Layout
- **Grid:** 2×2 pseudo‑grid (place top‑left, manner top‑right, voicing dot centered on lower row).
- **Voicing:** `•` under the main pair; voiceless leaves the slot empty.
- **Suprasegmentals:** stress marks float **above** the tile; length below; other C‑modifiers at upper‑right.

### 3.2 Vowel Tile Layout
- **Row:** position mark (◐/◑/◒) + height mark (▲/△/▽ or stepped variants), with optional rounding ○ below.
- **Suprasegmentals:** stress above, length below, nasalization (˜) at upper‑right.

### 3.3 Diphthongs
- Render **tile(V₁)**, arrow **→**, **tile(V₂)**. No parentheses or plus signs in the visual stream.

### 3.4 Syllable Blocks (optional view)
- Group **Onset (C*) + Nucleus (V/D) + Coda (C*)** into a box; stress floats above the block; length below the nucleus region. This view is informative in v0.1.

### 3.5 Ligatures (optional)
- Common clusters (e.g., **st, tr, pl, ŋg, ks**) MAY fuse into ligatures to reduce visual density. Ligature choices are **rendering‑level**; the underlying feature sequence MUST remain recoverable.

### 3.6 Writing Direction
- Default **LTR**. Scripts MAY also be set **RTL** or **vertical** in artistic contexts, but tooling support is **out of scope** for v0.1.

---

## 4. Radicals (Analytic Form)
Radicals are the canonical feature symbols used for teaching, debugging, and round‑trip clarity. They MUST NOT appear in Visual mode.
- Example (C): `○ + ┃ + •` (voiced bilabial stop → /b/)
- Example (V): `◒ + ▲ + ○` (close back rounded → /u/)

---

## 5. Inventory & Mapping (Partial in v0.1)
This draft covers a practical subset; the complete mapping targets IPA coverage in v0.2.

### 5.1 Consonant Examples
- Stops: p `○+┃`, b `○+┃+•`, t `|+┃`, d `|+┃+•`, k `⋀+┃`, g `⋀+┃+•`, ʔ `˽+┃`
- Fricatives: f `⊂+~`, v `⊂+~+•`, θ `¦+~`, ð `¦+~+•`, s `|+~`, z `|+~+•`, ʃ `∧+~`, ʒ `∧+~+•`, h `˽+~`
- Nasals: m `○+∩+•`, n `|+∩+•`, ŋ `⋀+∩+•`
- Approximants/Laterals: ɹ `|+⌣+•`, l `|+⌢+•`, w `○+⌣+•`, j `∧+⌣+•`

### 5.2 Vowel Examples
- i `◐+▲`, e `◐+△`, a `◐+▽`, ə `◑+△`, u `◒+▲+○`, o `◒+△+○`, ɑ `◒+▽`, ʊ `◒+▲△+○`, ɔ `◒+△▽+○`.

### 5.3 Diphthongs
- eɪ: `(◐+△) → (◐+▲)`
- oʊ: `(◒+△+○) → (◒+▲+○)`
- aɪ: `(◐+▽) → (◐+▲)`

> **Note:** Dental (¦) and alveolar (|) are distinct. Retroflex is ⌐. Postalveolar currently reuses ∧; a dedicated mark MAY be introduced in v0.2.

---

## 6. Encoding (Draft) — “Unifea PUA v0”
This section defines a **prototype** encoding in the Unicode **Private Use Area (PUA)** to enable storage, search, and rendering while Unifea is pre‑standard.

### 6.1 Code Ranges
- **Places:** U+E100–U+E109  
- **Manners:** U+E120–U+E127  
- **Voicing dot:** U+E140  
- **C‑modifiers:** U+E150 (ʰ), U+E151 (ʷ), U+E152 (ʲ), U+E153 (ˤ)  
- **Length:** U+E161 (ː); **Nasalization:** U+E160 (̃)  
- **Stress:** U+E170 (ˈ), U+E171 (ˌ)  
- **Vowel frontness:** U+E200–U+E202 (◐ ◑ ◒)  
- **Vowel height:** U+E210–U+E212 (▲ △ ▽)  
- **Vowel rounding:** U+E220 (○)

*(Exact assignments match the playground encoders used in prototypes.)*

### 6.2 Canonical Order (NUF — Normalization, Unifea Form)
- **Consonant grapheme:** PLACE → MANNER → VOICING → MODIFIERS (arbitrary order within the modifier class).  
- **Vowel grapheme:** FRONTNESS → HEIGHT → ROUNDING → MODIFIERS.  
- Encoders MUST emit this order; normalizers MUST reorder to it for equality tests.

### 6.3 Grapheme Boundaries
- One grapheme = one feature sequence (C or V) + zero or more modifiers.  
- **Optional delimiters:** Implementations MAY use PUA **U+E17E (Grapheme Terminator)** to force a boundary in ambiguous tooling; MUST be ignored by renderers.

### 6.4 Diphthongs
- Encode as **V₁ sequence**, followed by **U+2192 (→)**, followed by **V₂ sequence**, or by a PUA arrow if a pure‑PUA stream is required.

### 6.5 BCP‑47 Tagging
- Until an ISO‑15924 code exists, tag content as: `und-x-unifea` (generic) or `und-Qunf` (private‑use script subtag placeholder “Qunf”).

### 6.6 ASCII Transliteration (for logs/tests)
- **C:** `C(place,manner[,voiced][,mods…])` e.g., `C(bilabial,stop,voiced,ʰ)`
- **V:** `V(front,height[,rounded][,mods…])` e.g., `V(back,close,rounded,ː)`
- **D:** `{D: V1->V2}` e.g., `{D:(back,mid,rounded)->(back,close,rounded)}`

---

## 7. Rendering & Fonts
**OpenType:**
- Use **ccmp** to compose features into tiles, **mark/mkmk** for diacritics, **calt/liga** for ligatures and contextual fusion.
- **Anchors:** top (stress), right‑top (C‑mods, nasalization), bottom (length), center‑bottom (voicing dot).
- **Fallback:** Without a Unifea font, PUA displays as tofu; engines SHOULD present codepoint fallbacks in developer tools.

**Metrics:**
- Consonant tile: square aspect; vowel tile: slightly narrower.  
- Syllable blocks (optional view) draw a rounded rectangle around consecutive glyphs C*(V/D)C*.

---

## 8. Collation & Search
- **Default search:** by **radicals** (feature sequence) or by **IPA** if a transliterator is present.  
- **Sort keys:** PLACE→MANNER→VOICING for C; FRONTNESS→HEIGHT→ROUNDING for V; modifiers sort after the base.

---

## 9. Input Methods (IME)
- **Feature‑chord keyboard:** left hand = place/frontness; right hand = manner/height; thumb = voicing/rounding toggle.  
- **IPA→Unifea transliterator:** tokenize IPA, prefer multi‑char compounds (t͡ʃ, d͡ʒ, oʊ…), attach modifiers to preceding base, map to features, and render.

---

## 10. Accessibility
- **Contrast:** monochrome legibility; avoid relying solely on color.  
- **Braille (future):** map PLACE, MANNER, VOICE to cell regions; V‑triples to consistent dot bundles.  
- **Screen readers:** expose feature strings (radicals) as `aria-label` equivalents.

---

## 11. Examples & Test Vectors
(Visual shows tiles; radicals show analytic strings.)

1) **hello** — IPA: `h ɛ ˈ l oʊ`  
- Radicals: `(˽ + ~) (◐ + △▽) [ˈ] (| + ⌢ + •) (◒ + △ + ○) → (◒ + ▲ + ○)`  
- PUA (illustrative): `E109 E121   E200 E211 …  E103 E125 E140   E202 E211 E220 2192 E202 E210 E220`

2) **script** — IPA: `s k r ɪ p t`  
- Radicals: `(| + ~) (⋀ + ┃) (| + ⌣ + •) (◐ + ▲△) (○ + ┃) (| + ┃)`  
- Ligature (optional): `sk` and `tr` classes MAY fuse.

3) **bhārat** — IPA: `b ʱ aː r ə t`  
- Radicals: `(○ + ┃ + •)[ʰ] (◐ + ▽)[ː] (| + ≡≡ + •) (◑ + △) (| + ┃)`

4) **salām** — IPA: `s a l ãː m`  
- Radicals: `(| + ~) (◐ + ▽) (| + ⌢ + •) (◐ + ▽)[̃ː] (○ + ∩ + •)`

5) **strength** — IPA: `s t ɹ e ŋ k θ`  
- Radicals: `(|+~) (|+┃) (|+⌣+•) (◐+△) (⋀+∩+•) (⋀+┃) (¦+~)`

> Implementations SHOULD pass round‑trip tests (IPA → Unifea → radicals → Visual → PUA) for these vectors.

---

## 12. Conformance
Define the following **conformance classes**:
- **Encoder:** Accepts IPA or feature tokens → emits Unifea radicals and PUA per §6.2 order. **MUST** normalize to NUF.  
- **Renderer:** Displays Visual tiles from feature sequences. **MUST NOT** show plus signs in Visual.  
- **Shaper:** Applies ligatures and syllable blocks non‑destructively; base sequence **MUST** remain recoverable.  
- **IME:** Produces valid sequences and attaches modifiers to the correct base.

An implementation claiming “Unifea v0.1 compliant” MUST implement Encoder and Renderer; Shaper and IME are SHOULD.

---

## 13. Roadmap & Open Issues
- **Coverage:** add full IPA (ejectives, implosives, clicks, pharyngeals, tones) and language presets (phonemic).  
- **Postalveolar mark:** introduce a unique radical separate from ∧ (palatal).  
- **Arrow encoding:** PUA arrow for diphthongs in pure PUA streams.  
- **Standardization:** draft for Unicode (character model) and ISO‑15924 (script code).  
- **Accessibility:** define a Braille standard mapping; tactile variant.  
- **Tooling:** finalize CSV mapping; build GSUB/CCMP‑based font; publish test suite.

---

## Appendix A — Reference Tables (excerpt)
**Places (C):** `○ ⊂ ¦ | ⌐ ∧ ⋀ ∪ ˬ ˽`  
**Manners (C):** `┃ ~ ∩ › ⌣ ⌢ ≡ ≡≡`  
**Vowel frontness:** `◐ ◑ ◒`  
**Vowel heights:** `▲ △ ▽` (+ stepped variants `▲△`, `△▽`, `▽▽`)  
**Rounding:** `○`  
**Voicing:** `•`  
**Modifiers:** `ʰ ʷ ʲ ˤ ː ̃ ˈ ˌ`

---

## Appendix B — Licensing (proposal)
- **Spec text:** CC BY 4.0  
- **Reference fonts:** SIL Open Font License (OFL)  
- **Sample code:** Apache‑2.0

---

## Appendix C — Companion Artifacts (prototype)
- Encoding playground (HTML) with PUA/ASCII export.
- Ligature visualizer (HTML) with plus‑less tiles for C/V.
- Full mapping CSV (partial now; expand to full IPA in v0.2).

*End of v0.1 draft.*

