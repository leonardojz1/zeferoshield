# ZeferoShield

> **Protecting every digital citizen with transparent, community-driven cybersecurity tools.**

[![Patent Pending](https://img.shields.io/badge/Patent-Pending%20%2364%2F015%2C161-blue?style=flat-square)](https://uspto.gov)
[![License: Open Core](https://img.shields.io/badge/License-Open%20Core-green?style=flat-square)](#license)
[![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows-lightgrey?style=flat-square)](#)
[![Status](https://img.shields.io/badge/Status-Beta%20Sprint-orange?style=flat-square)](#roadmap)
[![Blockchain](https://img.shields.io/badge/Blockchain-Polygon%20PoS-purple?style=flat-square)](#)

ZeferoShield is an open-core cybersecurity ecosystem built for the 99% of users that enterprise security vendors ignore — home users, freelancers, and small businesses with no security budget or staff. It combines a community-governed vulnerability scanner, blockchain-anchored signature integrity, privacy-preserving edge AI inference, and a token-incentivized contributor economy into a single unified platform.

---

## The Problem

Enterprise security tools cost thousands per year, require dedicated staff to operate, and are designed for Fortune 500 companies. Meanwhile:

- **Home users** run unpatched software for years without knowing it
- **Small businesses** get ransomwared because they cannot afford Qualys or Nessus
- **Community contributors** get no recognition or reward for security research
- **No free scanner** can prove its signatures have not been tampered with

ZeferoShield addresses all four problems simultaneously.

---

## Product Suite

| Product | Description | Model |
|---------|-------------|-------|
| **ZeferoX** | Lightweight CLI vulnerability scanner — single binary, no root, offline-capable | Open Source |
| **ZeferoDay** | Community-curated threat feed with on-chain governance | Open Source |
| **ZeferoGuard** | Unified GUI dashboard managing all ZeferoShield tools | Open Core |
| **ZeferoZero** | Privacy tool and WireGuard VPN with cryptographic session isolation | Commercial |
| **ZeferoPulse** | Real-time log aggregation with edge AI anomaly detection | Commercial |
| **ZeferoVault** | Encrypted secrets vault for credentials and API tokens | Commercial |
| **ZeferoAttack** | Ethical red-team framework for payload simulation | Commercial |

---

## What Makes ZeferoShield Different

### 1. Blockchain-Anchored Signature Integrity
Every vulnerability detection rule is cryptographically anchored to the Polygon PoS blockchain. Users can independently verify that the signatures they are scanning with have not been tampered with — by anyone, including ZeferoShield. No other free scanner offers this.

```
User downloads signature bundle
         ↓
SHA-256 hash computed locally
         ↓
Hash verified against Polygon smart contract via eth_call
         ↓
Bundle accepted only if hashes match (constant-time comparison)
```

### 2. Community DAO Governance
Detection rules are submitted, reviewed, and approved through a Decentralized Autonomous Organization (DAO) with stake-weighted anonymous council voting. Every vote is permanently recorded on-chain. Contributors earn ZFR utility tokens for accepted rules — creating a self-sustaining security research flywheel.

### 3. Privacy-Preserving Edge AI
The ZeferoPulse anomaly detection layer runs entirely on a local hardware neural processing unit (Hailo-10H NPU, 40 TOPS). Raw system telemetry never leaves the device. When AI-generated remediation guidance is requested, only publicly-known CVE identifiers are transmitted to external APIs — never file paths, package versions, or system metadata.

### 4. Cryptographically Isolated VPN
ZeferoZero uses a three-phase protocol to issue blinded HMAC session tokens that contain no subscriber identity. The exit node cannot identify the subscriber. WireGuard Pre-Shared Keys are derived via HKDF per session. No on-chain transaction dependency for tunnel establishment.

---

## ZeferoX — Open Source Scanner

ZeferoX is the flagship open-source component. A single compiled binary under 15MB that:

- Requires **no root privileges**
- Performs **no network calls during scanning** (fully offline-capable)
- Applies a **Linux seccomp sandbox** (35-syscall allowlist) during execution
- Outputs **SARIF 2.1.0**, JSON, and human-readable table formats
- Verifies **binary self-integrity** at startup
- Writes an **append-only audit log** for forensic accountability

```bash
# Install
curl -fsSL https://get.zeferoshield.com/install.sh | sh

# Scan
zeferox scan /usr/local --output table

# Explain a finding
zeferox explain CVE-2021-44228
```

---

## Technology Stack

### Core Scanner
| Component | Technology | Purpose |
|-----------|-----------|---------|
| Scanner engine | Rust 1.78 (MSRV 1.75) | Zero unsafe code, seccomp sandbox |
| Cryptography | ring 0.17 | SHA-256, Ed25519, HKDF, constant-time |
| Rule engine | regex (linear-time) | ReDoS-safe pattern matching |
| Output formats | SARIF 2.1.0, JSON, table | CI/CD and human-readable |

### Blockchain Layer
| Component | Technology | Purpose |
|-----------|-----------|---------|
| Smart contracts | Solidity, Polygon PoS | Signature bundle integrity anchoring |
| Token standard | ERC-20 (non-transferable) | ZFR contributor rewards |
| DAO governance | On-chain voting | Rule submission and approval |
| RPC | Alchemy (Polygon) | eth_call verification |

### AI Layer
| Component | Technology | Purpose |
|-----------|-----------|---------|
| Edge inference | Hailo-10H NPU (40 TOPS) | Local anomaly detection |
| AI runtime | HailoRT 5.x | NPU firmware and inference API |
| Privacy boundary | contextBridge whitelist | Prevents system data reaching cloud |
| LLM integration | Claude API (CVE IDs only) | Remediation guidance generation |

### Infrastructure
| Component | Technology | Purpose |
|-----------|-----------|---------|
| CI/CD | GitHub Actions | 4-platform build matrix |
| Binary signing | Cosign / Sigstore Fulcio | Keyless signed releases |
| CDN | Cloudflare R2 | Signature bundle distribution |
| GUI framework | Electron + gRPC | ZeferoGuard desktop app |

---

## Hardware Reference Implementation

ZeferoShield is developed and tested on:

```
Raspberry Pi 5 Model B (8GB)
├── Raspberry Pi AI HAT+ 2 (Hailo-10H, 40 TOPS)
│   └── Connected via PCIe FPC ribbon cable
├── Freenove Dual M.2 NVMe HAT
│   ├── NVME1: Samsung PM991 128GB (boot + development)
│   └── NVME2: Available for ZeferoPulse data
└── OS: Raspberry Pi OS 64-bit (Debian 13 Trixie)
    └── Kernel: 6.12.x
```

This configuration demonstrates ZeferoShield's core design principle: enterprise-grade security capabilities on affordable, accessible hardware.

---

## Security Architecture

ZeferoShield was built with a formal STRIDE threat model covering 57 documented findings across all products:

```
30 HIGH severity   — all mitigated
23 MEDIUM severity — all mitigated  
4  LOW severity    — all mitigated
```

Key security decisions:
- **No unsafe Rust** in the scanner engine (`#![deny(unsafe_code)]`)
- **Secrets-aware redaction** — AWS keys, JWT tokens, PEM headers stripped from all output
- **Symlink rejection** — output paths validated before any write operation
- **Constant-time comparisons** — all cryptographic comparisons use `ring::constant_time`
- **Atomic file writes** — all output written to temp file then renamed (no partial writes)

---

## ZFR Token

ZFR is a non-monetary ERC-20 utility token deployed on Polygon PoS. It is not a financial instrument, carries no monetary value, and cannot be purchased.

| Action | ZFR Earned |
|--------|-----------|
| Detection rule merged | 10 ZFR |
| Bug report verified | 50 ZFR |
| Feature PR merged | 100 ZFR |
| Security disclosure | 150 ZFR |
| Council ballot | 1 ZFR |

Token balance determines governance vote weight: `vote_weight = floor(sqrt(zfr_balance))`

---

## Roadmap

### Phase 1 — Foundation (Active)
- [x] Provisional patent filed — Application #64/015,161 (March 24, 2026)
- [x] STRIDE threat model — 57 findings documented
- [x] ZeferoX Rust architecture — 27 source files complete
- [x] ZeferoGuard Electron skeleton — Sprint 1 + Sprint 2
- [x] Cryptographic handshake spec — all 5 open questions resolved
- [ ] ZeferoX beta binary — compile fixes in progress
- [ ] 10 real CVE rules with zero false positives
- [ ] Public beta launch

### Phase 2 — Expansion (Months 6–12)
- [ ] ZeferoZero VPN launch
- [ ] ZFR ERC-20 deployed on Polygon mainnet
- [ ] Polygon correction-block contract live
- [ ] ZeferoPulse Hailo-10H anomaly detection

### Phase 3 — Scaling (Months 12–18)
- [ ] ZeferoVault secure vault
- [ ] ZeferoAttack red-team toolkit
- [ ] DAO test vote
- [ ] Full suite public launch

---

## Patent

ZeferoShield's core architecture is protected under a pending US utility patent:

**Application #64/015,161** — Filed March 24, 2026  
*System and Method for Community-Governed, Blockchain-Verified Vulnerability Detection with Token-Incentivized Rule Curation and Privacy-Preserving Edge Inference*

The patent covers the novel combination of: blockchain-anchored signature integrity verification, DAO governance of detection rules, token-incentivized open-source contribution, privacy-preserving edge AI inference, and cryptographically isolated VPN session authorization.

---

## License

| Component | License |
|-----------|---------|
| ZeferoX (scanner) | MIT |
| ZeferoDay (threat feed) | MIT |
| ZeferoGuard (GUI) | Source-available, free for personal use |
| ZeferoZero (VPN) | Commercial |
| ZeferoPulse (telemetry) | Commercial |
| ZeferoVault (secrets) | Commercial |
| ZeferoAttack (red team) | Commercial |

---

## Contributing

ZeferoShield's open-source components welcome contributions. Contributors earn ZFR tokens for accepted work.

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

**Built by the community. Protected by Zefero.**

---

*Patent Pending — Application #64/015,161 · © 2026 Jose Z / ZeferoShield*
