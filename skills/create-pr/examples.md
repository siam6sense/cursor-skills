# Create PR examples

## Good

**Title:** `feat(payment): persist review session client secret (EG-2401)`

**Body:**

```markdown
## Summary
- Keep Stripe client secret across Review & Pay remounts so customers are not charged twice
- Scope storage to haul + amount fingerprint

## Jira
- Key: EG-2401
- Link: https://your-domain.atlassian.net/browse/EG-2401

## Test plan
- [ ] Open Review & Pay, leave, return — same PaymentIntent
- [ ] Change haul total — new session created
- [ ] Unit tests for session read/write pass

## Size
- Changed lines vs main: 142 (budget 200)
```

## Bad

**Title:** `Update payment stuff` — not Conventional Commits, no Jira key

**Title:** `feat(payment): persist review session client secret and also refactor checkout provider and update docs (EG-2401)` — too long / multi-concern (likely over size budget)

**Body missing Jira / Size** — breaks traceability and DORA preflight visibility

**Opening at 340 lines** — violates hard gate; split before `gh pr create`
