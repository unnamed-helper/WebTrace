# GitHub Setup Guide

## Recommended repository name

```text
WebTrace
```

## Recommended GitHub description

```text
Air-gapped, browser-based HTTP log analysis and DFIR triage tool with local parsing, threat detection, behavioral indicators, filtering, export, and IR playbooks.
```

## Recommended topics

```text
DFIR
SOC
incident-response
digital-forensics
threat-hunting
log-analysis
web-security
http-logs
IIS
Apache
Nginx
cybersecurity
blue-team
air-gapped
security-tools
```

## Create the repository

Create an empty GitHub repository with the recommended name. The project files are already prepared locally, so do not create duplicate README, license, or `.gitignore` files during repository creation.

GitHub's documentation recommends a README because it is the first place visitors can understand what a repository does, and GitHub supports relative links to documentation files inside the repository.

## Upload with Git

Open PowerShell in the extracted project folder:

```powershell
cd path\to\WebTrace-GitHub

git init
git branch -M main
git add .
git commit -m "Initial release: WebTrace v1.0.0"
git remote add origin <YOUR-GITHUB-REPOSITORY-URL>
git push -u origin main
```

Replace `<YOUR-GITHUB-REPOSITORY-URL>` with the repository URL GitHub gives you.

## Create the release

After pushing the repository:

```powershell
git tag -a v1.0.0 -m "WebTrace v1.0.0"
git push origin v1.0.0
```

Then create a GitHub Release from the `v1.0.0` tag and optionally attach:

```text
WebTrace.html
WebTrace-GitHub.zip
```

Releases are useful for distributing a specific version while the main branch continues to evolve.

## What people should download

For normal use:

```text
WebTrace.html
```

For a complete offline copy of the project:

```text
WebTrace-GitHub.zip
```

## GitHub repository presentation

After the first push, check:

- README renders correctly.
- Documentation links open.
- License is detected.
- Topics are added.
- Repository description is set.
- Release `v1.0.0` exists.
- No real logs or confidential evidence were committed.

## Suggested first commit

```text
Initial release: WebTrace v1.0.0
```

## Suggested future commits

```text
feat: add custom parser for <format>
feat: add detection rule for <behavior>
feat: add IR playbook for <category>
fix: correct IIS field normalization
fix: improve responsive viewport handling
docs: update parser customization guide
release: v4.6
```
