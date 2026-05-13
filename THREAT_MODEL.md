# Threat Model — carin1122345

## Overview

This document captures the threat model for the **carin1122345** project.  
It follows the **STRIDE** methodology (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege) and is intended to be a living document, updated as the architecture and codebase evolve.

> **Current State**: The repository is in its early stages with minimal code. This document serves as a baseline framework that should be refined once the system architecture, data flows, and trust boundaries are defined.

---

## 1. Scope

- **In scope**: All application code, configurations, CI/CD pipelines, secrets management, third-party dependencies, and the runtime environment.
- **Out of scope**: Physical security of hosting infrastructure (unless self-managed), social-engineering attacks against end users (unless the application handles authentication/authorization).

---

## 2. Trust Boundaries

_Placeholder — to be populated once architecture is defined._

| Boundary | Description | Data Crossing |
|----------|-------------|---------------|
| _TBD_   | _TBD_       | _TBD_         |

---

## 3. Asset Inventory

_Placeholder — to be populated once the system is designed._

| Asset | Sensitivity | Storage Location | Access Requirements |
|-------|-------------|------------------|---------------------|
| _TBD_ | _TBD_       | _TBD_            | _TBD_               |

---

## 4. Threat Catalogue (STRIDE)

### 4.1 Spoofing

| # | Threat | Affected Component | Likelihood | Impact | Mitigation |
|---|--------|--------------------|------------|--------|------------|
| S1 | Weak or missing authentication allows identity forgery | All endpoints | Medium | High | Enforce strong authentication (MFA where possible); use OAuth 2.0 / OIDC; validate tokens on every request. |
| S2 | Hard-coded credentials in source code or config | Codebase / CI | Medium | Critical | Use a secrets manager (e.g., HashiCorp Vault, AWS Secrets Manager, GitHub Secrets); never commit secrets. |

### 4.2 Tampering

| # | Threat | Affected Component | Likelihood | Impact | Mitigation |
|---|--------|--------------------|------------|--------|------------|
| T1 | Unsigned or unverified dependencies (supply-chain attack) | Build pipeline | Medium | Critical | Pin dependency versions; verify checksums/signatures; use SBOM tooling (e.g., `syft`, `grype`); enable Dependabot/Renovate. |
| T2 | Insecure deserialization leading to code execution | Data layer | Low | Critical | Avoid native deserialization of untrusted data; use safe serialization formats (JSON Schema validation, Protobuf). |
| T3 | CI/CD pipeline injection | GitHub Actions / CI | Low | High | Restrict workflow triggers; require approvals for external contributions; pin action versions by SHA. |

### 4.3 Repudiation

| # | Threat | Affected Component | Likelihood | Impact | Mitigation |
|---|--------|--------------------|------------|--------|------------|
| R1 | Lack of audit logging prevents incident investigation | All services | High | Medium | Emit structured, immutable audit logs for all security-relevant events (login, privilege change, data mutation). |
| R2 | Log tampering by an attacker with elevated access | Logging infra | Low | High | Forward logs to a tamper-evident append-only store; use log integrity hashes. |

### 4.4 Information Disclosure

| # | Threat | Affected Component | Likelihood | Impact | Mitigation |
|---|--------|--------------------|------------|--------|------------|
| I1 | Sensitive data exposed in error messages or stack traces | API layer | Medium | High | Sanitise error responses in production; return generic error messages to clients; log details server-side only. |
| I2 | Unencrypted data in transit | Network | Medium | High | Enforce TLS 1.2+ everywhere; use HSTS; reject plaintext connections. |
| I3 | Unencrypted data at rest | Databases / Storage | Medium | Critical | Encrypt databases, backups, and file stores; manage encryption keys separately from data. |
| I4 | Overly permissive CORS configuration | API | Medium | Medium | Restrict `Access-Control-Allow-Origin` to known domains; never use `*` with credentials. |

### 4.5 Denial of Service (DoS)

| # | Threat | Affected Component | Likelihood | Impact | Mitigation |
|---|--------|--------------------|------------|--------|------------|
| D1 | Resource exhaustion via unbounded requests | API / Backend | High | High | Enforce rate limiting; set request size/timeout caps; implement circuit breakers. |
| D2 | Expensive queries or N+1 database patterns | Data layer | Medium | Medium | Paginate all list endpoints; use query complexity analysis; monitor slow queries. |
| D3 | Dependency confusion / namespace squatting | Package registry | Low | High | Scope internal packages; configure registry mirrors with priorities. |

### 4.6 Elevation of Privilege

| # | Threat | Affected Component | Likelihood | Impact | Mitigation |
|---|--------|--------------------|------------|--------|------------|
| E1 | Broken access control (IDOR, missing authorisation checks) | API / Backend | High | Critical | Enforce attribute-based or role-based access control on every endpoint; deny by default; unit-test access control logic. |
| E2 | Privilege escalation via misconfigured IAM roles | Cloud / Infra | Medium | Critical | Apply least-privilege principle; regularly review IAM policies; use policy-as-code tooling. |
| E3 | JWT algorithm confusion (`alg: none` / HMAC vs RSA) | Auth service | Low | Critical | Whitelist accepted algorithms; validate `alg` field explicitly; use a well-audited JWT library. |

---

## 5. Data Flow Diagrams (DFD)

_Placeholder — to be created once architecture is designed._

- **Level 0 (Context)**: _TBD_
- **Level 1 (System)**: _TBD_

---

## 6. Risk Scoring Matrix

| Likelihood →<br>↓ Impact | Low | Medium | High |
|--------------------------|-----|--------|------|
| **Critical** | Medium | High | **Critical** |
| **High**     | Low   | Medium | High |
| **Medium**   | Low   | Low    | Medium |
| **Low**      | Low   | Low    | Low |

Risks rated **Critical** or **High** must have mitigations implemented before production release.

---

## 7. Secure Development Lifecycle Integration

| Phase | Security Activity |
|-------|-------------------|
| **Design** | Threat modelling session; architecture review; define trust boundaries. |
| **Develop** | SAST linting in IDE; pre-commit hooks (`detect-secrets`, `trufflehog`); peer code review with security checklist. |
| **Build** | SCA (dependency scanning); container image scanning; signed commits. |
| **Test** | DAST (dynamic analysis); penetration testing; fuzz testing; unit tests for access control. |
| **Deploy** | Immutable infrastructure; blue/green or canary deployments; secrets injection at runtime. |
| **Operate** | Continuous monitoring (SIEM); vulnerability disclosure program; regular dependency updates; incident response plan. |

---

## 8. Review Cadence

- **Trigger-based**: On any major architectural change, new feature, or security incident.
- **Scheduled**: Quarterly review of the threat model and risk register.

---

## 9. References

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [STRIDE Methodology — Microsoft](https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats)
- [NIST SP 800-53](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)
- [CWE Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/)

---

_Last updated: 2025-06-20 — Initial baseline template._