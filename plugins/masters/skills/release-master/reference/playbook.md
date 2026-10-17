# release-master expert playbook

Read before Phase 2 and before delegating to any child. On conflict this file beats the child. Copy its gate additions into SPEC.md Done-when.

## Routing overrides
- **Never use `version-bump` or `release`** (bundled or the installed `claude-mem:` versions). They are the claude-mem author's private release script: hard-coded manifests, `git add -A`, push to main, npm publish and a Discord webhook. Use the generic bump procedure below.
- Safety overrides cover every tag, push, deploy (including previews), GitHub release, npm/PyPI publish and webhook, whichever child runs it: a fresh yes naming the exact command.
- **commit / commit-push-pr**: secret scan before every commit (staged diff for keys, tokens, `.env`); match the repo's convention (`git log -20 --oneline`); keep any required `Co-Authored-By` trailer; ask before pushing even though commit-push-pr says "single message".
- **clean-gone**: run `git fetch --prune` first, otherwise `[gone]` is stale. Show every branch and worktree it would remove and get a yes before any deletion.
- **oh-my-issues**: for 20+ issues, cluster by root cause. It has no labels or priority triage; do that yourself. Closing issues is public: draft, show, then a yes. Do not mass-close.
- **childhood**: post-ship watch only; ignore its claude-mem paragraphs and dead `babysit`/`version-bump` links.
- **verification-before-completion**: run tests and a clean-checkout build before tagging.
- Shell snippets are POSIX: Git Bash on Windows.

## Generic bump procedure
1. Detect the scheme: SemVer, CalVer or PEP 440; note pre-release tags (rc, beta, npm dist-tags).
2. Run tests and build from a clean checkout.
3. Choose the bump from the public-API diff (breaking = MAJOR; feature = MINOR; fix = PATCH; 0.x may break in MINOR). Show the evidence.
4. Bump only the files `git grep <old version>` shows.
5. Write a Keep a Changelog entry dated today (Added, Changed, Deprecated, Removed, Fixed, Security).
6. One release commit on a branch or PR.
7. One yes per command, each shown first: `git tag -s vX.Y.Z -m "vX.Y.Z"` (`-a` if no signing key) on the tested commit; `git push origin vX.Y.Z`; `gh release create vX.Y.Z --verify-tag --draft --generate-notes`; attach assets with SHA-256 checksums; then publish the draft. GitHub immutable releases lock assets and tags once published.
8. Verify with `gh release view` and `git ls-remote --tags`.

## Project types
- **npm**: trusted publishing (OIDC) with provenance needs npm CLI 11.5.1 or newer; long-lived tokens are gone and granular tokens expire within 90 days.
- **Windows app** (e.g. Cadence): signed installer or zip, SHA-256 checksums, version in the About screen, a smoke run of the built artifact.
- **Static site** (e.g. MCU site): deploy preview first, keep the previous build for one-command rollback, check key URLs after deploy.
- **Rollback / yank**: write the steps before releasing (`npm deprecate`, PyPI yank, never move or delete the released tag: ship a fixed patch release).

## Gate additions
- Tests and a clean build passed on the tagged commit; changelog entry in that commit.
- Every public action ran only after a yes naming its command.

## Sources to require
semver.org 2.0.0 · keepachangelog.com 1.1.0 · Conventional Commits 1.0.0 · calver.org · PEP 440 · GitHub Docs: releases, `--generate-notes`, `.github/release.yml`, immutable releases · docs.npmjs.com trusted publishers and provenance · SLSA and OpenSSF Scorecard · Google SRE Book ch. 8 "Release Engineering" · Chris Beams "How to write a git commit message" · Google eng-practices (CL descriptions).
