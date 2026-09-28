# Customization Guide

The current release uses a **single-file architecture**. All parser, detection-rule, and IR-playbook changes are made in:

```text
WebTrace.html
```

Make a backup or create a Git branch before editing.

---

## 1. Add a custom detection rule

Find:

```javascript
const customVectorRules = [
```

A rule has this structure:

```javascript
{
  category: "Custom Example Rule",
  severity: "HIGH",
  patterns: [
    /example-pattern/i,
    /another-pattern/i
  ],
  playbook: "Short fallback guidance for this rule."
}
```

### Add

Add another object before the closing `];`.

### Update

Change the `category`, `severity`, `patterns`, or `playbook` fields.

### Remove

Delete the complete object for that rule.

### Important

Use escaped regular-expression syntax correctly. Test the rule against both a benign line and an intended matching line.

The engine evaluates URI/query content and User-Agent data for the rule patterns.

---

## 2. Add or change a suspicious User-Agent

Find:

```javascript
function isSuspiciousUserAgent(ua) {
```

The function contains the User-Agent regular expression.

To add a pattern, extend the expression, for example:

```javascript
/(?:existing-tool|new-tool)/i
```

To remove a pattern, remove its name from the expression.

Document why the pattern is useful and note possible legitimate uses.

---

## 3. Change public-IP upload detection

Find:

```javascript
function isUploadRequest(log) {
```

This function controls which HTTP methods and endpoint patterns are treated as upload/import activity.

The detection is subsequently applied when the source IP is classified as public.

To change the upload endpoint vocabulary, modify the endpoint/path pattern in this function.

---

## 4. Change data-exfiltration indicators

Find:

```javascript
function statusExfiltrationIndicator(log) {
```

This function evaluates HTTP status, method, response size, and download/export/archive-style targets.

To tune the threshold, modify the response-size condition.

To add file types, extend the target-extension pattern.

To change severity or wording, update the corresponding `applyBehaviorTag(...)` call in `runThreatDetectionEngine(...)`.

### Recommended approach

Do not make the indicator simply mean `HTTP 200 = exfiltration`. A successful HTTP response is normal. Combine status with:

- public source
- download/export target
- response size
- filename/extension
- frequency
- authenticated identity
- endpoint purpose

---

## 5. Add a custom parser

The main parser entry point is:

```javascript
function parseLogLine(line, context) {
```

The normalized event is produced through:

```javascript
function normalizeEvent(...)
```

### Parser workflow

1. Detect the log format.
2. Extract fields from the source line.
3. Create an object containing the available fields.
4. Pass it through `normalizeEvent(...)`.
5. Preserve the original raw line.
6. Set the parser/source information.
7. Return the normalized event.

### Example pattern

Add a format check before the generic fallback:

```javascript
if (looksLikeMyFormat(line)) {
  const parsed = parseMyFormat(line);
  return normalizeEvent({
    hostname: parsed.hostname,
    clientIP: parsed.clientIP,
    timestamp: parsed.timestamp,
    method: parsed.method,
    uri: parsed.uri,
    status: parsed.status,
    userAgent: parsed.userAgent,
    referrer: parsed.referrer,
    responseBytes: parsed.responseBytes,
    raw: line,
    parser: 'My Custom Parser'
  });
}
```

### Update an existing parser

Modify only the relevant format branch. Do not change the normalized object model unless there is a documented requirement.

### Remove a parser

Remove its format-detection branch and any helper function used exclusively by that parser.

Before removal, check whether the helper is referenced elsewhere.

---

## 6. Add a custom IR playbook

Find:

```javascript
const IR_PLAYBOOKS = {
```

Each playbook contains:

```javascript
'Custom Detection': {
  investigate: [
    'Investigation step one.',
    'Investigation step two.'
  ],
  remediate: [
    'Remediation step one.',
    'Remediation step two.'
  ]
}
```

### Add

Add a new category whose key exactly matches the detection category.

### Update

Edit the `investigate` and `remediate` arrays.

### Remove

Delete the category object.

If a detection has no matching playbook, the application uses the generic fallback in `getDetailedPlaybook(...)`.

---

## 7. Keep detection and playbook names synchronized

If a rule says:

```javascript
category: "Custom Web Shell / Backdoor"
```

the playbook key must be exactly:

```javascript
'Custom Web Shell / Backdoor': {
```

Otherwise the detailed playbook will not be selected.

---

## 8. Safe customization workflow

Recommended workflow:

```text
1. Copy the HTML file.
2. Make one logical change.
3. Test with a small sample log.
4. Test a benign example.
5. Test the expected match.
6. Check the log drawer and playbook.
7. Check filtering and export.
8. Run JavaScript syntax validation.
9. Update CHANGELOG.md.
10. Commit the change.
```

## 9. JavaScript syntax validation

Because the project is a standalone HTML file, extract the JavaScript and validate it with Node.js:

```bash
python - <<'PY'
from pathlib import Path
import re
html = Path('WebTrace.html').read_text(encoding='utf-8')
scripts = re.findall(r'<script[^>]*>(.*?)</script>', html, flags=re.S|re.I)
Path('/tmp/webtrace_check.js').write_text('\n'.join(scripts), encoding='utf-8')
PY
node --check /tmp/webtrace_check.js
```

A successful `node --check` returns exit code 0.

## 10. UI changes

The existing interface is intentionally preserved. When changing backend functionality, do not rewrite the dashboard, tabs, colors, KPI cards, or chart layout unless the requirement specifically calls for a UI change.
