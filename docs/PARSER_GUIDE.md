# Parser Guide

## Current supported formats

### JSON

The parser accepts JSON-style HTTP event records and maps available properties into the normalized event model.

### Apache/Nginx combined-style logs

The parser extracts the conventional client IP, timestamp, request method/URI, status, response bytes, referrer, and User-Agent fields where present.

### IIS W3C

The parser uses the `#Fields:` header when available so field positions follow the actual IIS field order.

A headerless IIS fallback is also present for standard W3C-style ordering.

### Generic HTTP fallback

When a more specific format is not recognized, the generic parser attempts to identify the client IP, request line, status, and quoted HTTP fields without discarding the raw record.

### Timestamp support

The parser recognizes ISO-style timestamps, HTTP/Apache-style timestamps, IIS date/time combinations, and supported Syslog-style timestamps.

Display timestamps are normalized while the raw log remains available for evidence review.

## Normalization

`normalizeEvent(...)` provides a common representation regardless of source format.

Important normalized fields include:

- timestamp
- hostname
- serverIP
- serverPort
- clientIP
- username
- virtualHost
- method
- uri
- query
- filename
- extension
- httpVersion
- status
- substatus
- win32Status
- timeTaken
- responseBytes
- referrer
- userAgent
- threatTag
- sourceFile
- raw

## Adding a parser

See `docs/CUSTOMIZATION.md` for the exact edit points.

The key requirement is that every parser should return the normalized event model and preserve the raw source line.

## Testing parser changes

Use the files in `examples/` as regression samples. Add a new example whenever a new source format is introduced.
