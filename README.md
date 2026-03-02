# Tanzanian Law MCP Server

**The Tanzania Laws alternative for the AI age.**

[![npm version](https://badge.fury.io/js/@ansvar%2Ftanzanian-law-mcp.svg)](https://www.npmjs.com/package/@ansvar/tanzanian-law-mcp)
[![MCP Registry](https://img.shields.io/badge/MCP-Registry-blue)](https://registry.modelcontextprotocol.io)
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![GitHub stars](https://img.shields.io/github/stars/Ansvar-Systems/Tanzanian-law-mcp?style=social)](https://github.com/Ansvar-Systems/Tanzanian-law-mcp)
[![CI](https://github.com/Ansvar-Systems/Tanzanian-law-mcp/actions/workflows/ci.yml/badge.svg)](https://github.com/Ansvar-Systems/Tanzanian-law-mcp/actions/workflows/ci.yml)
[![Provisions](https://img.shields.io/badge/provisions-32%2C865-blue)](https://github.com/Ansvar-Systems/Tanzanian-law-mcp)

Query **3,562 Tanzanian laws** -- from the Personal Data Protection Act 2022 and the Penal Code to the Employment and Labour Relations Act, Companies Act, and more -- directly from Claude, Cursor, or any MCP-compatible client.

If you're building legal tech, compliance tools, or doing Tanzanian legal research, this is your verified reference database.

Built by [Ansvar Systems](https://ansvar.eu) -- Stockholm, Sweden

---

## Why This Exists

Tanzanian legal research means navigating tanzlii.org and parliament.go.tz across a complex federation of Acts, Subsidiary Legislation, and regional instruments. Whether you're:
- A **lawyer** validating citations in a brief or contract
- A **compliance officer** checking obligations under the PDPA 2022 or the Electronic and Postal Communications Act
- A **legal tech developer** building tools on Tanzanian law
- A **researcher** tracing legislative history across 3,562 Acts

...you shouldn't need dozens of browser tabs and manual cross-referencing. Ask Claude. Get the exact provision. With context.

This MCP server makes Tanzanian law **searchable, cross-referenceable, and AI-readable**.

---

## Quick Start

### Use Remotely (No Install Needed)

> Connect directly to the hosted version -- zero dependencies, nothing to install.

**Endpoint:** `https://tanzanian-law-mcp.vercel.app/mcp`

| Client | How to Connect |
|--------|---------------|
| **Claude.ai** | Settings > Connectors > Add Integration > paste URL |
| **Claude Code** | `claude mcp add tanzanian-law --transport http https://tanzanian-law-mcp.vercel.app/mcp` |
| **Claude Desktop** | Add to config (see below) |
| **GitHub Copilot** | Add to VS Code settings (see below) |

**Claude Desktop** -- add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "tanzanian-law": {
      "type": "url",
      "url": "https://tanzanian-law-mcp.vercel.app/mcp"
    }
  }
}
```

**GitHub Copilot** -- add to VS Code `settings.json`:

```json
{
  "github.copilot.chat.mcp.servers": {
    "tanzanian-law": {
      "type": "http",
      "url": "https://tanzanian-law-mcp.vercel.app/mcp"
    }
  }
}
```

### Use Locally (npm)

```bash
npx @ansvar/tanzanian-law-mcp
```

**Claude Desktop** -- add to `claude_desktop_config.json`:

**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "tanzanian-law": {
      "command": "npx",
      "args": ["-y", "@ansvar/tanzanian-law-mcp"]
    }
  }
}
```

**Cursor / VS Code:**

```json
{
  "mcp.servers": {
    "tanzanian-law": {
      "command": "npx",
      "args": ["-y", "@ansvar/tanzanian-law-mcp"]
    }
  }
}
```

---

## Example Queries

Once connected, just ask naturally:

- *"What does the Personal Data Protection Act 2022 say about consent?"*
- *"Find provisions in the Penal Code about cybercrime offences"*
- *"Search for employment law under the Employment and Labour Relations Act"*
- *"Is the Electronic and Postal Communications Act still in force?"*
- *"What does the Companies Act say about director duties?"*
- *"Find provisions about land rights under the Land Act"*
- *"Validate the citation 'Section 15 Personal Data Protection Act 2022'"*
- *"Build a legal stance on data breach notification requirements in Tanzania"*

---

## What's Included

| Category | Count | Details |
|----------|-------|---------|
| **Statutes** | 3,562 laws | Comprehensive Tanzanian legislation from tanzlii.org |
| **Provisions** | 32,865 sections | Full-text searchable with FTS5 |
| **Database Size** | ~55 MB | Optimized SQLite, portable |
| **Legal Definitions** | Table reserved | Extraction planned for upcoming release |
| **Freshness Checks** | Automated | Drift detection against official sources |

**Verified data only** -- every citation is validated against official sources (Tanzania Laws, Parliament of Tanzania). Zero LLM-generated content.

---

## Why This Works

**Verbatim Source Text (No LLM Processing):**
- All statute text is ingested from [tanzlii.org](https://tanzlii.org) and [parliament.go.tz](https://parliament.go.tz)
- Provisions are returned **unchanged** from SQLite FTS5 database rows
- Zero LLM summarization or paraphrasing -- the database contains statute text, not AI interpretations

**Smart Context Management:**
- Search returns ranked provisions with BM25 scoring (safe for context)
- Provision retrieval gives exact text by Act identifier + section number
- Cross-references help navigate without loading everything at once

**Technical Architecture:**
```
tanzlii.org / parliament.go.tz --> Parse --> SQLite --> FTS5 snippet() --> MCP response
                                     ^                        ^
                              Provision parser         Verbatim database query
```

### Traditional Research vs. This MCP

| Traditional Approach | This MCP Server |
|---------------------|-----------------|
| Search tanzlii.org by Act name | Search by plain English: *"personal data consent"* |
| Navigate multi-section statutes manually | Get the exact provision with context |
| Manual cross-referencing between Acts | `build_legal_stance` aggregates across sources |
| "Is this Act still in force?" -- check manually | `check_currency` tool -- answer in seconds |
| Find EAC/AU alignment -- search manually | `get_eu_basis` -- linked frameworks instantly |
| No API, no integration | MCP protocol -- AI-native |

**Traditional:** Search tanzlii.org -> Navigate HTML -> Ctrl+F -> Cross-reference between Acts -> Repeat

**This MCP:** *"What are the data breach notification requirements under the Personal Data Protection Act 2022?"* -> Done.

---

## Available Tools (13)

### Core Legal Research Tools (8)

| Tool | Description |
|------|-------------|
| `search_legislation` | FTS5 full-text search across 32,865 provisions with BM25 ranking. Supports quoted phrases, boolean operators, prefix wildcards |
| `get_provision` | Retrieve specific provision by Act identifier + section number |
| `check_currency` | Check if a statute is in force, amended, or repealed |
| `validate_citation` | Validate citation against database -- zero-hallucination check |
| `build_legal_stance` | Aggregate citations from multiple statutes for a legal topic |
| `format_citation` | Format citations per Tanzanian legal conventions |
| `list_sources` | List all available statutes with metadata, coverage scope, and data provenance |
| `about` | Server info, capabilities, dataset statistics, and coverage summary |

### International Law Integration Tools (5)

| Tool | Description |
|------|-------------|
| `get_eu_basis` | Get EU directives/regulations that a Tanzanian statute aligns with (e.g., PDPA 2022 and GDPR principles) |
| `get_tanzanian_implementations` | Find Tanzanian laws aligning with a specific international framework |
| `search_eu_implementations` | Search EU documents with Tanzanian alignment counts |
| `get_provision_eu_basis` | Get international law references for a specific provision |
| `validate_eu_compliance` | Check alignment status of Tanzanian statutes against EU/international frameworks |

---

## International Law Alignment

Tanzania is not an EU member state. The international alignment tools cover the frameworks that matter for Tanzanian law practice:

- **EAC frameworks** -- East African Community treaty obligations and harmonisation instruments
- **African Union** -- AU conventions including the Malabo Convention on Data Protection
- **Commonwealth** -- Commonwealth legal frameworks and model laws
- **Personal Data Protection Act 2022** aligns with international data protection principles; the `get_eu_basis` tool maps these to GDPR-equivalent provisions for cross-reference
- **Employment and Labour Relations Act** aligns with ILO conventions and EAC labour protocols

The international bridge tools allow you to explore alignment relationships -- checking which Tanzanian provisions correspond to EAC or international requirements, and vice versa.

> **Note:** International cross-references reflect alignment and treaty obligations, not formal transposition. Tanzania adopts its own legislative approach, and these tools help identify where Tanzanian and international law address similar domains.

---

## Data Sources & Freshness

All content is sourced from authoritative Tanzanian legal databases:

- **[Tanzania Legal Information Institute (TanzLII)](https://tanzlii.org/)** -- Primary source for Tanzanian legislation
- **[Parliament of Tanzania](https://parliament.go.tz/)** -- Official parliamentary records and Acts

### Data Provenance

| Field | Value |
|-------|-------|
| **Authority** | Tanzania Laws, Parliament of Tanzania |
| **Primary language** | English (Swahili for some subsidiary instruments) |
| **License** | Public domain (government publications) |
| **Coverage** | 3,562 Tanzanian Acts and statutory instruments |
| **Last ingested** | 2026-02-28 |

### Automated Freshness Checks

A [GitHub Actions workflow](.github/workflows/check-updates.yml) monitors Tanzanian legal sources for changes:

| Check | Method |
|-------|--------|
| **Statute amendments** | Drift detection against known provision anchors |
| **New statutes** | Comparison against tanzlii.org index |
| **Repealed statutes** | Status change detection |

**Verified data only** -- every citation is validated against official sources. Zero LLM-generated content.

---

## Security

This project uses multiple layers of automated security scanning:

| Scanner | What It Does | Schedule |
|---------|-------------|----------|
| **CodeQL** | Static analysis for security vulnerabilities | Weekly + PRs |
| **Semgrep** | SAST scanning (OWASP top 10, secrets, TypeScript) | Every push |
| **Gitleaks** | Secret detection across git history | Every push |
| **Trivy** | CVE scanning on filesystem and npm dependencies | Daily |
| **Socket.dev** | Supply chain attack detection | PRs |
| **Dependabot** | Automated dependency updates | Weekly |

See [SECURITY.md](SECURITY.md) for the full policy and vulnerability reporting.

---

## Important Disclaimers

### Legal Advice

> **THIS TOOL IS NOT LEGAL ADVICE**
>
> Statute text is sourced from Tanzania Laws and TanzLII official sources. However:
> - This is a **research tool**, not a substitute for professional legal counsel
> - **Court case coverage is not included** -- do not rely solely on this for case law research
> - **Verify critical citations** against primary sources before court filings
> - **International cross-references** reflect alignment relationships, not formal transposition
> - **Zanzibar legislation** may have separate applicability -- verify jurisdiction carefully

**Before using professionally, read:** [DISCLAIMER.md](DISCLAIMER.md) | [SECURITY.md](SECURITY.md)

### Client Confidentiality

Queries go through the Claude API. For privileged or confidential matters, use on-premise deployment.

### Bar Association Reference

For professional use, consult the **Tanganyika Law Society (TLS)** guidelines on AI-assisted legal research.

---

## Development

### Setup

```bash
git clone https://github.com/Ansvar-Systems/Tanzanian-law-mcp
cd Tanzanian-law-mcp
npm install
npm run build
npm test
```

### Running Locally

```bash
npm run dev                                       # Start MCP server
npx @anthropic/mcp-inspector node dist/index.js   # Test with MCP Inspector
```

### Data Management

```bash
npm run ingest              # Ingest statutes from tanzlii.org
npm run build:db            # Rebuild SQLite database
npm run drift:detect        # Run drift detection against anchors
npm run check-updates       # Check for source updates
npm run census              # Generate coverage census
```

### Performance

- **Search Speed:** <100ms for most FTS5 queries
- **Database Size:** ~55 MB (efficient, portable)
- **Reliability:** 100% ingestion success rate across 3,562 laws

---

## Related Projects: Complete Compliance Suite

This server is part of **Ansvar's Compliance Suite** -- MCP servers that work together for end-to-end compliance coverage:

### [@ansvar/eu-regulations-mcp](https://github.com/Ansvar-Systems/EU_compliance_MCP)
**Query 49 EU regulations directly from Claude** -- GDPR, AI Act, DORA, NIS2, MiFID II, eIDAS, and more. Full regulatory text with article-level search. `npx @ansvar/eu-regulations-mcp`

### [@ansvar/us-regulations-mcp](https://github.com/Ansvar-Systems/US_Compliance_MCP)
**Query US federal and state compliance laws** -- HIPAA, CCPA, SOX, GLBA, FERPA, and more. `npx @ansvar/us-regulations-mcp`

### [@ansvar/security-controls-mcp](https://github.com/Ansvar-Systems/security-controls-mcp)
**Query 261 security frameworks** -- ISO 27001, NIST CSF, SOC 2, CIS Controls, SCF, and more. `npx @ansvar/security-controls-mcp`

**80+ national law MCPs** covering Namibia, Uganda, Dominican Republic, Paraguay, Sri Lanka, Kenya, Nigeria, Ghana, South Africa, and more.

---

## Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Priority areas:
- Court case law expansion (Court of Appeal, High Court judgments)
- Zanzibar legislation integration
- EAC treaty cross-references
- Swahili-language provision support
- Historical statute versions and amendment tracking

---

## Roadmap

- [x] Core statute database with FTS5 search
- [x] Full corpus ingestion (3,562 laws, 32,865 provisions)
- [x] International law alignment tools
- [x] Vercel Streamable HTTP deployment
- [x] npm package publication
- [ ] Court case law expansion (Court of Appeal, High Court)
- [ ] Zanzibar legislation
- [ ] Swahili provision text
- [ ] EAC treaty cross-references
- [ ] Historical statute versions

---

## Citation

If you use this MCP server in academic research:

```bibtex
@software{tanzanian_law_mcp_2026,
  author = {Ansvar Systems AB},
  title = {Tanzanian Law MCP Server: AI-Powered Legal Research Tool},
  year = {2026},
  url = {https://github.com/Ansvar-Systems/Tanzanian-law-mcp},
  note = {3,562 Tanzanian laws with 32,865 provisions}
}
```

---

## License

Apache License 2.0. See [LICENSE](./LICENSE) for details.

### Data Licenses

- **Statutes & Legislation:** Tanzania Laws (public domain, government publications)
- **International Metadata:** Public domain

---

## About Ansvar Systems

We build AI-accelerated compliance and legal research tools for the global market. This MCP server makes Tanzanian law accessible to legal professionals and compliance teams worldwide.

So we're open-sourcing it. Navigating 3,562 Acts shouldn't require a law degree.

**[ansvar.eu](https://ansvar.eu)** -- Stockholm, Sweden

---

<p align="center">
  <sub>Built with care in Stockholm, Sweden</sub>
</p>
