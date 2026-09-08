# Upstream Synchronization Runbook

This runbook keeps the FranklyBuilds customization branch current with [Wei-Shaw/sub2api](https://github.com/Wei-Shaw/sub2api).

## Branch contract

- `main` is the upstream mirror. Do not add product or brand changes to it.
- `fbspecial` is the tested FranklyBuilds integration branch.
- `feature/*` branches begin at `fbspecial` and merge back into `fbspecial` through a pull request.
- `release/*` branches are short-lived release candidates created from `fbspecial`.

## Regular upstream update

Run weekly, and immediately for an upstream security, authentication, payment, or data migration change.

```powershell
# Refresh remote references
git fetch origin --prune
git fetch upstream main --tags

# Update the clean upstream mirror
git switch main
git merge --ff-only upstream/main
git push origin main

# Integrate the mirror into the custom branch
git switch fbspecial
git merge --no-ff main -m "chore(sync): integrate upstream sub2api"

# Validate before pushing
# Run the repository's documented backend/frontend build, tests, and migration checks.
git push origin fbspecial
```

If the first merge cannot fast-forward, stop: `main` contains local customization and must be reconciled before the next synchronization.

## Required validation after integration

1. Build the frontend and backend.
2. Start a clean test environment and apply migrations once.
3. Verify login, subscription, payment, redemption, and admin access.
4. Verify every enabled custom module, including lottery eligibility, inventory decrement, audit record, and reward delivery.
5. Update `docs/CUSTOMIZATION.md` with the upstream SHA if the integration will be released.

## Release procedure

```powershell
git switch fbspecial
git pull --ff-only origin fbspecial
git switch -c release/YYYY.MM.N
# Only release-blocking fixes are allowed here.
# After validation:
git tag -a v<upstream-version>-fb.<N> -m "FranklyBuilds release"
git push origin release/YYYY.MM.N
git push origin --tags
```

Record the upstream SHA, fbspecial SHA, migration version, test evidence, and rollback instructions in the release notes. Keep database changes forward-compatible; application rollback must not require a database rollback.