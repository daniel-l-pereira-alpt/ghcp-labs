---
title: Lab 07 Security Scan Report
description: Security scan report for lab07 requirements based on pip-audit findings and GitHub Advisory severities.
ms.date: 2026-06-19
ms.topic: reference
keywords:
  - security scan
  - pip-audit
  - cve
  - lab07
---

## Summary

This scan audited 4 direct requirements and 12 resolved packages from the
dependency graph. The audit found 17 unique vulnerabilities across 3 packages:
`requests`, `idna`, and `urllib3`.

* Critical: 1
* High: 4
* Medium: 12
* Low: 0

No known vulnerabilities were reported for `bcrypt`, `pytest`, or `pyyaml` in
this scan.

## Findings

| Package | Current version | Known CVE ID | Severity | Recommended action |
|---|---|---|---|---|
| requests | 2.18.0 | CVE-2018-18074 | HIGH | Upgrade `requests` to `>= 2.33.0` to clear this and later `requests` advisories. |
| requests | 2.18.0 | CVE-2023-32681 | MEDIUM | Upgrade `requests` to `>= 2.33.0`; if you cannot upgrade immediately, avoid proxied redirects with embedded credentials. |
| requests | 2.18.0 | CVE-2024-35195 | MEDIUM | Upgrade `requests` to `>= 2.33.0`; avoid reusing `Session` objects after calls with `verify=False`. |
| requests | 2.18.0 | CVE-2024-47081 | MEDIUM | Upgrade `requests` to `>= 2.33.0`; if blocked, set `trust_env=False` to avoid `.netrc` credential loading. |
| requests | 2.18.0 | CVE-2026-25645 | MEDIUM | Upgrade `requests` to `>= 2.33.0`; avoid calling `extract_zipped_paths()` on untrusted archives until upgraded. |
| idna | 2.5 | CVE-2024-3651 | MEDIUM | Upgrade `idna` to `>= 3.15`, or upgrade `requests` and pin a safe transitive resolution. |
| idna | 2.5 | CVE-2026-45409 | MEDIUM | Upgrade `idna` to `>= 3.15`; enforce domain length limits if an immediate upgrade is not possible. |
| urllib3 | 1.21.1 | CVE-2018-20060 | CRITICAL | Upgrade `urllib3` to `>= 2.6.0`, or upgrade `requests` to a release that resolves to a patched `urllib3`. |
| urllib3 | 1.21.1 | CVE-2019-11324 | HIGH | Upgrade `urllib3` to `>= 2.6.0`; verify TLS validation behavior in downstream integrations after the upgrade. |
| urllib3 | 1.21.1 | CVE-2019-11236 | MEDIUM | Upgrade `urllib3` to `>= 2.6.0`; do not pass untrusted request parameters into raw request construction APIs. |
| urllib3 | 1.21.1 | CVE-2020-26137 | MEDIUM | Upgrade `urllib3` to `>= 2.6.0`; avoid passing untrusted HTTP methods into low-level request APIs. |
| urllib3 | 1.21.1 | CVE-2023-43804 | HIGH | Upgrade `urllib3` to `>= 2.6.0`; do not rely on automatic redirects when sending `Cookie` headers. |
| urllib3 | 1.21.1 | CVE-2023-45803 | MEDIUM | Upgrade `urllib3` to `>= 2.6.0`; disable redirects for requests that may carry sensitive bodies. |
| urllib3 | 1.21.1 | CVE-2018-25091 | MEDIUM | Upgrade `urllib3` to `>= 2.6.0`; do not forward authorization headers across redirects. |
| urllib3 | 1.21.1 | CVE-2024-37891 | MEDIUM | Upgrade `urllib3` to `>= 2.6.0`; use `ProxyManager` correctly and avoid manual `Proxy-Authorization` headers. |
| urllib3 | 1.21.1 | CVE-2025-50181 | MEDIUM | Upgrade `urllib3` to `>= 2.6.0`; disable redirects at request level instead of only at `PoolManager` construction. |
| urllib3 | 1.21.1 | CVE-2025-66471 | HIGH | Upgrade `urllib3` to `>= 2.6.0`; avoid streaming compressed content from untrusted sources until patched. |

## Remediation Plan

1. Replace `requests==2.18.0` with a current maintained release, at minimum
   `requests>=2.33.0`, then reinstall dependencies and regenerate the SBOM.
2. Re-resolve the dependency graph so that `urllib3>=2.6.0` and `idna>=3.15`
   are selected, either transitively through `requests` or via explicit pins if
   needed.
3. Re-run `pip-audit -r requirements.txt -f json` and confirm the vulnerability
   count returns to zero before merging.
4. Keep the generated `sbom.xml` and this report in the branch so the PR shows
   both the risk and the remediation evidence.

## Notes

Severity labels were taken from the GitHub Advisory Database pages referenced by
the advisories returned from `pip-audit`. Duplicate advisory IDs in the raw
audit output were deduplicated in this report.