# Ground Truth Credential (GTC)

**A deployer-side disclosure protocol for AI-assisted content.**

---

## The Problem

Provider-side provenance is largely solved.

C2PA Content Credentials tell you what tool made something. Anthropic's watermarks prove Claude processed it. EU AI Act Article 50 (enforceable August 2, 2026) mandates machine-readable marks on AI-generated content. LinkedIn now displays the CR badge on images carrying Content Credentials.

None of them answer the deployer question:

**What structural conditions existed when this content was produced — and what conflicts exist between the tool, the subject, and the author?**

A watermark says *AI was here.* A Content Credential says *this tool made it.* Neither says:

> "The person who published this has a financial relationship with the AI provider they're writing about."

> "The model used to draft this analysis was built by the company the analysis critiques."

That layer doesn't exist yet. GTC fills it.

---

## What GTC Is

GTC is an open protocol for deployer-side disclosure of AI-assisted content. Three components:

**1. A visual mark** — a designed glyph (created by DJJ) signaling "this content carries a structured disclosure chain." The mark sits alongside provider-side marks (CR badge, watermarks), not instead of them.

**2. A JSON disclosure manifest** — a structured, hashable document containing:
- The AI system used and its provider
- The subject relationship (who or what the content is about)
- The author's structural conflicts of interest
- A SHA-256 content hash
- A timestamp
- A verification URL

Schema: [`schema/gtc-manifest-v0.1.json`](schema/gtc-manifest-v0.1.json)

**3. A verifiable chain** — manifests are stored in this public repository, committed with Git timestamps. The chain cannot be fabricated after the fact.

---

## What GTC Is Not

- **Not a replacement for C2PA / Content Credentials.** GTC is a companion protocol. C2PA handles provider-side provenance. GTC handles deployer-side disclosure. Complementary, not competing.
- **Not a watermark.** GTC is a visible, voluntary disclosure mark — not an invisible, embedded signal.
- **Not a certification or endorsement.** The mark signals that a disclosure manifest exists and is verifiable. It does not certify that the content is accurate or endorsed by any entity.
- **Not DRM or copyright protection.**

---

## The Gap This Fills

| Layer | What It Answers | Who Provides It | Status |
|---|---|---|---|
| **C2PA / Content Credentials** | What tool made this? | Provider (Adobe, camera OEMs) | Mature for images; text is least tested |
| **AI watermarks** (Anthropic, Google SynthID) | Did an AI model process this? | Provider (Anthropic, Google) | Active; text watermarks launched August 2026 |
| **EU AI Act Article 50** | Is AI-generated content machine-readably marked? | Provider obligation, regulatory mandate | Enforceable August 2, 2026 |
| **GTC (this protocol)** | What conflicts exist between the tool, the subject, and the author? | **Deployer** | **This project** |

---

## Quick Start

### Using the GTC Tools

**[→ Open GTC Tools](https://djaredj.github.io/Ground-Truth-Credential/tools/)** — runs entirely in-browser, no account required.

Five tabs:

- **Verify** — paste text or upload any file to generate a SHA-256 hash for comparison against a manifest's `content_hash`. Look up manifests by ID or URL.
- **Create Manifest** — fill in deployer, content, AI system, and disclosure fields; the tool generates a ready-to-commit JSON manifest.
- **Social Post Credentialer** — credential text posts, images, video, PDFs, carousels, audio, and documents for any platform. Composites the GTC mark onto images, appends it to text, hashes files, and generates the manifest.
- **Site Audit** — single-item audit (hash check + disclosure validation + schema check) and bulk spreadsheet audit (CSV import/export, row-by-row status, full audit report download).
- **Image Verifier** — reads an image's own declared provenance: parses any C2PA manifest (via `@contentauth/c2pa-web`, WASM) and XMP/EXIF metadata (via `exif-js`) to report its digital source type — `trainedAlgorithmicMedia`, `compositeWithTrainedAlgorithmicMedia`, `compositeCapture`, or `digitalCapture`. Absent metadata is reported as "Undeclared," never inferred as evidence of AI generation. A handful of clearly-labeled, non-authoritative heuristic flags are shown separately and never affect the declared classification. Everything — including C2PA validation — runs client-side; nothing is uploaded. See [`schema/gtc-image-verification-schema-v0.1.json`](schema/gtc-image-verification-schema-v0.1.json) for the report format.

### Reading a GTC manifest manually

1. Find the GTC mark on the content (corner of image, end of text block, document footer).
2. Follow the verification link or locate the manifest in [`manifests/`](manifests/).
3. Check the `content_hash` field against the content's SHA-256 hash (use the Verify tab or `sha256sum`).
4. Read the `disclosures` array for the deployer's structural conflict declarations.

### Creating a GTC manifest manually

See the [annotated example](examples/example-manifest.json) and the [JSON schema](schema/gtc-manifest-v0.1.json).

1. Write your content.
2. Generate the SHA-256 hash of the final published version.
3. Fill in the manifest fields per the schema — or use the Create Manifest tab in [GTC Tools](https://djaredj.github.io/Ground-Truth-Credential/tools/).
4. Commit the manifest to this repository or publish it at a stable URL.
5. Place the GTC mark on your content per the [usage specification](SPECIFICATION.md).

---

## Repository Structure

```
ground-truth-credential/
├── README.md                    ← You are here
├── LICENSE                      ← CC BY 4.0
├── SPECIFICATION.md             ← Mark usage specification
├── schema/
│   ├── gtc-manifest-v0.1.json               ← JSON Schema for disclosure manifests
│   └── gtc-image-verification-schema-v0.1.json  ← JSON Schema for Image Verifier reports
├── mark/
│   ├── gtc-symbol-black.svg    ← Vector master, black on light
│   ├── gtc-symbol-white.svg    ← Vector master, white on dark
│   ├── gtc-wordmark-black.svg  ← Symbol + "GTC" lockup
│   ├── gtc-wordmark-white.svg
│   └── GTC-Symbol.otf          ← Font glyph (Unicode PUA codepoint)
├── manifests/
│   ├── glasswing-001.json      ← Retroactive manifests for The Glasswing
│   ├── ...                        Translator publication archive
│   └── glasswing-014.json
├── tools/
│   ├── index.html              ← GTC Tools: Verify, Create Manifest,
│   │                              Social Post Credentialer, Site Audit,
│   │                              Image Verifier
│   │                              (GitHub Pages, runs entirely in-browser)
│   └── lib/                    ← Self-hosted dependencies (no CDN calls)
│       ├── exif.js             ← XMP/EXIF parsing (exif-js)
│       └── c2pa-web/           ← C2PA manifest reading (WASM, runs in a Worker)
│           ├── index.js
│           ├── c2pa-CL7pqSPf.js
│           ├── c2pa_worker.js
│           ├── highgain.js
│           └── resources/
│               └── c2pa_bg.wasm
├── verify/
│   └── index.html              ← Redirect to tools/ (legacy URL support)
└── examples/
    ├── example-manifest.json   ← Annotated example manifest
    └── example-image.png       ← Sample image with GTC mark placed
```

---

## Proof of Concept

The [Glasswing Translator](https://www.cognitivefusion.systems) is an AI governance publication written using Anthropic's Claude. Every issue since Issue 001 has disclosed AI involvement and structural conflicts in prose. GTC formalizes those disclosures into structured, hashable, verifiable manifests.

The `manifests/` directory contains retroactive records for every published Glasswing issue — a verifiable disclosure chain back to Issue 001. Git commit timestamps prove these disclosures were formalized. The chain is auditable by anyone.

---

## Recursive Disclosure

This protocol was developed using Anthropic's Claude. The manifest for the Glasswing issue announcing GTC uses the GTC protocol on itself.

This recursion is not a flaw — it is the protocol's first test case. A deployer-side disclosure system that cannot disclose its own AI involvement would undermine its own premise. See the manifest's `disclosures` array for the specific conflict declaration.

---

## Related Standards

- [C2PA Technical Specification](https://c2pa.org/specifications/) — Provider-side content provenance
- [Content Authenticity Initiative](https://contentauthenticity.org) — Adobe-led adoption of C2PA
- [EU AI Act Article 50](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — Transparency obligations for AI-generated content
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) — AI governance framework
- [MITRE ATLAS](https://atlas.mitre.org) — Adversarial threat landscape for AI systems

---

## Adoption

If you use GTC on your content, open an issue. This repo tracks real-world use.

---

## Author

**Dee Jared Johnson** — AI governance architect, [Cognitive Fusion](https://www.cognitivefusion.systems). Founder and CAIO. Contributor to MITRE ATLAS. Claude Partner Network member.

## License

[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). Free to use, adapt, and build on with attribution.
