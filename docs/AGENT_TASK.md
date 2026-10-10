# L850 Public Update Notice — Agent Entry (PLANNING MODE)

```text
TASK_ID=LAUNCHER-PUBLIC-MANIFEST-BOOTSTRAP-20261010
LANE=LAUNCHER_HELPER_INTEGRATION
OWNER_REPO=8xbwvwd8c9-code-taka/L850-public-updates
BRANCH=main
WORK_MODE=PLAN_AND_REVIEW_ONLY
PRODUCT_SOURCE_OWNER=SEPARATE_PRIVATE_REPOSITORY
CURRENT=docs/CURRENT.md
MANIFEST=stable/manifest.json
PUBLIC_RELEASE=NONE
RUST_NOTICE_IMPLEMENTED=NO
AUTO_DOWNLOAD=PAUSED
AUTO_EAT=PAUSED
NEW_CLOUD_SERVICES=NO
```

## TARGET

Provide an anonymous-read, **credential-free** public update-notification metadata contract for a future `850Rust.exe`, while preserving the user's manual Google Drive distribution and current launcher-priority work. Public repo is only the distributor of verified release *metadata*.

## KNOWN

- Private source repo remains private. Never mirror or copy it here.
- Public repo exists and is public; `main` is default. Plan-only scaffolding is authorized.
- Public stable manifest is `published=false`, `release=null`: no actual patch release has been approved.
- Google Drive download must remain **manual**, no Google Drive API key, no OAuth, no GitHub token in player EXE.
- Editing the same Google Drive file without publishing a new manifest does **NOT** notify players.
- Uploaded ZIP SHA, size, public share accessibility and installed version receipt are unverified.

## DO — NOW (DOCUMENTATION / REVIEW ONLY)

1. Read `README.md`, `docs/CURRENT.md`, `docs/AGENT_TASK.md`, `docs/PUBLISHING.md`, schema and example in that order.
2. Review `stable/manifest.json` for disabled status; ensure examples contain invalid placeholders and are never live.
3. Review release schema: `published=false/release=null` vs `published=true/release=verified record`; ensure no secrets or proprietary source.
4. Produce a review report and identify missing operator inputs; do not fill in fictitious values.
5. If editing public docs to correct an issue, preserve scope and `published=false` and update `docs/WORKLOG.md` with exact commit SHA if known.

## FUTURE WORK, ONLY AFTER USER EXPLICITLY UNPAUSES

- Prepare exact remote manifest URL and design Rust offline/mock notice UI.
- Four primary states: NO_PUBLISHED_RELEASE / CHECK_UNKNOWN / INSTALL_STATUS_UNKNOWN / UPDATE_AVAILABLE and UP_TO_DATE with a reliably installed receipt.
- Unit tests for valid disabled payload, invalid published values, malicious URL, unknown/duplicate JSON fields, changes A→B, already installed A, failure modes 403/429/timeout, and 1 request/start.
- Keep all private Rust source edits on the owning private branch, **not in this public metadata repo**.
- Publishing `published=true` requires separate verified ZIP release authorization; never infer this permission from agent assignment.

## PASS

`PUBLIC_METADATA_ONLY=YES`; schema/sample/readme/docs are coherent; raw manifest anonymously readable; no release advertised; no secrets or private code; paused gates untouched.

## STOP

Do not commit private content, new real Drive links, real hashes that were not independently verified, live release, executable artifacts, login scripts, private repository source, Rust code, cloud secrets, HFS, an update server, automatic download/eat, game process changes or unrelated branch merges.

## RETURN

`HEAD|PUBLIC=YES|MANIFEST_URL|UNPUBLISHED=YES|SCHEMA_OK|DOCS_OK|PRIVATE_MIRROR=NO|CODE_TOUCHED=NO|NEXT=WAIT_FOR_EXPLICIT_AUTHORIZATION`


## WebView2 UI/IPC proposal review (documentation only)

User-proposed WebView2 JS `fetch()` plus existing Rust Wry IPC is a **conditional** design choice, not an accepted runtime PASS. Read `docs/WEBVIEW2_NOTICE_DESIGN_20261010.md` and review the `with_html` origin `null`, GitHub Raw CORS compatibility, bounded `UpdateManifestObserved` IPC, valid published schema and independent update/game-launch indicators. The existing `檢查更新` label means recheck; `雲端硬碟` opens the approved browser link; when a newer release exists, use a visibly labeled `下載更新` CTA rather than a silently repurposed check button. Network failure is UNKNOWN (not green) and never a new game-launch block. No actual Windows CORS or anonymous raw fetch PASS is claimed, and source/EXE changes remain STOP.
