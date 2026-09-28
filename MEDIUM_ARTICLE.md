**Author:** Arslan Sabir

# Building WebTrace: A Local-First Web Log DFIR Tool

**GitHub:** [Download the project and source code](YOUR-GITHUB-REPOSITORY-LINK)

When I started working on WebTrace, the idea was not to build another dashboard just for the sake of having a dashboard.

The problem was much more practical.

During a web investigation, an analyst can easily end up moving between raw IIS or Apache logs, SIEM queries, spreadsheets, parsing scripts, browser tools, and investigation notes. Each individual tool may do its job, but the actual investigation becomes fragmented.

I wanted something simpler:

**Take the logs. Keep them local. Parse them. Find the interesting activity. Investigate it from the same screen.**

That became WebTrace.

## What is WebTrace?

WebTrace is a browser-based HTTP log analysis and DFIR triage tool designed to work locally.

The main application is a single HTML file. There is no backend server required for the core workflow and no requirement to upload investigation data to an external service.

That makes the approach particularly useful for environments where log data is sensitive or where an investigation workstation is intentionally isolated.

The project is available on GitHub:

**[Download WebTrace](YOUR-GITHUB-REPOSITORY-LINK)**

The repository contains the standalone tool, documentation, sample logs, customization instructions, and release information.

---

# Why build another log-analysis tool?

The answer is simple: investigation workflow.

A raw web log contains a lot of information, but the information is not necessarily presented in the way an investigator needs it.

For example, an analyst may need to answer questions such as:

- Which public IPs are interacting with the application?
- Is someone scanning the application?
- Are there repeated 401, 403, or 404 patterns?
- Is a suspicious User-Agent involved?
- Is a public source uploading files?
- What filename and extension was requested?
- Was a large export or download returned successfully?
- Which requests belong to the same source?
- What should I investigate next?

The goal of this project is to reduce the time between **seeing a suspicious request** and **having enough context to investigate it**.

---

# Keeping the evidence local

One of the design decisions behind the project was to keep the core workflow local.

The analyst opens the HTML application and selects the log files.

The application processes the data in the browser.

There is no requirement for a central database, web server, or external API for the core functionality.

This does not automatically make an investigation system secure — workstation security, evidence handling, access controls, and organizational procedures still matter — but it removes one unnecessary dependency from the basic analysis workflow.

For sensitive investigations, this local-first approach can be useful because the analyst can keep the source evidence inside the approved investigation environment.

---

# Parsing different web-log formats

One of the first problems was format diversity.

Not every environment produces the same HTTP logs.

The current parser supports several common patterns:

- JSON HTTP events
- Apache/Nginx combined-style logs
- IIS W3C logs using `#Fields`
- Headerless IIS W3C-style data
- Generic HTTP log fallback parsing
- Supported Syslog-style timestamps

The important part is not simply parsing the line.

The parser converts different source formats into a common forensic event model.

That means the analyst can work with fields such as:

- Timestamp
- Hostname / Source
- Server IP
- Server Port
- Client IP
- Username
- Virtual Host
- HTTP Method
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
- Raw Log

The raw record is still preserved so that normalization does not replace the original evidence.

---

# From raw requests to useful indicators

Parsing alone is not enough.

The next step was adding detections that can help an analyst prioritize what deserves attention.

The current detection engine includes patterns for:

- RCE / Command Injection
- Web Shell Activity
- SQL Injection
- Path Traversal / LFI
- File Inclusion Wrappers
- SSRF
- XSS
- Log4Shell / JNDI injection
- Sensitive file/configuration exposure
- XXE
- SSTI
- HTTP request smuggling / CRLF probing
- Automated reconnaissance

There are also behavioral detections for patterns such as repeated authentication failures, repeated access-denied requests, and large numbers of 404 requests that may indicate reconnaissance.

The important design principle here is that a detection should be explainable.

The tool should not simply say:

> Threat detected.

It should give the analyst enough information to understand why the request was flagged and what should be checked next.

---

# Suspicious User-Agent detection

One of the additional capabilities is suspicious User-Agent detection.

Web logs frequently contain User-Agent strings that can provide useful context about the client.

The tool checks for patterns associated with common scanners, enumeration tools, scripted clients, and automated HTTP tooling.

Examples include scanner and automation identifiers such as:

- sqlmap
- nikto
- nmap
- masscan
- gobuster
- dirsearch
- ffuf
- nuclei
- Nessus/OpenVAS-related clients
- BurpSuite-related clients
- scripted HTTP clients

This should not be interpreted as saying that every request from one of these clients is malicious.

Security teams may legitimately use scanners.

The purpose of the indicator is to answer a much more useful question:

**Does this request deserve investigation?**

The analyst can then correlate the source with approved scanning activity, change windows, penetration tests, and other telemetry.

---

# Monitoring public-IP upload activity

Another useful addition is the ability to identify upload-style activity coming from public IP addresses.

The detection looks for relevant HTTP methods such as POST, PUT, or PATCH combined with upload/import/attachment-style endpoint patterns and a public source IP.

For example, a request to an endpoint resembling an upload or import function can be highlighted for investigation when it originates externally.

Again, this is not automatically treated as malicious.

A public-facing application may legitimately accept uploads.

The purpose is to make the activity visible so the analyst can ask:

- Is the endpoint supposed to be public?
- Was the user authenticated?
- Is the source expected?
- What was uploaded?
- Was the activity part of a normal business workflow?
- Did other suspicious requests occur around the same time?

---

# Extracting filenames and extensions

A small feature can make a surprisingly large difference during investigation.

The parser now attempts to identify filenames and extensions from request-related data.

Instead of only seeing:

`/download/report/customer_data.xlsx`

an analyst can work with separate values such as:

**Filename:** `customer_data.xlsx`

**Extension:** `xlsx`

This makes it easier to search for specific file types and identify potentially interesting downloads, exports, backups, archives, scripts, configuration files, or other artifacts.

It also creates a better foundation for future detection logic.

---

# HTTP status codes as investigation signals

HTTP status codes are often treated as simple application health information.

For DFIR, they can provide another layer of context.

A large number of 404 responses may indicate discovery activity.

Repeated 403 responses may indicate attempts to access restricted resources.

Repeated 401 responses may indicate authentication pressure.

And successful 2xx responses combined with other characteristics can provide useful signals around downloads and exports.

The tool therefore includes an indicator for potential data-exfiltration patterns using successful HTTP responses, response size, and download/export/archive-style targets.

For example, a large successful response for a report, archive, export, or data file can be surfaced as:

**Potential Data Exfiltration**

The wording is intentional.

It is a potential indicator, not a conclusion.

A legitimate employee downloading a large report can look similar at the HTTP-log level.

That is why the next investigation step should be correlation with:

- User identity
- Authentication events
- Source IP
- Endpoint purpose
- Filename and extension
- Response size
- Frequency
- Application logs
- WAF telemetry
- Network telemetry
- Endpoint telemetry

The log engine is there to highlight the event. The investigation determines what actually happened.

---

# The analyst workflow

The interface was kept intentionally compact.

The main dashboard provides four key KPIs:

- Total Requests
- Threats Identified
- Clean Traffic
- Unique Client IPs

From there, the analyst can move into:

- HTTP status distribution
- Top requested URIs
- Threat categories
- Top source IPs
- Unique Client IP inspection
- Filtering
- Exclusions
- Individual event details

The idea is to support a natural investigation path:

**Overview → Pivot → Event → Evidence → Playbook**

---

# The IR playbook is part of the detection

Another lesson from building the tool was that a detection without investigation guidance is only half useful.

When an event is opened, the application can show an IR playbook with two sections:

### How to Investigate

This gives the analyst practical questions and correlation points to validate the detection.

### Remediation Steps

This gives the analyst actions to consider after validation and evidence preservation.

For example, an RCE-related detection can direct the investigation toward process creation, the web-service account, outbound connections, and the affected application process.

A sensitive-file exposure detection can direct the analyst toward response status, returned content, credentials, tokens, and configuration data.

The goal is not to automate the analyst's decision.

The goal is to make the next investigation step easier to find.

---

# Custom rules are important

Every organization has its own environment.

A rule that is useful in one environment may create noise in another.

That is why the project includes a customization path.

Custom detection rules can be added to the `customVectorRules` section.

Custom rules can define:

- Category
- Severity
- Regular-expression patterns
- Short playbook guidance

The project documentation also explains how to add, update, and remove rules.

---

# Custom parsers

The same principle applies to parsers.

Organizations often have application-specific logging formats.

Instead of requiring the whole project to be redesigned, a custom parser can be added to the parser flow and converted into the common normalized event model.

The repository includes a parser customization guide explaining where to make those changes and how to preserve the raw event while normalizing the important fields.

---

# Why keep it as one HTML file?

There is a temptation to immediately turn every project into a large framework with a backend, database, build system, package manager, API layer, and deployment pipeline.

For this project, that would defeat part of the original purpose.

Sometimes an analyst just needs a tool that can be copied into an isolated environment and opened.

One HTML file provides exactly that simplicity.

It is easy to move.

It is easy to archive.

It is easy to version.

It is easy to inspect.

And it does not require a development environment just to perform basic log triage.

That does not mean this architecture can never evolve.

If the project grows significantly, parsers, detection rules, playbooks, and UI components can eventually be separated into modules.

For the current use case, the single-file model keeps the deployment footprint small.

---

# What I learned building it

The most useful lesson was that building a security tool is not just about adding detections.

It is about connecting the pieces.

A parser gives you fields.

A detection gives you a signal.

A filter gives you a pivot.

A raw event gives you evidence.

A playbook gives you a next step.

When these pieces are connected, the tool becomes much more useful during an actual investigation.

Another important lesson was to avoid treating every suspicious pattern as a confirmed incident.

Security telemetry is full of legitimate activity that can resemble malicious behavior.

A scanner can be authorized.

An upload can be normal.

A large download can be legitimate.

A 404 storm can come from a vulnerability scanner performing an approved assessment.

Good DFIR tooling should help the analyst investigate those questions rather than making the decision for them.

---

# Where the project is going

There is plenty of room for future development.

Some areas I would like to explore include:

- More application-specific parsers
- More behavioral correlation
- Better evidence timelines
- Investigation case management
- More flexible rule configuration
- Additional web-server formats
- Stronger test coverage
- More export formats
- MITRE ATT&CK mapping for applicable detections
- Better integration with broader DFIR workflows

The important part is keeping the project useful for the analyst rather than adding complexity just because the technology allows it.

---

# Try it yourself

The project is available on GitHub, including the standalone HTML tool, documentation, sample logs, customization guides, and release information.

**[Download WebTrace on GitHub](YOUR-GITHUB-REPOSITORY-LINK)**

The easiest way to try it is simply to download:

`WebTrace.html`

Open it in a modern browser and load a sample log from the `examples` directory.

No server is required for the core workflow.

---

# Final thoughts

WebTrace started as an attempt to make a repetitive SOC/DFIR task easier: taking raw HTTP logs and turning them into something an investigator can work with quickly.

It is not intended to replace a SIEM, EDR, WAF, forensic acquisition platform, or experienced analyst.

It is a focused investigation aid.

The philosophy behind it is simple:

**Keep the evidence local. Normalize the data. Highlight useful indicators. Preserve the raw record. Give the analyst the context needed to investigate.**

That is what I wanted WebTrace to become.

And this is only the beginning.

---

**Project:** WebTrace v1.0.0  
**Focus:** DFIR | SOC | Threat Hunting | Web Log Analysis | Incident Response  
**Distribution:** Standalone HTML / GitHub
