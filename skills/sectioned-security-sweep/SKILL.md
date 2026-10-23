---
name: sectioned-security-sweep
description: >-
  Section a codebase into focused security passes, search each section with a
  frontier model, and adversarially cross-check every finding with a second
  frontier model from a different vendor. Use when asked for a security sweep,
  vulnerability hunt, adversarial code review, dual-model security pass, or
  sectioned security review of a project. Default is report-only; auto-fix is
  an explicit switch.
---

# sectioned-security-sweep

Hunt real security holes. Stay quiet when a section is clean. Never ship a
finding that only one model believes.

## Defaults

- `auto_fix`: off. Find, cite, propose. Do not edit the project unless the
  invoker sets `auto_fix: on` for this run.
- Scope: one named project root. Do not wander into sibling repos.
- Output: a short pass log per section, plus a human-facing report that lists
  only dual-confirmed findings.

## Model requirements (choose at run time)

Do not hard-code model product names. They rot in weeks. Pick by property:

| Role | Required properties |
|------|---------------------|
| Seeker | Frontier / top-tier for that vendor. High reasoning depth. Same vendor may lead different passes over time. |
| Counter-reader | Frontier / top-tier. High reasoning depth. **Different vendor** from the seeker on this pass. |

If only one vendor is available, stop and say so. A same-vendor "second opinion"
does not satisfy this skill.

Alternate which vendor leads the seeker role across passes when quota allows.

## Procedure

### 1. Section the tree

Read [reference/sectioning.md](reference/sectioning.md). Produce five to ten
sections with explicit path globs or directory lists. Prefer security-shaped
cuts (auth, sessions, payments, public API, admin, data store, crypto, supply
chain, deploy/config) over arbitrary file chunks.

Completion: a numbered section list exists and every relevant source path sits
in exactly one section (or is consciously deferred as out of scope).

### 2. One section per pass

For each section, in order or by the invoker's priority:

1. Restrict context to that section's paths.
2. Give the seeker a short brief: the section name, the paths, and "find
   exploitable or seriously risky issues; cite file and line; invent nothing."
3. If the seeker returns no findings, archive the pass with a one-line
   `clean` note. Do not escalate silence into a report entry.
4. If the seeker returns findings, run the counter-reader on **each** finding
   using [reference/adversarial-review.md](reference/adversarial-review.md).
5. Keep only findings the counter-reader marks `hold`. Drop `overread` and
   `insufficient_evidence`. On `disagree` with no shared core, drop or demote
   to `note` outside the confirmed list; never present disagreement as a
   confirmed hole.
6. Write the pass result using [reference/report-format.md](reference/report-format.md).

Completion for a section: either `clean`, or every seeker finding has a
counter-reader verdict and only `hold` items remain in the confirmed list.

### 3. Human handoff

Show the invoker only dual-confirmed findings, each with seeker cite,
counter-reader stance summary, and a proposed fix (not applied unless
`auto_fix: on`).

If `auto_fix: on`, apply the smallest safe fix for confirmed items only, then
re-run the counter-reader on the diff before claiming done.

## Hard stops

- Do not report seeker-only findings as confirmed.
- Do not use a weak or local small model for seeker or counter-reader.
- Do not expand a pass beyond its section to "just check one more file" unless
  that file is required evidence for a finding already under review.
- Do not claim a clean bill of health for the whole project after one section.
