# BabySea Security Reports

Public security reports, red-team summaries, and infrastructure hardening notes for BabySea.

BabySea is the execution control plane for generative media. Because BabySea sits in the execution path for image and video workloads, security is not only about blocking bad requests. It is about preserving the invariants that keep identity, tenancy, credits, webhooks, files, regional boundaries, and execution state correct under pressure.

> The edge is the shield. The invariants are the fortress.

This repository contains sanitized public security artifacts. It is designed for customers, developers, partners, and security reviewers who want to understand how BabySea tests and documents its production security posture without exposing operational secrets or replayable attack payloads.

## Latest public report

| Date       | Report                                                                                  | Type                             | Scope                           | Result                              |
| ---------- | --------------------------------------------------------------------------------------- | -------------------------------- | ------------------------------- | ----------------------------------- |
| 2026-03-13 | [White-Box Red-Team Assessment](./reports/2026-03-13-white-box-red-team-assessment.mdx) | Public sanitized red-team report | US, EU, APAC production regions | 300 attempts, 0 unexpected findings |

Related story:

- [We ran 300 White-Box Attacks against BabySea before launch](https://babysea.ai/blog/we-ran-300-white-box-attacks-against-babysea-before-launch)

## What is in this repo

This repository is for public, sanitized security material.

It may include:

- red-team assessment summaries
- public penetration-test summaries
- security hardening notes
- WAF/API Shield validation summaries
- regional security validation notes
- incident or near-miss postmortems, when appropriate
- sanitized diagrams and evidence tables
- public control-mapping summaries

It does not include:

- live API keys
- account IDs
- generation IDs
- raw exploit scripts
- reusable attack automation
- internal database dumps
- provider secrets
- private infrastructure identifiers
- unredacted logs
- internal-only remediation tickets

Public transparency should improve trust. It should not publish a replay guide.

## Why BabySea publishes security reports

BabySea is infrastructure, not a wrapper.

The platform coordinates production workloads across providers, regions, billing state, webhooks, files, and customer-facing APIs. That means security failures can affect more than a request/response cycle.

A failure could become:

- cross-account access
- forged provider state
- broken regional isolation
- stuck credit reservations
- duplicate charges
- invalid refunds
- unsafe file access
- SSRF through media inputs
- provider abuse through malformed execution requests

For this reason, BabySea publishes selected public security reports to show how the system is tested against the failure modes that matter for generative-media infrastructure.

## Security model

BabySea uses layered controls. No single layer is expected to carry the full system.

```text
Client
  -> Regional API endpoint
  -> API key authentication
  -> Account and scope resolution
  -> Request validation
  -> Rate and concurrency controls
  -> Credit reservation
  -> Provider execution and failover
  -> Charge/refund settlement
  -> Webhooks, logs, and observability
```

The public security posture focuses on these invariants:

| Invariant          | Meaning                                                                        |
| ------------------ | ------------------------------------------------------------------------------ |
| Identity           | Account identity must come from verified API keys, not user-controlled headers |
| Tenancy            | Customers must not access, cancel, delete, or mutate non-owned generations     |
| Regional isolation | API keys must not replay across isolated data regions                          |
| Validation         | Malformed inputs must not reach unsafe execution paths                         |
| SSRF defense       | Media input URLs must not become internal network probes                       |
| Webhook trust      | Provider callbacks must not be forgeable state transitions                     |
| Idempotency        | Retries must not create duplicate work or duplicate settlement                 |
| Credit integrity   | Successful generations must settle exactly once                                |
| Defense in depth   | Edge controls help, but app and database invariants still enforce safety       |

## Public report standard

Public reports in this repository should follow this standard:

1. State the scope clearly.
2. Identify what was tested and what was not tested.
3. Use anonymized operational identifiers.
4. Avoid publishing raw exploit scripts or replayable payloads.
5. Include result tables where possible.
6. Include limitations and non-certification language.
7. Separate evidence from interpretation.
8. Map findings to product invariants or control areas.
9. Include remediation status when relevant.
10. Keep internal secrets, logs, and customer data out of the repository.

This repository is intended to be useful without being dangerous.

## How to read the reports

A public BabySea security report is not a claim that the system is perfectly secure.

It means:

- a defined assessment was performed
- the scope is documented
- the results are summarized
- operational identifiers are sanitized
- limitations are stated
- the tested invariants are described

It does not mean:

- every possible vulnerability class was tested
- every future code change inherits the same result
- every provider-side behavior is controlled by BabySea
- the report replaces independent third-party security review
- the public version contains all internal evidence

Serious security claims should be bounded. Bounded claims are more trustworthy than absolute claims.

## Responsible disclosure

If you believe you have found a security issue in BabySea, please report it responsibly through one of the channels below. We acknowledge reports within **48 hours** and coordinate fixes under our published disclosure policy.

### Reporting channels

| Channel              | Address                                                                                          |
| -------------------- | ------------------------------------------------------------------------------------------------ |
| Email                | [security@babysea.ai](mailto:security@babysea.ai)                                                |
| Security policy      | [https://babysea.ai/security](https://babysea.ai/security)                                       |
| Privacy policy       | [https://babysea.ai/privacy-policy](https://babysea.ai/privacy-policy)                           |
| Machine-readable     | [https://babysea.ai/.well-known/security.txt](https://babysea.ai/.well-known/security.txt)       |
| Encryption (PGP)     | [https://babysea.ai/.well-known/pgp-key.txt](https://babysea.ai/.well-known/pgp-key.txt)         |
| Acknowledgments      | [https://babysea.ai/acknowledgments](https://babysea.ai/acknowledgments)                         |
| Support (non-urgent) | [https://babysea.ai/support](https://babysea.ai/support)                                         |
| Trust center         | [https://trust.babysea.ai](https://trust.babysea.ai)                                             |

The canonical reporting policy is the published [`security.txt`](https://babysea.ai/.well-known/security.txt). If anything in this README disagrees with `security.txt`, treat `security.txt` as the source of truth.

### Rules of engagement

When testing or reporting, please:

- **Do not** access, modify, delete, or exfiltrate data that does not belong to you.
- **Do not** run destructive tests against production systems.
- **Do not** perform denial-of-service or volumetric testing without prior written authorization.
- **Do not** target other customers' accounts, generations, files, or webhook endpoints.
- **Do not** publish exploit material, payloads, or proof-of-concept code in public channels before coordination.
- **Do** use your own test account and your own API keys.
- **Do** stop and report immediately if you encounter another customer's data.
- **Do** give us a reasonable window to remediate before any public disclosure.

### What to include in a report

A good report includes:

1. A clear description of the issue and the affected component.
2. The reproduction steps, scoped to your own account.
3. The expected vs. observed behavior.
4. The potential impact (what an attacker could achieve).
5. Any logs, request IDs, or timestamps from your own session.
6. Your contact information for follow-up and acknowledgment.

We operate a coordinated disclosure model. Researchers who follow this policy in good faith will not be subject to legal action by BabySea for their security research, and may be listed on the public [Acknowledgments](https://babysea.ai/acknowledgments) page upon request.

## License

This repository contains two kinds of material, each licensed separately.

### Documentation and reports

Unless otherwise stated, published security reports under `reports/` and this README are licensed under the [Creative Commons Attribution 4.0 International License (`CC BY 4.0`)](https://creativecommons.org/licenses/by/4.0/). Helper templates, scaffolds, and example structures are covered separately under [`Apache-2.0`](#templates-and-helpers).

You are free to:

- **Share** ➜ copy and redistribute the material in any medium or format.
- **Adapt** ➜ remix, transform, and build upon the material for any purpose, including commercially.

Under the following terms:

- **Attribution** ➜ You must give appropriate credit to BabySea, link to the license, and indicate if changes were made.
- **No additional restrictions** ➜ You may not apply legal terms or technological measures that legally restrict others from doing anything the license permits.

### Templates and helpers

Any non-sensitive helper templates, scaffolds, or example structures included in this repository are licensed under the [Apache License 2.0 (`Apache-2.0`)](https://www.apache.org/licenses/LICENSE-2.0), unless a different license is stated at the top of the file.

### What is not licensed for reuse

The following are explicitly **not** included in this repository and are not granted under any license:

- live or historical API keys, account IDs, generation IDs, or other operational identifiers
- internal log rows, database dumps, or production telemetry
- raw exploit scripts, payloads, or reusable attack automation
- internal hardening notes, remediation tickets, or unredacted incident reports
- BabySea trademarks, logos, and brand assets (all rights reserved)

Attribution example:

> Based on "BabySea Public Red-Team Assessment Report" by BabySea, licensed under CC BY 4.0. Source: https://github.com/babysea-ai/babysea-security-reports.

## About BabySea

BabySea is the execution control plane for generative media.

Developers send image and video workloads through one API. BabySea manages request validation, provider selection, failover, credit reservation and settlement, execution state, artifact delivery, events, webhooks, and observability across inference providers.

The goal is to make generative-media execution predictable in production.

Learn more:

- Website: [https://babysea.ai](https://babysea.ai)
- Docs: [https://docs.babysea.ai](https://docs.babysea.ai)
- Status: [https://status.babysea.ai](https://status.babysea.ai)
- Trust center: [https://trust.babysea.ai](https://trust.babysea.ai)
- GitHub: [https://github.com/babysea-ai](https://github.com/babysea-ai)
