# Detection Rule Reference

## Signature rules

The current built-in rule set includes:

| Category | General purpose |
|---|---|
| RCE / Command Injection | Identify command/execution-oriented request patterns |
| Web Shell Activity | Identify common web-shell indicators |
| SQL Injection (SQLi) | Identify SQL injection patterns |
| Path Traversal / LFI | Identify traversal and local-file access probes |
| File Inclusion Wrapper | Identify file/URL wrapper patterns |
| Server-Side Request Forgery (SSRF) | Identify server-side URL fetching targets/patterns |
| Cross-Site Scripting (XSS) | Identify script-injection patterns |
| Log4Shell / JNDI Injection | Identify JNDI-style request indicators |
| Sensitive File / Config Exposure Probe | Identify requests for sensitive files/configuration |
| XXE / XML Entity Injection | Identify XML entity injection indicators |
| Server-Side Template Injection (SSTI) | Identify template-expression indicators |
| HTTP Request Smuggling / CRLF Probe | Identify ambiguous HTTP/header manipulation patterns |
| Automated Recon Scanner | Identify automated scanning/recon patterns |

## Behavioral rules

Behavioral detection also covers patterns such as:

- Brute Force Hammering (401s)
- Repeated Access-Denied (403) Probing
- 404 Recon Storm

These detections are based on activity across multiple events rather than a single request.

## Additional indicators

### Suspicious User-Agent

Matches known scanner, enumeration, scripted HTTP-client, and automation patterns.

### Public IP Upload Activity

Raised when a public source IP performs an upload/import/attachment-style request using relevant HTTP methods.

### Potential Data Exfiltration

The current implementation uses successful HTTP responses combined with large response sizes and download/export/archive-style targets. It is explicitly an indicator requiring correlation.

## Severity

The engine uses the severity values defined by the rules, including CRITICAL, HIGH, MEDIUM, LOW, and clean/no-threat states.

Severity should be treated as triage guidance rather than a statement of confirmed compromise.

## Adding a rule

Use `customVectorRules` in the HTML. See `docs/CUSTOMIZATION.md`.
