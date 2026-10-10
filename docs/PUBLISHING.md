# L850 Public Release Metadata — Operator Procedure (FUTURE ONLY)

**No release is currently published.** This describes a manual procedure for a future explicitly approved publish; it is not an instruction to publish now.

## Repository structure

```text
README.md
stable/manifest.json             # Real public URL, currently published=false
schemas/manifest.schema.json      # Contract for valid public JSON
examples/manifest.published.example.json  # Intentionally invalid placeholders
docs/CURRENT.md
docs/AGENT_TASK.md
docs/PUBLISHING.md
docs/WORKLOG.md
.gitignore
```

## Roles and storage

The private game-code repo remains private. This repo publishes only release metadata. Google Drive hosts update ZIP for **manual** player download. Player Rust executable reads this repo's **public, anonymous HTTPS** raw JSON; no GitHub token or Drive API key bundled. IP, port and world selection continue to live in the existing game-world configuration, not here.

## Operator steps for each future approved patch

1. **Freeze final ZIP bytes** on operator PC. Record its actual size and SHA256:
   ```powershell
   $p = 'C:\L850-Release\L850_Update.zip'
   (Get-Item -LiteralPath $p).Length
   (Get-FileHash -LiteralPath $p -Algorithm SHA256).Hash
   ```
2. Upload this ZIP as a **new version** of the same existing Google Drive file, OR use a new public file share URL. A new file ID must replace the URL in the manifest. Do not claim old link auto-follows new file IDs.
3. In a logged-out browser, verify the Drive link is **accessible** and downloads the intended content. This may hit Google download quotas or consent pages; it is still a manual browser workflow, not a direct-download API.
4. Copy `examples/manifest.published.example.json` to a temporary local working JSON and replace every placeholder. Use a *new*, immutable `release_id`. Enter **actual** file size, hash and public share URL; keep `mandatory=false`. Validate against `schemas/manifest.schema.json` and the policy in README.
5. Check the public manifest diff for privacy leaks: no private repo content, source code, credentials, game endpoints, ZIP/EXE/DLL, internal build logs, hidden or personal URLs.
6. Only AFTER ZIP verification, write `stable/manifest.json` with `published=true`; use reviewed PR/commit. This is the final publish switch. Protect the publishing branch and require review where possible.
7. Test the exact anonymous raw URL and observe the new version. If unavailable, do not treat an empty/failed HTTP response as `UP_TO_DATE`.
8. Do not immediately reuse the same `release_id` for a different ZIP; use a new version to preserve a consistent audit trail.

## Player status contract for future Rust read-only notice

```text
published=false                       => NO_PUBLISHED_RELEASE, manual link remains separate
HTTP / JSON / schema / host error     => CHECK_UNKNOWN, never block original Launcher
published=true + no installed receipt => INSTALL_STATUS_UNKNOWN, offer manual link
published=true + same installed id    => UP_TO_DATE (only if reliable receipt)
published=true + differing id         => UPDATE_AVAILABLE, offer manual link
user clicks Drive link                => NO automatic version receipt
user presses 'I updated'              => USER_ATTESTED only, not verified install
```

Do not conflate `last_seen_release_id` with `last_installed_release_id`.

## Key/security constraints

The manifest contains SHA256 to allow content identification, but it is publicly writable by authorized repo collaborators; SHA256 and HTTPS alone **do not prove trusted publisher identity** against compromised repo control. Do not auto-install executable code. If automated updates are explicitly authorized later, use a signed manifest with public-key verification, rollback protection, atomic staging and compatibility validation.

Network use by the eventual Launcher: one GET per startup, reasonable timeout, ETag cache if needed, bounded response size, exact HTTPS raw host/path allowlist, text-only UI rendering, no shell execution, no polling loops. Avoid a required update gate that blocks first client-only smoke or existing successful logins.

## Rollback and incidents

If a release is wrong, do not silently overwrite the same identifier with different bytes. Publish a new corrective release ID or return to `published=false` only under operator approval, preserving a written record of why. This public repository should never contain a ZIP itself.
