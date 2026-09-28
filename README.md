# WebTrace

**Air-gapped, browser-based HTTP log analysis and DFIR triage tool.**

WebTrace is a self-contained HTML application designed for analysts who need to inspect web/application logs locally without uploading evidence to a third-party service. It combines parsing, normalization, filtering, threat detection, behavioral indicators, investigation views, and IR playbooks in a single offline-capable file.

> **Current release:** v1.0.0 — Initial Public Release
>
> **Primary file:** `WebTrace.html`

## Why this project exists

During web-log investigations, an analyst often has to move between raw log files, parsing scripts, SIEM queries, spreadsheets, and separate notes. This project brings the first-pass workflow into one local interface:

- Upload one or more log files.
- Parse supported HTTP log formats.
- Normalize important fields into a common forensic record.
- Search, filter, exclude, sort, and inspect records.
- Identify suspicious HTTP activity and behavioral patterns.
- Open an event to review investigation and remediation guidance.
- Export the working dataset for further analysis.

The application is intentionally **local-first**. Log data is processed in the browser and the project does not require a backend server for its core workflow.

## Features

### Log ingestion and parsing

- Multiple-file upload.
- Streaming ingestion for large text files where supported by the browser.
- JSON log parsing.
- Apache/Nginx combined-style parsing.
- IIS W3C parsing using `#Fields` metadata.
- IIS W3C headerless fallback parsing.
- Generic HTTP-log fallback parsing.
- Syslog-style timestamp recognition.
- Source file tracking.
- Source line tracking.
- Raw log preservation.

### Normalized forensic fields

The application maintains a common event model including:

- Timestamp
- Hostname / Source
- Server IP
- Server Port
- Client IP
- Username
- Virtual Host
- Method
- Request URI
- Query String
- Filename
- Extension
- HTTP Version
- Status
- Substatus
- Win32 Status
- Time Taken
- Response Bytes
- Referrer
- User-Agent
- Threat Tag
- Source File
- Raw Logs

### Detection and triage

Built-in detection coverage includes:

- RCE / Command Injection
- Web Shell Activity
- SQL Injection (SQLi)
- Path Traversal / LFI
- File Inclusion Wrapper
- Server-Side Request Forgery (SSRF)
- Cross-Site Scripting (XSS)
- Log4Shell / JNDI Injection
- Sensitive File / Config Exposure Probe
- XXE / XML Entity Injection
- Server-Side Template Injection (SSTI)
- HTTP Request Smuggling / CRLF Probe
- Automated Recon Scanner
- Behavioral brute-force detection
- Repeated 403 probing
- 404 reconnaissance patterns
- Custom internal policy rules
- Custom web-shell/backdoor rules

Additional indicators include:

1. **Suspicious User-Agent detection** for common scanners, enumeration tools, scripted clients, and automated HTTP clients.
2. **Public IP Upload Activity** for POST/PUT/PATCH upload/import/attachment-style requests originating from public IP addresses.
3. **Filename and extension extraction** from URI/query/referrer data.
4. **HTTP status and response-size-driven exfiltration indicators**, including successful large download/export/archive patterns. These are indicators requiring analyst correlation, not automatic proof of exfiltration.

## Dashboard

The original compact dashboard structure is retained while the backend has been extended.

- Total Requests
- Threats Identified
- Clean Traffic
- Unique Client IPs
- Public/private IP filtering
- HTTP status distribution
- Top requested URIs
- Threat categories
- Top source/client IPs
- Unique Client IP Inspector
- Filter and exclusion actions
- Detailed log drawer
- Investigation and remediation playbook
- Column visibility controls
- CSV export

## Supported environment

The project is intentionally simple to run:

1. Download `WebTrace.html`.
2. Open it in a modern browser.
3. Upload your log files.
4. Begin analysis.

No Python installation, database, web server, Node.js runtime, or external API is required for the core application.

For investigations involving sensitive evidence, use an approved isolated analysis workstation and follow your organization's evidence-handling requirements.

## Repository structure

```text
WebTrace/
├── WebTrace.html       # Main standalone application
├── README.md                         # Project overview and quick start
├── CHANGELOG.md                      # Version/change history
├── LICENSE                           # Project license
├── CONTRIBUTING.md                   # Contribution workflow
├── CODE_OF_CONDUCT.md                # Community expectations
├── SECURITY.md                       # Security issue reporting
├── CITATION.cff                      # Citation metadata
├── VERSION                           # Current release version
├── .gitignore
├── docs/
│   ├── USER_GUIDE.md                 # Analyst usage guide
│   ├── CUSTOMIZATION.md              # Add/update/remove parser, rules, playbooks
│   ├── PARSER_GUIDE.md               # Parser architecture and extension guide
│   ├── DETECTION_RULES.md            # Detection rule reference
│   ├── IR_PLAYBOOKS.md               # Playbook reference and editing guide
│   ├── ARCHITECTURE.md               # Application architecture
│   └── RELEASE_PROCESS.md             # Release/checklist guidance
├── examples/
│   ├── apache_combined_sample.log
│   ├── iis_w3c_sample.log
│   └── json_http_sample.jsonl
└── .github/
    └── ISSUE_TEMPLATE/
        ├── bug_report.md
        └── feature_request.md
```

## Quick start

### Option A — GitHub download

Open the repository and download `WebTrace.html`, then open it locally.

### Option B — Git clone

```bash
git clone https://github.com/YOUR-USERNAME/WebTrace.git
cd WebTrace
```

Then open:

```text
WebTrace.html
```

## Important: evidence handling

This tool is intended for defensive investigation and triage. It does not replace your SIEM, EDR, WAF, application telemetry, forensic acquisition process, or evidence-preservation procedures.

Detection names such as **Potential Data Exfiltration** are deliberately treated as indicators. Analysts should correlate them with authentication, endpoint, network, application, and response-size evidence before concluding that data was actually exfiltrated.

## Customization

The application is currently a single-file architecture. Customization is therefore performed inside `WebTrace.html`.

See:

- [Custom parser, rule and playbook guide](docs/CUSTOMIZATION.md)
- [Parser guide](docs/PARSER_GUIDE.md)
- [Detection rules](docs/DETECTION_RULES.md)
- [IR playbooks](docs/IR_PLAYBOOKS.md)

These documents explain exactly where to add, update, and remove each component without redesigning the UI.

## Development philosophy

The project follows a few practical principles:

- Preserve the analyst workflow and existing UI unless a UI change is explicitly required.
- Keep the core application self-contained.
- Prefer deterministic local parsing and detection.
- Preserve raw evidence alongside normalized fields.
- Make detections explainable.
- Treat behavioral indicators as leads that require correlation.
- Keep parser changes separate from detection-rule changes.
- Validate JavaScript syntax before release.

## License

See [LICENSE](LICENSE).

## Author / project

**Author:** Arslan Sabir  
**Project:** WebTrace

Built as a practical DFIR/SOC investigation project focused on local web-log analysis, threat hunting, and incident-response triage.

## Community recommendations and code review

WebTrace accepts community recommendations through GitHub Issues and code proposals through Pull Requests. Recommendations and pull requests are reviewed before they become part of the official `main` branch. See [`CONTRIBUTING.md`](CONTRIBUTING.md) and [`docs/CONTRIBUTION_WORKFLOW.md`](docs/CONTRIBUTION_WORKFLOW.md).
