# Analyst User Guide

## 1. Start the tool

Open `WebTrace.html` in a modern browser.

No server or backend is required for the core workflow.

## 2. Load evidence

Use the file-upload control to select one or more HTTP/application log files.

The application reads the files locally and creates normalized forensic records.

## 3. Review the KPIs

The dashboard provides:

- Total Requests
- Threats Identified
- Clean Traffic
- Unique Client IPs

The Unique Client IP view can be used to pivot into source-IP activity.

## 4. Filter traffic

Available filtering includes:

- All IPs
- Public Only
- Private Only
- Client IP
- Hostname
- HTTP method
- HTTP status
- Threat category
- Search / keyword

Exclusion controls can remove known-good or irrelevant IPs, hosts, statuses, and paths from the working view.

## 5. Review dashboards

The dashboard summarizes:

- HTTP status codes
- Top requested URIs
- Threat categories
- Top source/client IPs

Use the filter/exclude actions on the dashboard to pivot quickly.

## 6. Inspect a log

Click a log entry to open its detail view.

Review:

- Normalized fields
- Raw log
- Parser/source information
- Response size
- User-Agent
- Threat indicators
- Investigation guidance
- Remediation guidance

## 7. Interpret indicators carefully

Examples:

- Suspicious User-Agent → investigate whether the source is an approved scanner or automation.
- Public IP Upload Activity → validate the endpoint, identity, source, and business purpose.
- Potential Data Exfiltration → correlate response size, requested file, user/session, destination, and network telemetry.

These indicators are triage leads, not automatic incident conclusions.

## 8. Export

Use the export controls to save the current normalized/working dataset for additional analysis or case documentation.
