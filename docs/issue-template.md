# Issue record template

Copy and fill this format for a **single** problem. Explicitly distinguish an observed symptom from an established cause.

```markdown
## ID — Short descriptive title

- **Evidence class:** OFFICIAL CONSTRAINT / UPSTREAM ISSUE / FIELD REPORT / HYPOTHESIS
- **Product / exact version / relevant firmware:** 
- **Environment and triggering conditions:**
- **First observed (YYYY-MM-DD, if known):**
- **Last observed (YYYY-MM-DD, if known):**
- **Upstream issue status verified on (YYYY-MM-DD, if actually checked):**
- **Symptoms / exact error text (redacted):**
- **Expected behavior:**
- **Reproduction attempts and results:**
- **Evidence links:** Official vendor docs / GitHub issue and PR / field report
- **Affected versions and fixed versions (if confirmed):**
- **Operational workaround (if any):**
- **Underlying cause confidence:** Confirmed / Suspected / Unknown
- **AI pre-install warning and checks:**
- **Operational status:** Fixed here / Workaround / Still occurring / Unknown
- **Privacy check:** No personal domains, live addresses, account names, tokens,
  passwords, certificates, MACs, serials, or unredacted screenshots/logs
```

## Rules for issue reporting

1. A field report is **not automatically** a vendor defect.
2. Do not assert “still unresolved as of DATE” without checking official release notes or trackers **on that date**.
3. A closed GitHub issue can mean fixed, duplicate or wontfix; confirm whether a fix shipped in the user's precise release.
4. Separate confirmed supported-version constraints from RC/Beta risk advisories.
5. State **Unknown** where facts are missing. Dates of observation and external verification are different.
6. No destructive commands, exposed credentials or private deployment details in a public report.
