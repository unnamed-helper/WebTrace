# WebTrace Contribution and Approval Workflow

WebTrace is designed so that the community can suggest improvements without automatically changing the official codebase.

## 1. Feature recommendations

Use GitHub Issues → **Feature / Detection Recommendation** for ideas such as:

- a new parser
- a new detection rule
- a new threat indicator
- a new IR playbook
- a new filter or investigation capability

The issue is a recommendation only. It does not modify the repository.

## 2. Code proposals

When a contributor is ready to implement an accepted idea, they can open a pull request against `main`.

The pull request should contain the code and documentation changes and explain how the change was tested.

## 3. Automated checks

Every pull request runs the `Validate WebTrace` GitHub Actions workflow. The workflow checks the main HTML file and repository version metadata.

## 4. Human review

Arslan Sabir reviews the proposed change. Possible outcomes are:

- Approved and merged
- Changes requested
- Closed without merge

A pull request is never treated as an accepted WebTrace change simply because it was submitted.

## 5. Protecting main

Configure GitHub branch protection for `main` so contributors cannot push directly to the release branch. Require a pull request and an approving review.

Recommended settings:

- Require a pull request before merging
- Require at least 1 approving review
- Require `Validate WebTrace` to pass
- Require CODEOWNER review
- Disable direct pushes for normal contributors

## 6. Release

After an accepted change is merged, update the version/changelog when appropriate and create a release tag. See `docs/RELEASE_PROCESS.md`.
