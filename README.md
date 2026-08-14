# Ground Truth Credential (GTC)

**A deployer-side disclosure protocol for AI-assisted content.**

---

## The Problem

Provider-side provenance is solved. C2PA Content Credentials tell you what tool made something. Anthropic's watermarks prove Claude processed it. EU AI Act Article 50 (enforceable August 2, 2026) mandates machine-readable marks on AI-generated content. LinkedIn displays the CR badge on images carrying Content Credentials.

None of these answer the deployer-side question: **what structural conditions existed when this content was produced, and what conflicts exist between the tool, the subject, and the author?**

A watermark says "AI was here." A Content Credential says "this tool made it." Neither says "the person who published this has a financial relationship with the AI provider they're writing about," or "the model used to draft this analysis was built by the company the analysis critiques."

That layer doesn't exist yet. GTC fills it.

## What GTC Is

GTC is an open protocol for deployer-side disclosure of AI-assisted content. It consists of:

1. **A visual mark** — a designed glyph that signals "this content carries a structured disclosure chain." The mark sits alongside provider-side marks (CR badge, watermarks), not instead of them.

2. **A JSON disclosure manifest** — a structured, hashable document containing: the AI system used, the provider, the subject relationship, the author's structural conflicts, a content hash, a timestamp, and a verification URL. See [`schema/gtc-manifest-v0.1.json`](schema/gtc-manifest-v0.1.json).

3. **A verifiable chain** — manifests are stored in this public repository, hashed (SHA-256), and committed. Git commit timestamps provide immutable provenance. The chain cannot be fabricated after the fact.

4. **A usage specification** — placement rules, color rules, sizing minimums, co-placement with legal and provenance marks. See [`SPECIFICATION.md`](SPECIFICATION.md).

## What GTC Is Not

- **Not a replacement for C2PA / Content Credentials.** GTC is a companion protocol. C2PA handles provider-side provenance. GTC handles deployer-side disclosure. They are complementary.
- **Not a watermark.** GTC is a visible, voluntary disclosure mark — not an invisible, embedded signal.
- **Not a certification or endorsement.** The mark indicates that a disclosure manifest exists and is verifiable. It does not certify that the content is true, accurate, or endorsed by any entity.
- **Not DRM or copyright protection.**

## The Gap

| Layer | What It Answers | Who Provides It | Status |
|---|---|---|---|
| **C2PA / Content Credentials** | What tool made this? | Provider (Adobe, camera OEMs) | Mature for images; text is least tested |
| **AI watermarks** (Anthropic, Google SynthID) | Did an AI model process this? | Provider (Anthropic, Google) | Active; text watermarks launching 2026 |
| **EU AI Act Article 50** | Is AI-generated content machine-readably marked? | Provider obligation, regulatory mandate | Enforceable August 2, 2026 |
| **GTC (this protocol)** | What conflicts exist between the tool, the subject, and the author? | **Deployer** | **This project** |

## Quick Start

### Reading a GTC manifest

Every piece of GTC-marked content links to a manifest in this repository. To verify:

1. Find the GTC mark on the content (corner of image, end of text block, document footer).
2. Follow the verification link or locate the manifest file in [`manifests/`](manifests/).
3. Check the `content_hash` field against the content's SHA-256 hash.
4. Read the `disclosures` array for the deployer's structural conflict declarations.

### Creating a GTC manifest

See the [annotated example](examples/example-manifest.json) and the [JSON schema](schema/gtc-manifest-v0.1.json).

1. Write your content.
2. Generate the SHA-256 hash of the final published version.
3. Fill in the manifest fields (see schema).
4. Commit the manifest to your repository or publish it at a stable URL.
5. Place the GTC mark on your content per the [usage specification](SPECIFICATION.md).

## Repository Structure

```
ground-truth-credential/
├── README.md                    ← You are here
├── LICENSE                      ← CC BY 4.0
├── SPECIFICATION.md             ← Mark usage specification
├── schema/
│   └── gtc-manifest-v0.1.json  ← JSON Schema for disclosure manifests
├── mark/
│   ├── gtc-symbol-black.svg    ← Vector master, black on light
│   ├── gtc-symbol-white.svg    ← Vector master, white on dark
│   ├── gtc-wordmark-black.svg  ← Symbol + "GTC" lockup
│   ├── gtc-wordmark-white.svg
│   └── GTC-Symbol.otf          ← Font glyph (Unicode PUA)
├── manifests/
│   ├── glasswing-001.json      ← Retroactive manifests for The Glasswing
│   ├── ...                        Translator publication archive
│   └── glasswing-0NN.json
├── verify/
│   └── index.html              ← GitHub Pages verification lookup
└── examples/
    ├── example-manifest.json   ← Annotated example manifest
    └── example-image.png       ← Sample image with GTC mark
```

## Proof of Concept

The [Glasswing Translator](https://www.cognitivefusion.systems) is an AI governance publication written using Anthropic's Claude. Every issue since Issue 001 has disclosed the AI involvement and structural conflicts in prose. GTC formalizes those disclosures into structured, hashable, verifiable manifests.

The [`manifests/`](manifests/) directory contains retroactive manifests for every published Glasswing issue, creating a verifiable disclosure chain back to Issue 001. Git commit timestamps prove these disclosures were formalized — the chain is auditable by anyone.

## Recursive Disclosure

This protocol was developed using Anthropic's Claude. The manifest for the Glasswing issue announcing GTC uses the GTC protocol on itself. This recursion is not a flaw — it is the protocol's first test case. A deployer-side disclosure protocol that cannot disclose its own AI involvement would undermine its premise.

See the manifest's `disclosures` array for the specific conflict declaration.

## Related Standards and Frameworks

- [C2PA Technical Specification](https://c2pa.org/specifications/) — Provider-side content provenance
- [Content Authenticity Initiative](https://contentauthenticity.org) — Adobe-led adoption of C2PA
- [EU AI Act Article 50](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — Transparency obligations for AI-generated content
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) — AI governance framework
- [MITRE ATLAS](https://atlas.mitre.org) — Adversarial threat landscape for AI systems

## Author

**Dee Jared Johnson** — AI governance architect, [Cognitive Fusion](https://www.cognitivefusion.systems). Founder and CAIO (Chief AI Intelligence Officer). Contributor to MITRE ATLAS. Claude Partner Network member.

## License

This work is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You are free to share and adapt this material for any purpose, including commercial use, provided you give appropriate attribution.
