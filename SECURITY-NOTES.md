# Log4Shell Security Notes (CVE-2021-44228)

## Summary

Log4Shell is a critical remote code execution vulnerability in Apache Log4j 2, announced December 9, 2021. It arises from JNDI features in Log4j 2's `log4j-core` that do not protect against attacker-controlled LDAP and other JNDI-related endpoints. An attacker who can control log messages or log message parameters can execute arbitrary code loaded from LDAP servers when message lookup substitution is enabled.

## CVE Identifier

- **CVE-2021-44228** (Log4Shell) — CVSS 3.x Base Score: **10.0 CRITICAL** (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H)

Related CVEs in the same series: CVE-2021-45046 (CVSS 9.0 CRITICAL), CVE-2021-45105, and CVE-2021-44832.

## Affected Versions

- **Affected:** Apache Log4j2 versions 2.0-beta9 through 2.14.1 (inclusive) — i.e., all versions from 2.0-beta9 up to and including 2.14.1 are vulnerable to CVE-2021-44228.
- Per the Apache Logging Services security page, the affected ranges are `[2.0-beta9, 2.3.1) ∪ [2.4, 2.12.2) ∪ [2.13.0, 2.15.0)` (i.e., up to but not including 2.15.0).

## Fixed Versions

- **2.15.0** — first release where JNDI behavior is disabled by default.
- **2.16.0** (Java 8 and later) — complete removal of JNDI lookups; recommended target version.
- **2.12.2** (Java 7) and **2.3.1** (Java 6) — fixed releases for legacy Java versions.

 Note: only the `log4j-core` JAR is impacted; applications using only `log4j-api` without `log4j-core` are not affected by this vulnerability.

## Severity

- **CVSS 3.x Base Score: 10.0 — CRITICAL** (NVD and Apache both rate this 10.0 CRITICAL; vector CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H).

## Remediation Actions

1. **Upgrade path** — upgrade affected Log4j 2 versions (2.0-beta9–2.14.1) to the fixed version: 2.16.0+ for Java 8+, 2.12.2 for Java 7, 2.3.1 for Java 6. Tracked as issue #2.
2. **Detecting vulnerable deployments** — scan logs, configs, and request inputs for `${jndi:` patterns and verify runtime `log4j-core` versions. Tracked as issue #3.
3. **Hardening** — disable message lookup substitution (`-Dlog4j2.formatMsgNoLookups=true` or `log4j2.formatMsgNoLookups=true`), audit/remove JMSAppender from Log4j 1 configurations, and restrict outbound LDAP/RMI connections. Tracked as issue #4.
4. **Automated dependency scanning** — enable GitHub Dependabot, run OWASP Dependency-Check in CI, and integrate Snyk/Trivy/Grype; block 2.0-beta9–2.14.1 and require >= 2.16.0 (Java 8+). Tracked as issue #5.

## Sources

- NVD — CVE-2021-44228: https://nvd.nist.gov/vuln/detail/cve-2021-44228
- Apache Logging Services Security Page: https://logging.apache.org/security.html
- Apache Log4j CVEs (ASF Blog): https://news.apache.org/foundation/entry/apache-log4j-cves
- GitHub Advisory Database (GHSA-v57x-gxfj-484q): https://github.com/advisories/GHSA-v57x-gxfj-484q
- OSV (GHSA-v57x-gxfj-484q): https://osv.dev/vulnerability/GHSA-v57x-gxfj-484q

## Checklist Reference

Parent tracking issue: "Log4Shell remediation checklist" (issue #1). Sub-issues #2–#5 cover the upgrade path, vulnerability detection, hardening, and automated dependency scanning.
