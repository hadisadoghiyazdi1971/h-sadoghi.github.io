# CODEX TASK — h-sadoghi.github.io — Phase 01: Isolated Baseline Build

**Date:** 2026-10-17  
**Target repository (read-only):** `J:\SadoghiSite2\h-sadoghi.github.io`  
**Known repo:** `https://github.com/h-sadoghi/h-sadoghi.github.io.git`  
**Previous discovery report:** `SITE_ARCHITECTURE_REPORT.md` in the repository root.  
**Language for human-facing report:** Persian. Preserve commands and technical identifiers in English.

## Overall objective

Establish a verifiable, non-destructive **baseline build** of the existing academic Jekyll/al-folio website. Discover actual reproducible build failures and warnings. **Do not fix source code during this phase.** Do not deploy or publish. The owner will send the final single handoff ZIP to ChatGPT for review before authorizing any code changes.

## Hard safety requirements (non-negotiable)

1. Treat `J:\SadoghiSite2\h-sadoghi.github.io` as **read-only**. Do not edit, delete, format, auto-fix, or overwrite anything within it. Do not create build/cache/log files in the original root.
2. No `git commit`, `git push`, `git pull`, remote changes, branch checkout/switch, reset, clean, rebase, merge, force-push, workflow dispatch, Pages settings change, or deployment. Do not access the `gh-pages` branch.
3. **Do not run** `bin/deploy`, `bin/entry_point.sh` against the original repository, or `docker compose` with the original repository as a bind mount. The discovery report warns that these scripts may delete `Gemfile.lock` or force-push.
4. Do not modify `papers.bib`, YAML/Markdown, images, GitHub workflows, dependencies or `Gemfile.lock` in the original repository. No BibTeX deduplication, CV fallback edits or 'cleanup' in Phase 01.
5. Do not request GitHub login credentials or tokens. Never copy or report credentials, personal environment variables, secrets or the contents of `.git/config` if credential-bearing. Avoid installing software globally or invoking elevated/admin operations.
6. Use a **sibling sandbox outside the original repository**, for example `J:\SadoghiSite2\h-sadoghi_stabilization_sandbox\`; outputs and logs go inside that sibling sandbox. If it already exists and contains user work, do not overwrite it: choose a new uniquely suffixed folder.
7. If safe build dependencies are unavailable, or a step would require an OS-wide installation or destructive mutation, **STOP and report a clear blocker** rather than guessing or weakening safety restrictions.
8. Do not claim a successful website build based solely on `bundle install`, static checks, or an earlier GitHub Actions run. Capture the actual local command, exit code, and log.

## Step A — Verify original repo without changing it

- Inspect `git status --porcelain=v1 --untracked-files=all`, `git branch --show-current`, `git rev-parse HEAD`, and remote **names and host/path only**. Expect `master`, HEAD beginning with `8512a9f` in the earlier report, but record reality instead of forcing the earlier state.
- Check `git diff --quiet` and `git diff --cached --quiet` for tracked changes. If any tracked source changes exist, do **not** proceed with a HEAD-only archive pretending it equals the working tree. Report and stop; do not discard changes.
- Record source state before running anything (`original_git_before.txt`). Existing untracked files, especially `CODEX_H_SADOGHI_SITE_DISCOVERY.md` and `SITE_ARCHITECTURE_REPORT.md`, are expected and must not be removed.
- Verify the repository really contains `_config.yml`, `Gemfile`, `Gemfile.lock`, `_pages/`, `_bibliography/papers.bib`, and `.github/workflows/deploy.yml`.

## Step B — Make an immutable-snapshot equivalent test copy

- Prefer Git archive of the verified local `HEAD`, written **outside** the original repo, then expand into a new sandbox `source/`. Example for Windows PowerShell (adapt quoting as needed):

```powershell
$repo = 'J:\SadoghiSite2\h-sadoghi.github.io'
$root = 'J:\SadoghiSite2\h-sadoghi_stabilization_sandbox'
# FIRST ensure $root does not already contain user work; otherwise choose a new path.
New-Item -ItemType Directory -Path "$root\source" -Force | Out-Null
& git -C $repo archive --format=zip --output="$root\source_at_head.zip" HEAD
if ($LASTEXITCODE -ne 0) { throw 'git archive failed' }
Expand-Archive -LiteralPath "$root\source_at_head.zip" -DestinationPath "$root\source"
```

- This archive contains tracked HEAD files only and intentionally excludes `.git` and untracked reports. Note that explicitly in the report. Do not confuse it with a working-tree snapshot if tracked modifications exist.
- Build only inside this sandbox (or an isolated container/WSL copy of it) and save all logs outside the original repository.
- Record source commit, archive SHA-256, and exact local paths. Verify required files exist in the copied `source/` tree.

## Step C — Check build environment and select one safe approach

- Inspect (without installation or global configuration changes) Git, Ruby/Bundler, available WSL distributions, Docker availability, Python/Jupyter and ImageMagick. Capture versions, or `NOT_AVAILABLE`, in `environment.txt`. Do not dump the full environment or secrets.
- The audited config uses Jekyll 4.3.3 and `Gemfile.lock`; GitHub Actions specifies Ruby 3.2.2. `Gemfile.lock` does not explicitly list native Windows as a platform. Therefore **prefer an already-working WSL/Linux or isolated Docker environment** if available. Native Windows may work, but do not edit the lock to force it.
- Never bind-mount the original project into Docker/WSL for writes. If using Docker, only mount the sandbox copy. Avoid `docker-compose.yml`/`bin/entry_point.sh` unless individually validated and limited to the sandbox; a direct isolated build is preferred.
- Project-specific gems and pip packages may be installed into **sandbox-local** paths if available tools permit it and it does not require administrator rights or global modification. Set Bundler to use the sandbox, and keep `Gemfile.lock` frozen when possible. Clearly log packages installed and their scope. If OS-level prerequisites, network, certificates or Ruby compatibility block installation, stop rather than silently altering the original lock.
- The site uses Jekyll Scholar, external post retrieval and image processing; lack of network or optional tools must be identified separately from source bugs.

## Step D — Execute the baseline build, not a deployment

- In the isolated sandbox, install dependency sets as safely feasible; log exact commands and exit codes. Never silently auto-update versions, remove site features, disable plugins or rewrite config to make build pass.
- Preferred actual build command, adapted to chosen environment with destination remaining inside the sandbox:

```bash
JEKYLL_ENV=production bundle exec jekyll build --trace
```

- Capture full stdout, stderr, wall time (if possible), runtime versions and exit code in `build.log` and `build_result.json`.
- If and only if Jekyll build succeeds, note generated `_site` existence and check that key expected outputs such as the landing page, `/publications/`, `/projects/`, `/people/`, `/teaching/`, `/cv/`, and `/repositories/` are present in the sandbox. Do not treat mere output file existence as proof pages work in a browser.
- Do **not** run a deployment, push, production workflow or `bin/deploy`. No need for HTML PurgeCSS in this first baseline if not already configured; record whether it was skipped. Do not mislabel an untested full production pipeline as complete.
- Do not fix failure(s) within this phase. One faithful attempt is preferable to undocumented trial-and-error. If a failure has an obviously environment-only cause, one clearly documented additional attempt is acceptable without changing source/lock/config; label the two attempts distinctly.

## Step E — Evidence-based triage ONLY (no changes)

Make a short table of every significant problem discovered:

- `ERROR_SOURCE`: source or configuration defect with file/line and log evidence;
- `ERROR_ENVIRONMENT`: missing Ruby/WSL/Docker, gems, permissions, OS incompatibility or network;
- `EXTERNAL_DEPENDENCY`: Scholar/Medium/Google/CDN requests and similar external causes;
- `WARNING`: non-fatal warning;
- `UNVERIFIED`: suspected issue not actually exercised.

For each item include: concrete evidence (log excerpt reference/line), effect, confidence and suggested smallest future remedy. **Do not declare earlier findings (such as 12 duplicate BibTeX keys, the WiFiLocation case mismatch, a missing resume JSON, or incorrect repository.yml entries) to be fixed or build failures unless this run demonstrates it.** You may mention them under `UNVERIFIED`, linked to the earlier discovery report.

## Step F — Verify original unchanged

- Rerun original `git status --porcelain=v1 --untracked-files=all`, `git rev-parse HEAD`, `git diff --exit-code`, and `git diff --cached --exit-code`.
- Compare beginning/end untracked status and confirm there were **no new files, tracked file changes, branch changes, or HEAD changes** in original repo. Document exceptions truthfully if any; do not undo anything automatically.
- Only the isolated sandbox may contain dependency/cache/site output and phase reports.

## Required handoff: ONE ZIP only

Create a compact `PHASE01_HANDOFF.zip` in the sandbox containing (where available):

- `PHASE01_REPORT.md` — Persian executive summary: exact source/HEAD, sandbox location, tested build platform, `SUCCESS`/`FAILED`/`BLOCKED_ENVIRONMENT`, actual command, exit code, evidence table, next recommended action, and whether the original remained untouched;
- `original_git_before.txt` and `original_git_after.txt` — Git state/status with credentials excluded;
- `environment.txt` — tools and versions; no secrets;
- `commands_used.txt` — actual commands, working directories, exit codes;
- `build.log` — full build error/warning record if executed (redact secrets only, do not hide diagnostics);
- `build_result.json` — build status, timestamp, exit code and output paths;
- `sandbox_manifest.txt` — source HEAD, source archive SHA-256 and key build output paths.

Do not include `_site/`, `vendor/`, dependency caches, private credentials or the full cloned repository in the handoff ZIP. If a log is huge, retain the complete local log but package a meaningful capped extract with explicit location/size of original. Keep the package small and easy to upload.

**Final Codex chat reply:** Give exactly the test outcome (success / failed / blocked), one-line cause if failed, absolute path to `PHASE01_HANDOFF.zip`, and the statement `No source changes; no commit/push/deploy` only if actually verified. **Stop and await approval for Phase 02.**
