# Architecture

## Current model

WebTrace v1.0.0 is a single-file client-side application.

```text
User selects log files
        |
        v
File ingestion / streaming
        |
        v
parseLogLine()
        |
        v
normalizeEvent()
        |
        +----> filename / extension extraction
        |
        +----> IP classification
        |
        v
runThreatDetectionEngine()
        |
        +----> signature rules
        +----> behavioral rules
        +----> suspicious User-Agent
        +----> public-IP upload indicator
        +----> HTTP response/data-exfiltration indicator
        |
        v
Filtering / exclusions / sorting
        |
        +----> dashboard
        +----> forensic log stream
        +----> unique IP inspector
        +----> detailed event drawer
        +----> IR playbook
        |
        v
Export
```

## Why single-file

The application is deliberately easy to distribute in an isolated environment. An analyst can carry one HTML file and open it without installing a backend.

## Evidence model

The application retains the raw source line while adding normalized fields. This allows the analyst to pivot between machine-readable fields and original evidence.

## Detection model

The engine combines:

1. Signature matching.
2. User-Agent indicators.
3. IP-scope-aware behavioral indicators.
4. HTTP-status/response-size indicators.
5. Sliding-window behavioral detection.

## Future architecture

If the rule set grows significantly, a later release can separate parsers, rules, playbooks, and UI assets into individual modules. Until then, the single-file model keeps deployment simple for air-gapped use.
