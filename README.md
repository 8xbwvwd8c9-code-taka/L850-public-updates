# L850 — Public Update Metadata

This **public** repository is reserved for non-sensitive L850 patch **release metadata**, not game source code or a game update server.

**Current status: PLANNING / UNPUBLISHED.** No downloadable release is asserted, no player client update is enabled, and no patch is automatically downloaded or installed.

## Responsibilities

| Service | Contents | Access |
| --- | --- | --- |
| This public repository | `stable/manifest.json`, public release info, validation and operator instructions | Anonymous HTTPS read |
| Google Drive | Operator-approved client patch ZIP | Manual browser download |
| Private product repository | Launcher, Helper, AutoHunt, server code, private authority | **Never copied here** |

A future Rust Launcher can read the **small** `stable/manifest.json` anonymously. If `published=false`, it must display `NO_PUBLISHED_RELEASE` / `CHECK_UNKNOWN`, never "up to date". The existing Launcher must not require this endpoint to start the game.

## Read order for agents

1. This `README.md`
2. `docs/CURRENT.md`
3. `docs/AGENT_TASK.md`
4. `docs/PUBLISHING.md`
5. `schemas/manifest.schema.json` and `examples/manifest.published.example.json`

## Public endpoints (already allocated; not a published update)

- Public metadata: [stable/manifest.json](stable/manifest.json)
- Anonymous raw path: `https://raw.githubusercontent.com/8xbwvwd8c9-code-taka/L850-public-updates/main/stable/manifest.json`
- The raw endpoint is **not** a release until it explicitly has `published=true` with verified metadata.

## Publishing invariant

1. Finalize update ZIP locally and compute **actual** SHA256 and size.
2. Upload the intended ZIP to Google Drive and validate the public download in a logged-out session.
3. Validate manifest JSON using the approved schema and release safety rules.
4. Commit the `published=true` manifest **last** to make an update visible.
5. Never replace an already published `release_id` with different content. Use a new release ID.

## Security and scope

- **NO** API tokens, OAuth credentials, SSH keys, executable clients, native DLLs, patch ZIPs, private repo archives, world IPs/ports or source trees in this repo.
- A public SHA256 from the same manifest detects inadvertent differences, **not** malicious repository takeover. Signed releases are required before automated patch installation is considered.
- `status=unpublished` means **there is no authorized current release**. Never fabricate a ZIP size or SHA256 to fill it.
- No GitHub Action publishing, automatic download/eat, game startup or private repo linkage is enabled by this scaffold.

**Owner decision 2026-10-10:** Plan-only groundwork is authorized. Notification implementation and updater/eat execution remain **PAUSED** until separately requested.
