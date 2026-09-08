# Customization Register — sub2api-fbspecial

## Upstream baseline

- Upstream: https://github.com/Wei-Shaw/sub2api
- Upstream branch: `main`
- Last synchronized commit: `270eac6973049fe1b50eb75560a74a029e82884c`
- Baseline version: `0.2.3`
- Integration branch: `fbspecial`

## Custom extension boundaries

Use dedicated extension directories for FranklyBuilds changes:

- `frontend/src/custom/branding/`: brand name, logo, theme tokens, navigation, footer, notices.
- `frontend/src/custom/features/`: standalone frontend features such as lottery pages.
- `backend/internal/custom/`: standalone backend modules such as lottery eligibility, inventory, audit, and rewards.

Keep upstream authentication, payment, subscription core flows, shared ORM infrastructure, and existing migrations unchanged whenever possible.

## Direct upstream modifications

Every intentional edit to an upstream-owned file must:

1. Include an `FB-SPECIAL:` code comment where appropriate.
2. Be recorded below with the reason and a path to decouple it later.
3. Be isolated in its own commit or pull request.

| Path | Change / PR | Reason | Decoupling plan |
| --- | --- | --- | --- |
| _None yet_ | — | — | — |

## Release metadata

Every production release must record:

- upstream commit SHA and upstream version;
- fbspecial commit SHA and release tag;
- applied database migration version;
- verification and rollback notes.