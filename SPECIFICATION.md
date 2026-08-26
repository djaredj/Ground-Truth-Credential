# Ground Truth Credential (GTC) — Mark Usage Specification v0.1

**Author:** Dee Jared Johnson | **Date:** 2026-08-12 | **Status:** DRAFT

---

## 1. The Mark

The GTC mark is a deployer-side disclosure symbol indicating that the content it accompanies carries a structured, verifiable disclosure manifest. The mark's form references the visual language of a thought bubble — the shape points to origin; the interior carries meaning.

The mark is a designed glyph created by DJJ. It is a companion to provider-side provenance marks (C2PA Content Credentials, Anthropic watermarks), never a replacement.

### 1.1 Mark Variants

| Variant | Use |
|---|---|
| **Symbol only** | Inline placement on images, text blocks, documents, metadata. Primary use case. |
| **Symbol + wordmark ("GTC")** | Documentation, the GitHub README, the usage spec, explainer contexts. Wordmark sits to the right of or below the symbol, never inside it. |
| **Symbol as font glyph** | Unicode Private Use Area codepoint embedded in a dedicated GTC font. For text-only contexts where image marks are stripped. Survives copy-paste, platform reformatting, and text-only rendering. |

### 1.2 Font Deliverables

Two font files:

- **GTC-Symbol.otf** — Single-glyph font. The GTC mark mapped to one PUA codepoint. For mixed-font workflows: type body text in any font, switch to GTC-Symbol for the closing mark.
- **GTC-Text.otf** — Full text font with the GTC symbol added to the PUA. For controlled environments where the full typographic stack is owned.

### 1.3 Master File Formats

- SVG (vector master, all variants)
- PNG (exports at 16px, 32px, 64px, 128px, 256px, 512px)
- OTF/TTF (font glyph)
- ICO/favicon (for verification page)

---

## 2. Color

Two colors only. No gradients, no outlines, no partial transparency.

| Context | Mark Color | Background |
|---|---|---|
| Light backgrounds | Black (#000000) | Any background where black provides sufficient contrast (WCAG AA minimum 4.5:1) |
| Dark backgrounds | White (#FFFFFF) | Any background where white provides sufficient contrast (WCAG AA minimum 4.5:1) |

### 2.1 Color Rules

- The mark is always solid — black or white, never gray, never brand-colored, never tinted.
- If neither black nor white achieves WCAG AA contrast against the background, the mark must be placed on a contrast pill (small rectangular or rounded-rect backing shape) in the opposing color.
- The mark never adopts the colors of the content it accompanies.

---

## 3. Sizing

### 3.1 Minimum Sizes

| Context | Minimum Size |
|---|---|
| Digital screen (images, web) | 16px height |
| Print | 4mm height |
| Text-inline (font glyph) | Cap height of the body text |

### 3.2 Clear Space

The mark requires clear space equal to 100% of the symbol's width on all sides. No text, imagery, or other marks may intrude into this zone.

Exception: when co-placed with legal/identity marks per Section 4.3, clear space may be reduced to 50% of symbol width on the side adjacent to the legal mark.

---

## 4. Placement

### 4.1 On Images

The mark is placed in any corner of the image where it:

- Complements the fundamentals of design (visual balance, does not obscure the subject).
- Does not cover, overlap, or invalidate copyrights, trademarks, identity marks, or other legal ownership symbols.
- Is readily visible — not hidden in areas of matching tonal value.

Default position: upper-left corner, inset by one clear-space unit from both edges. Alternate corners are permitted when design fundamentals require it.

### 4.2 On Text Blocks

The mark is placed **block-aligned right on the final line** of the text block.

- The symbol sits at the **baseline** of the last line of text.
- The symbol is sized to match the **cap height** of the body text.
- If the final line has insufficient space (less than 200% of the symbol width remaining), the symbol drops to a new line, right-aligned.
- The symbol **never** floats mid-paragraph.
- The symbol **never** appears on any line other than the final line.
- In multi-paragraph content (articles, posts), the mark appears once at the end of the entire block, not per paragraph.

### 4.3 Co-placement with Legal / Identity Marks

When the content carries copyrights, trademarks, identity marks, or entity marks:

- The GTC mark may be placed **at visible size in proportion to** these marks.
- The GTC mark is placed either:
  - **Inside** the identity/trademark lockup area, following the fundamentals of design, OR
  - **Within a distance of 200%** of the GTC symbol's width from the nearest legal entity mark.
- The GTC mark **never** covers, overlaps, or visually competes with legal marks.
- When placed alongside the C2PA CR badge: the GTC mark occupies a different corner or is separated by at least 200% of the larger mark's width.

### 4.4 On Documents (PDFs, Reports)

- **Cover page:** Symbol placed per image rules (Section 4.1) on any cover image, or in the document header/footer area.
- **Interior pages:** Symbol appears once in the document footer of the first page, right-aligned, at the cap height of the footer text.
- **The manifest URL or hash** appears adjacent to the mark in the footer in the document's body font at a reduced size (70–80% of body text).

### 4.5 In Metadata

The GTC symbol **must** be included in the metadata of any file it is linked with. This is not optional — a visible mark without a metadata entry is incomplete, and a metadata entry without a visible mark is permitted but not preferred.

Metadata fields (format-dependent):

| Format | Metadata Location |
|---|---|
| JPEG/PNG/TIFF | XMP `dc:rights` or custom XMP namespace |
| PDF | XMP metadata stream or document properties |
| DOCX/PPTX | Custom document property |
| Plain text | GTC font glyph (PUA codepoint) at end of text |
| HTML | `<meta>` tag or microdata/JSON-LD |
| C2PA-enabled files | Custom assertion sidecar within the C2PA manifest |

---

## 4.6 On Social Media Posts

Social media content presents three placement contexts not fully addressed in §4.1–4.4. The GTC Tools social credentialer implements these rules; manual placement must follow them.

- **Text-only posts**

The mark is appended at the end of the post text as a styled superscript rendered in the platform's default sans-serif font stack. The display form is `GTC` in superscript. In plain-text clipboard contexts where font styling cannot be carried, the mark is represented as `[GTC]` — a bracketed plain-text fallback that survives platform reformatting and copy-paste.

The mark never appears mid-post. It appears once, at the end of the full text block.

- **Image posts**

The mark follows §4.1 (image placement rules). Default position is top-left, inset by one clear-space unit. The deployer moves to an alternate corner when another provenance mark (C2PA CR badge, platform watermark) already occupies the default corner. The GTC Tools auto-contrast mode samples the actual image luminance at the target corner and selects black or white accordingly — manual placement must do the same.

- **Mixed posts (text + image)**

Both placements are applied: the mark appears on the image per image rules AND is appended to the text per text-only rules. This is intentional — the two marks serve different verification surfaces. A reader who screenshots the image and strips the text still has the mark; a reader who copies the text and loses the image still has the mark.

- **Video posts**

The GTC mark is composited onto the video cover frame (thumbnail), not burned into the video file itself. The credentialed cover frame is uploaded to the platform as the video thumbnail. The manifest's `content_hash` is the SHA-256 of the video file bytes; the `hash_scope` field must state this explicitly.

- **PDF / carousel posts**

The mark is composited onto the cover page or first slide and uploaded as the cover image. The manifest's `content_hash` is the SHA-256 of the PDF or presentation file bytes; `hash_scope` must state this explicitly.

- **Audio posts**

No visual mark is applied to the audio file itself. The mark appears on any accompanying cover art or post text. The manifest's `content_hash` is the SHA-256 of the audio file bytes.

- **Platform coverage**

GTC is platform-agnostic. The C2PA CR badge is limited to images on LinkedIn. GTC applies to all content formats (text, image, video, PDF, audio, document) on all platforms (LinkedIn, X / Twitter, Instagram, Facebook, Threads, Bluesky, Mastodon, YouTube, TikTok, and others). The `content.platform` field in the manifest records which platform the post was published on.

- **Manifest fields for social posts**

Social post manifests must include:

| Field | Value |
|---|---|
| `content.type` | `post` |
| `content.platform` | Platform name (e.g. `LinkedIn`) |
| `content.content_format` | `text`, `image`, `both`, `video`, `pdf`, `audio`, or `doc` |
| `content.content_hash` | SHA-256 of the text body (text posts) or file bytes (media posts) |
| `content.hash_scope` | Explicit description of what was hashed |

---

## 5. Prohibitions

The GTC mark must NOT be:

- Stretched, skewed, rotated, or distorted in any way.
- Recolored outside the black/white specification.
- Used with drop shadows, glows, outlines, or other effects.
- Rendered at less than the minimum size.
- Used without a linked, verifiable disclosure manifest. A GTC mark with no backing manifest undermines the protocol's premise and is a misuse.
- Placed over or touching other provenance marks (CR badge, watermarks) without the specified clear space.
- Used to imply endorsement, certification, or approval by any entity other than the content's deployer.
- Used on content the deployer did not produce or authorize.
- Partially reproduced (e.g., cropping the tail of the thought-bubble shape).

---

## 6. Manifest Linkage

Every instance of the GTC mark must resolve to a disclosure manifest. The link method is one of:

| Method | Mechanism | Status |
|---|---|---|
| **QR / short URL** | Embedded in or adjacent to the mark; points to manifest on GitHub or verification site | [PROPOSED] |
| **Hash in metadata** | SHA-256 hash of the manifest embedded in the file's metadata fields | [PROPOSED] |
| **C2PA custom assertion** | Disclosure manifest attached as a C2PA assertion sidecar alongside standard assertions | [PROPOSED] |
| **PUA glyph detection** | Automated tools detect the GTC font codepoint in text and query a manifest registry | [PROPOSED] |

Link method is not yet locked. Multiple methods may be used simultaneously.

---

## 7. Versioning

The visual mark does not carry a version indicator. Versioning lives in the data layer:

- Manifest schema version is declared in the JSON manifest's `schema_version` field.
- The mark's visual form is version-independent — it does not change when the schema evolves.
- If the mark's visual form ever changes (major redesign), the old and new forms are documented in this spec with effective dates.

---

## 8. Usage Gallery

*To be populated with real examples from Glasswing Issues 001–014+ after retroactive manifest generation (GTC execution path step 4). Each example demonstrates the mark in a different context: image placement, text block placement, document footer, dark background, light background, co-placement with CR badge.*

---

## 9. Revision History

| Version | Date | Changes |
|---|---|---|
| 0.1 | 2026-08-12 | Initial scaffold. Mark form, color, sizing, placement, prohibitions, manifest linkage, versioning. |
| 0.1.1 | 2026-08-26 | Added §4.6: social media placement rules for text, image, mixed, video, PDF/carousel, and audio posts. Added platform coverage note. Added social manifest field table. |
