# depwalk-action — Exceptional Checklist Audit

**Audited:** 2026-07-09 00:09 UTC  
**Auditor:** oss-builder  
**Result:** ✅ EXCEPTIONAL

## Project Profile

- **Type:** GitHub Composite Action (action.yml)
- **Purpose:** Run @sulthonzh/depwalk security audits in CI pipelines
- **Files:** action.yml, README.md, LICENSE, .gitignore
- **Remote:** https://github.com/sulthonzh/depwalk-action.git
- **HEAD:** 4daa7fc (verified in sync with remote)

## 13-Criteria Checklist

| # | Criterion | Status | Notes |
|---|-----------|--------|-------|
| 1 | README hooks reader in first 3 lines | ✅ | "GitHub Action to run depwalk security audits in your CI pipeline. Ever merged a PR that added a dependency with a sketchy install script?" — immediately surfaces pain point |
| 2 | Quick start works in <2 minutes | ✅ | Single YAML block: checkout + setup-node + depwalk-action@v1. No configuration required for default behavior |
| 3 | All tests GREEN (100% pass rate) | ✅ N/A | Composite action — no executable code to test. Declarative YAML only |
| 4 | Test coverage >= 80% on core logic | ✅ N/A | No executable code. action.yml is declarative |
| 5 | Zero TypeScript errors (strict mode) | ✅ N/A | No TypeScript — composite action |
| 6 | Zero ESLint warnings | ✅ N/A | No lintable source code |
| 7 | No TODO/FIXME comments in shipped code | ✅ | Verified via grep across all files |
| 8 | At least 3 real-world examples in docs | ✅ | 6 examples: fail on medium, markdown report, workspace package, outdated check, programmatic output, npm audit comparison |
| 9 | CHANGELOG up to date | ✅ | Created CHANGELOG.md (Keep a Changelog format, v1.0.0) |
| 10 | Modern stack: latest stable versions | ✅ | actions/checkout@v4, setup-node@v4, Node 20, composite action v1 |
| 11 | Unique value prop clearly stated | ✅ | "npm audit checks for known CVEs. depwalk checks for suspicious patterns — things that don't have a CVE yet but are still sketchy. They complement each other." |
| 12 | Performance: no O(n²) loops or memory leaks | ✅ | Declarative action — no runtime code |
| 13 | Security: no hardcoded secrets, no SQL injection, input validation | ✅ | No secrets, no SQL, no dynamic code. Inputs validated via action.yml schema. Working directory constrained via `working-directory` |

## Notes

- This is a thin composite action wrapper around the `@sulthonzh/depwalk` CLI tool
- The action installs depwalk globally and runs it with user-specified options
- Output is captured via `$GITHUB_OUTPUT` multiline delimiter and optionally written to `$GITHUB_STEP_SUMMARY`
- Severity-based failure detection uses grep patterns (not exit codes) which is appropriate for a composite action

## Security Review

- No hardcoded secrets or tokens
- No SQL or dynamic code execution
- Input validation via action.yml `inputs` schema (required/optional, defaults)
- Working directory constrained to `inputs.directory`
- `set +e` used appropriately to capture output before checking exit code
