# Workflow: Dev → QA → Merge

## Trigger
A PR is opened or marked ready for review in any core platform repo (`vm`, `flcc`, `actions`, `rhythm`, or a feature repo).

## Steps

| Step | Owner | Action |
|------|-------|--------|
| 1. PR opened | Dev | Opens PR, adds `gschleic` as reviewer via API, posts `@gschleic ready for your merge` comment |
| 2. QA gate request | Dev | Messages QA with: repo, PR branch, test level requested (smoke minimum; regression for significant changes) |
| 3. Test run | QA | Runs appropriate test level against PR branch; posts results as PR comment |
| 4. Gate decision | QA | ✅ Pass → notifies Dev to proceed; ❌ Fail → posts findings, notifies Dev and Fix if it's a bug |
| 5. Fix loop (if needed) | Fix / Dev | Fix resolves failing tests; Dev re-requests QA once fixed |
| 6. Merge | Glenn | Glenn merges after QA passes |

## Handoff conditions

- **Dev → QA**: PR branch is built and stable locally; Dev has done a basic smoke check
- **QA → Dev (pass)**: All requested test levels green; no new failures introduced
- **QA → Dev/Fix (fail)**: Clear failure description with repro steps in PR comment
- **Dev → Glenn**: QA pass confirmed; PR comment updated

## Notes
- For hotfixes, smoke only is acceptable before merge; regression runs after
- QA does not block on `low` PRs (doc changes, non-code updates) — Dev can self-certify
- See `qa/test-strategy.md` for test level definitions
