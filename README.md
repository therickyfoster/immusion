# Sequential Proof Stacks (SPS)
**Cross-chain immutable archive architecture for compounding, verifiable truth over time.**  
_Status: Draft spec v1.0 • Interface-stable • Implementation-ready_

- 🧱 **Immutable by design**: Content-addressed payloads, manifests, and lineage.
- 🛰️ **Redundant permanence**: Stored across **IPFS** and **Arweave** with periodic multi-chain anchors.
- ⏱️ **Sequential proof**: Every append strengthens history (merkle/vector commits + time-seal bundle).
- 🧭 **Human-literate**: Court-readable records, machine-parsable schemas, offline verification kits.

---

## Table of Contents
- [TL;DR](#tldr)
- [Why SPS](#why-sps)
- [Core Guarantees](#core-guarantees)
- [Architecture Overview](#architecture-overview)
  - [Layers](#layers)
  - [Data Model](#data-model)
- [CLI (reference)](#cli-reference)
- [HTTP API (minimal)](#http-api-minimal)
- [Threat Model & Mitigations](#threat-model--mitigations)
- [Governance & Stewardship](#governance--stewardship)
- [UX & Ops](#ux--ops)
- [Install & Quickstart](#install--quickstart)
- [Offline Verification Kit](#offline-verification-kit)
- [Interoperability Notes](#interoperability-notes)
- [Evidence Handling & Compliance](#evidence-handling--compliance)
- [Performance & Cost Strategy](#performance--cost-strategy)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [Security Disclosure](#security-disclosure)
- [License](#license)
- [Appendix A — Glossary](#appendix-a--glossary)
- [Appendix B — GitHub Topics (slugified)](#appendix-b--github-topics-slugified)
- [Appendix C — One-Liners](#appendix-c--one-liners)
- [Changelog](#changelog)

---

## TL;DR
**SPS** is an append-only archival system that stores payloads and manifests on **IPFS + Arweave** and periodically notarizes sequence/state across **multiple chains**. Over time, anchors + receipts + witness attestations stack into a **Sequential Proof Stack**—a compounding evidence trail resilient to link rot, platform deprecations, and adversarial games.

---

## Why SPS
Today’s records die three deaths: **link rot**, **platform obsolescence**, and **trust decay**. SPS addresses all three:
- **Addressable permanence** without custodial chokepoints.
- **Time-compounded trust**: the older the record, the stronger its corroborations.
- **Legible continuity**: human-readable manifests, machine-verifiable proofs, offline kits.

---

## Core Guarantees
- **Permanence**: Content IDs (CIDs) for IPFS + Arweave TXIDs for redundancy.
- **Sequence Integrity**: Parent linkage (hash chain) + rolling Merkle/Vector commitments.
- **Temporal Attestation**: Multi-source **Time-Seal Bundle (TSB)** (multi-chain anchors + NTP sample + witness sig).
- **Detectable Equivocation**: Forks create divergent accumulators detectable on verification.
- **Sovereign Replication**: No single platform dependency; offline verification supported.

---

## Architecture Overview

### Layers
- **L0 — Content Addressing**: SHA-2/3 or BLAKE3; CIDs; Arweave TXIDs.
- **L1 — Redundant Storage**: Payloads + manifests pushed to **IPFS & Arweave**.
- **L2 — Sequence Ledger**: Append-only log with `parents[]` and TSB.
- **L3 — Cross-Chain Notary**: Anchors checkpoint merkle/vector state on plural L1/L2s.
- **L4 — Proof Orchestrator**: Aggregates receipts into the **Sequential Proof Stack**.
- **L5 — Access/UX**: Readable docs, CLI/API, and offline verifier bundles.

### Data Model
```json
{
  "sps_version": "1.0.0",
  "entry_id": "urn:sps:2025:09:07:000123",
  "parents": ["bafy...prev", "arweave_txid_prev"],
  "payload": {
    "cid_ipfs": "bafy...abc",
    "tx_arweave": "m3y...xyz",
    "bytes": 1048576,
    "mimetype": "application/pdf",
    "sha256": "e3b0c44298..."
  },
  "metadata": {
    "title": "Air Quality Field Log – Fort McMurray",
    "author": "R. Foster",
    "created_utc": "2025-09-07T20:01:05Z",
    "tags": ["proof-of-existence","research","environment"]
  },
  "time_seal_bundle": {
    "sources": [
      {"chain":"ethereum","block":20833452,"tx":"0xabc..."},
      {"chain":"arbitrum","block":26400222,"tx":"0xdef..."}
    ],
    "ntp_sample": ["time.google.com","time.cloudflare.com"],
    "witness_sig": "ed25519:...."
  },
  "accumulators": {
    "merkle_root": "0x9f3...",
    "rolling_bmt": "0x1ab...",
    "vector_commit": "0x77c..."
  }
}
