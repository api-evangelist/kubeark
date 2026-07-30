# Kubeark

Kubeark is an enterprise orchestration and AI automation platform that standardizes system
integration across hybrid estates — workflow automation, prebuilt integration connectors,
and agentic automation that can run on-premise for compliance-sensitive environments.

- Website — https://kubeark.com/
- Documentation — https://docs.kubeark.com/
- Release Notes — https://docs.kubeark.com/release-notes
- Product Lifecycle — https://docs.kubeark.com/product-lifecycle
- GitHub — https://github.com/Kubeark

Backed by: seedcamp

## API surface

Kubeark is **self-hosted software, not a public API provider**. It publishes no public API
host, no OpenAPI/Swagger description, no SDKs, no CLI and no MCP server. Customer instances
run at customer-controlled hostnames (the docs use `yourdomain.kubeark.com` as the
placeholder). The October 2024 release introduced a Go-based internal API Gateway as the
platform entry point, but it is not publicly documented.

Because of that, the spec-grounded artifacts in this pipeline — `openapi/`, `overlays/`,
`errors/`, `scopes/`, `skills/`, `mcp/`, `grpc/`, `sandbox/`, `cli/`, `components/`,
`packages/` — are intentionally **absent rather than fabricated**.

## Artifacts

| Artifact | Method | Notes |
|---|---|---|
| `llms/kubeark-llms.txt` | searched | Verbatim from `https://docs.kubeark.com/llms.txt` (354 KB) |
| `changelog/` | searched | 5 dated Release Notes entries, current version 6.0 |
| `lifecycle/` | searched | Published per-version support table (FS/LS) with end-of-support dates |
| `authentication/` | searched | Kubeark Identity (SAML2/LDAP/OAuth2/OIDC/SCIM) + vault-backed node tokens |
| `conformance/` | searched | Identity/IaC standards; no certifications published |
| `conventions/` | searched | Run modes, timeouts, vaults, variables, audit logging |
| `asyncapi/kubeark-webhooks.yml` | searched | Inbound webhook + trigger catalog; **no AsyncAPI published** |
| `data-model/` | searched | Entity graph read from Concepts & Terminology (no spec to derive from) |
| `security/kubeark-domain-security.yml` | probed | TLS 1.3, no HSTS, no DNSSEC, no CAA, SPF + DMARC (quarantine) |
| `well-known/` | probed | **Nothing present** — see soft-404 note below |

## Probe caveat — kubeark.com soft-404s

`kubeark.com` runs WordPress with a catch-all that returns **HTTP 200 and the full
marketing homepage for every unknown path**. A control probe of `/bogus-xyz123` returned
the same ~227 KB body as `/.well-known/security.txt`, `/llms.txt`, `/pricing/` and
`/privacy/`. Those 200s are **not** real documents. Any future automated probe of this
domain must compare against a control path before recording a hit — the security-program
probe produced a false-positive vulnerability-disclosure hit here that was verified and
discarded. `docs.kubeark.com` returns genuine 404s and can be probed normally.

Real pages verified by distinct body/title: `/terms/`, `/support-policy/`, `/blog/`,
`/contact-us/`, `/success-stories/`. No pricing page, no signup/login, no status page
(`status.kubeark.com` does not resolve), and no separate privacy page — the privacy
statement is a PDF.
