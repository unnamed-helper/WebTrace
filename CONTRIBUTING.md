# Contributing to WebTrace

**Project:** WebTrace  
**Author / Maintainer:** Arslan Sabir  
**Current release:** v1.0.0

WebTrace is a local-first HTTP log analysis and DFIR triage tool. Contributions are welcome, including feature ideas, parser improvements, detection recommendations, IR playbooks, bug fixes, documentation, and usability improvements.

## How recommendations work

WebTrace uses a maintainer-review model:

1. Anyone can open a **Feature / Detection Recommendation** issue.
2. The recommendation is reviewed by the maintainer.
3. If useful, the maintainer can ask for clarification or request a pull request.
4. A contributor can submit a pull request with the proposed code/documentation.
5. Automated validation runs against the pull request.
6. **The `main` branch is not automatically updated by a contributor.**
7. Arslan Sabir reviews the proposed change.
8. Only an accepted pull request is merged into `main`.
9. Accepted changes are documented in the changelog and included in a future release.

This means a public recommendation or pull request is a proposal, not an automatic change to WebTrace.

## What you can recommend

You can propose:

- New log parsers
- Parser field improvements
- New threat detections
- New behavioral indicators
- HTTP status-code correlation logic
- Suspicious User-Agent indicators
- Upload/download/exfiltration indicators
- Filename and extension extraction improvements
- New IR playbooks
- Filtering and investigation features
- Export improvements
- Performance improvements
- UI/usability improvements
- Documentation and sample logs

## Before submitting code

Please test the change against representative sample logs. For detection changes, document both the expected suspicious pattern and reasonable false-positive cases.

Do not submit real customer data, credentials, tokens, private IP inventories, production logs, or other sensitive evidence.

## Pull requests

Use the pull request template. Keep changes focused and explain:

- What changed
- Why it changed
- How it was tested
- What could trigger the new detection
- Any expected false positives
- Which documentation was updated

## Maintainer approval

A pull request can be reviewed, commented on, requested for changes, accepted, or closed. Only the maintainer's merge into `main` makes the change part of the official WebTrace codebase.

## Branch protection recommendation

For the GitHub repository, configure `main` as a protected branch and require:

- Pull requests before merging
- At least 1 approving review
- Required status checks from the `Validate WebTrace` workflow
- CODEOWNER review if available
- No direct pushes to `main`
- Optional: require branches to be up to date before merging

For a single-maintainer project, set Arslan Sabir as the CODEOWNER after replacing `YOUR-USERNAME` in `.github/CODEOWNERS`.
