# WebTrace — Project Summary
**Author:** Arslan Sabir

WebTrace is a local, browser-based HTTP log analysis and DFIR triage tool designed for analysts who need to investigate web logs without sending evidence to an external platform. Air-gapped, browser-based HTTP log analysis and DFIR triage tool with local parsing, threat detection, behavioral indicators, filtering, export, and IR playbooks.

## Detailed summary

The project started from a practical SOC/DFIR requirement: make web-log investigation faster without depending on spreadsheets, multiple parsing utilities, or uploading potentially sensitive evidence to external services.

The current release keeps the original compact analyst interface while adding a stronger backend for multi-format ingestion, normalization, detection, filtering, behavioral analysis, and investigation guidance.

## Main capabilities

### Parsing

- JSON
- Apache/Nginx Combined-style logs
- IIS W3C with `#Fields`
- IIS headerless fallback
- Generic HTTP fallback
- Supported Syslog-style timestamps

### Normalization

Common forensic fields are extracted from different source formats so analysts can work with a consistent view.

### Threat detection

Signature-based detections cover common web attack indicators, while behavioral detections identify patterns such as repeated 401, 403, and 404 activity.

### Newer triage indicators

- Suspicious User-Agent detection
- Public IP upload activity
- Filename and extension extraction
- HTTP-status/response-size-based potential data-exfiltration indicator

### Investigation workflow

The analyst can pivot from KPIs and charts to source IPs, URIs, individual events, raw logs, threat indicators, and IR playbooks.

### Air-gapped use

The core tool is a single HTML file. It can be copied to an isolated investigation workstation and opened locally without a server or external API dependency.

## Design principles

1. Keep evidence local.
2. Preserve raw logs.
3. Normalize without hiding source context.
4. Make detections explainable.
5. Treat alerts as investigation leads.
6. Keep the analyst workflow simple.
7. Make customization possible without rebuilding the application.

## Intended audience

- SOC analysts
- DFIR analysts
- Incident responders
- Threat hunters
- Security engineers
- Blue-team practitioners
- Web/application security teams

## Important limitation

The tool is a triage and analysis aid. A detection such as `Potential Data Exfiltration` is not proof that data was stolen. Investigation should correlate the log evidence with identity, endpoint, application, WAF, and network telemetry.
