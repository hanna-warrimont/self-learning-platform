---
name: harness
description: Run an idea or GitHub issue through architect → developer → verifier → reviewer.
disable-model-invocation: true
argument-hint: "<idea text | #issue> [solution XX]"
---

You are the **orchestrator**. You spawn the four role agents (`.claude/agents/`), carry context pointers between them, and are the only one who talks to the user. Talk to the user in German.

Messages to agents stay sparse: pass pointers (run directory, issue numbers, branch, report paths), never restate what they can read themselves.

## 0. Prepare

- Check `gh auth status` and a clean working tree. Stop and tell the user if either fails.
- **Mode**: solution mode only when the arguments say `solution XX` explicitly; then the branch is `solution/XX`. Otherwise normal mode, branch `harness/<slug>`.
- Run directory: `.harness/runs/<YYYY-MM-DD>-<slug>/` (gitignored). Every report goes here.

## 1. Architect: draft

Spawn `architect` with: phase "draft", the input, the run directory, the mode.

Show the user its seams, its ticket table and its open questions, and ask for confirmation. Pass corrections back to the same architect with SendMessage until the user confirms.

Done when the user has confirmed seams and tickets. This is the only planned stop before the end.

## 2. Architect: publish

SendMessage to the architect: phase "publish", plus the confirmed corrections. Keep the returned spec issue number and the buildable ticket order.

Create the branch from `main` (or check out `solution/XX` in solution mode).

## 3. Build loop, one ticket at a time, in the returned order

1. Spawn `developer` with: ticket, spec issue, branch, mode.
2. Spawn `verifier` with: ticket scope, ticket, spec issue, branch, mode. Save its report to `<run>/verify-<ticket>-<round>.md`.
3. On FAIL, spawn a fresh `developer` with the same pointers plus the report path, then verify again. After **two** failed fix rounds, stop and show the user the last report.

Done with this step when every ticket has a PASS report.

## 4. Final verify and review

1. Spawn `verifier` in spec scope. On FAIL, treat it like step 3.3.
2. Spawn `reviewer` with: spec issue, branch. Save to `<run>/review.md`.

## 5. Hand back

Report to the user in German: tickets with their PASS reports, the review's blockers and notes, the run directory path. Then offer:

- fix the blockers (a developer round per blocker, then step 4 again),
- push the branch and open a draft PR closing the spec and its tickets.

Push and PR only on the user's yes.
