# Pull Request Templates

Copy the section you need into your PR description, or save it as its own file in `.github/PULL_REQUEST_TEMPLATE/`.

## Contents

1. Refactor
2. Hotfix / Urgent Production Fix
3. Database Migration
4. Dependency Update
5. Documentation Only
6. Performance Improvement
7. Security Fix
8. API Change
9. Infrastructure / CI / Config
10. Release PR

---

## 1. Refactor

```markdown
## What is being refactored
<!-- Which code or module, and why now? -->

## Behavior change
- [ ] No behavior change intended
- [ ] Behavior changes (describe below)

## Approach
<!-- Key structural changes, moved or renamed files -->

## How I verified nothing broke
<!-- Existing tests passing, manual checks, benchmarks -->
```

---

## 2. Hotfix / Urgent Production Fix

```markdown
## Incident
<!-- Link to incident, ticket, or alert -->

## Impact
<!-- Who or what is affected, and how badly? -->

## Fix
<!-- Smallest change that resolves it -->

## Risk
<!-- What could this break? -->

## Rollback plan
<!-- How to revert quickly if it goes wrong -->

## Follow-up
<!-- Permanent fix, postmortem, or cleanup ticket -->
```

---

## 3. Database Migration

```markdown
## Ticket
<!-- PROJ-123 -->

## Migration summary
<!-- Tables/columns added, changed, or removed -->

## Backward compatible?
- [ ] Yes, old code works with the new schema
- [ ] No (explain deploy order below)

## Data impact
<!-- Row counts, backfill needed, expected run time, locking risk -->

## Rollback
<!-- Down migration or manual steps -->

## Tested on
- [ ] Local
- [ ] Staging with production-like data
```

---

## 4. Dependency Update

```markdown
## Package
<!-- name: old version -> new version -->

## Why update
<!-- Security fix, new feature, end of support -->

## Breaking changes
<!-- Link to changelog; list anything that affects us -->

## Verified
- [ ] Build passes
- [ ] Tests pass
- [ ] App smoke-tested locally
```

---

## 5. Documentation Only

```markdown
## What changed
<!-- Pages or sections updated -->

## Why
<!-- Outdated, missing, unclear, or new feature -->

## Checked
- [ ] Links work
- [ ] Code examples run
- [ ] Spelling and formatting reviewed
```

---

## 6. Performance Improvement

```markdown
## Problem
<!-- What was slow, and where was it measured? -->

## Change
<!-- What you optimized -->

## Results
| Metric | Before | After |
|--------|--------|-------|
|        |        |       |

## Trade-offs
<!-- Memory use, complexity, cache invalidation, etc. -->
```

---

## 7. Security Fix

```markdown
## Vulnerability
<!-- Type and severity. Don't include exploit details in a public repo. -->

## Reference
<!-- CVE, advisory, or internal ticket -->

## Fix
<!-- High-level description of the change -->

## Verification
- [ ] Fix tested
- [ ] No secrets or sensitive data in the diff
- [ ] Related code paths checked for the same issue
```

---

## 8. API Change

```markdown
## Endpoints affected
<!-- e.g. POST /v1/orders -->

## Change type
- [ ] New endpoint
- [ ] Modified request/response
- [ ] Deprecation
- [ ] Breaking change

## Compatibility
<!-- Impact on existing clients; versioning plan -->

## Docs
- [ ] OpenAPI / API docs updated
- [ ] Changelog entry added
```

---

## 9. Infrastructure / CI / Config

```markdown
## Change
<!-- Pipeline, Terraform, Docker, env vars, etc. -->

## Environments affected
- [ ] Dev
- [ ] Staging
- [ ] Production

## Risk and rollback
<!-- What happens if it fails, and how to undo it -->

## Secrets or variables needed
<!-- New values that must be set before merge -->
```

---

## 10. Release PR

```markdown
## Version
<!-- v1.4.0 -->

## Included changes
<!-- List of merged PRs or tickets -->

## Release checklist
- [ ] Changelog updated
- [ ] Version bumped
- [ ] Migrations reviewed
- [ ] Smoke tests passed on staging
- [ ] Release notes drafted
```
