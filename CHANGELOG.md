# Changelog

## v1.0.0 — Initial Public Release

### Added
- Forensic Log Stream backend integrated into the original v4.0 interface.
- JSON, Apache/Nginx Combined, IIS W3C, IIS headerless fallback, and generic HTTP parsing.
- Source-file and source-line tracking.
- Filename and extension extraction.
- Suspicious User-Agent detection.
- Public IP upload activity indicator.
- HTTP status / response-size-driven potential data-exfiltration indicator.
- Detailed investigation and remediation playbooks.
- Unique Client IP Inspector.
- Client IP filtering and exclusion actions.
- Public/private IP classification.
- Multiple filters and exclusions.
- CSV export.
- Responsive viewport-aware layout.

### Detection coverage
- RCE / Command Injection
- Web Shell Activity
- SQL Injection (SQLi)
- Path Traversal / LFI
- File Inclusion Wrapper
- SSRF
- XSS
- Log4Shell / JNDI Injection
- Sensitive File / Config Exposure Probe
- XXE
- SSTI
- HTTP Request Smuggling / CRLF Probe
- Automated Recon Scanner
- Brute-force / 401 behavioral detection
- Repeated 403 probing
- 404 reconnaissance storm
- Custom policy rules
- Custom web-shell/backdoor rules

## v4.0 baseline

The v4.0 release established the original dashboard, compact KPI layout, filters, charts, log table, and analyst interaction model that the current release preserves.


**Author:** Arslan Sabir
