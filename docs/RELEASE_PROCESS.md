# Release Process

## Before release

- [ ] Open the application and confirm the dashboard loads.
- [ ] Upload Apache/Nginx sample.
- [ ] Upload IIS sample.
- [ ] Upload JSON sample.
- [ ] Confirm multiple-file ingestion.
- [ ] Confirm filters and exclusions.
- [ ] Confirm sorting.
- [ ] Confirm filename/extension extraction.
- [ ] Confirm suspicious User-Agent indicator.
- [ ] Confirm public-IP upload indicator.
- [ ] Confirm HTTP status/large-download indicator.
- [ ] Open a threat and verify the IR playbook.
- [ ] Verify CSV export.
- [ ] Test common desktop resolutions.
- [ ] Test browser resizing between 21:9 and 16:9 displays.
- [ ] Run JavaScript syntax validation.
- [ ] Update CHANGELOG.md.
- [ ] Update VERSION.

## Git release

```bash
git add .
git commit -m "Release v1.0.0"
git tag -a v1.0.0 -m "WebTrace v1.0.0"
git push origin main
git push origin v1.0.0
```

Create a GitHub Release from the tag and attach the standalone HTML file if desired.
