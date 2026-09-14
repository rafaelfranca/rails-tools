---
name: rails-changelog-review
description: Review and update Rails component changelogs for a branch, commit range, release tag, backport, or GitHub compare URL. Use when asked to find missing Rails CHANGELOG.md entries, decide if a Rails change needs an entry, or add or verify entries.
---

# Rails Changelog Review

Review a Rails diff for user-visible changes. Match each relevant change to the correct component changelog.

## Set the range

1. Accept a base ref and a head ref, or parse them from a GitHub compare URL.
2. Prefer local Git refs. The GitHub compare page can omit details.
3. If the base is an ancestor of the head, use `base..head`.
4. Otherwise, use `git merge-base base head` as the range base.
5. Use the merge-base range for a GitHub `base...head` comparison.

## Inspect the diff

1. List commits with `git log --no-merges --reverse`.
2. List paths with `git diff --name-status`.
3. Map each production code change to its commit with `git log --name-only`.
4. Do not use a commit subject as evidence of behavior.
5. Read each source patch and its tests.
6. Read a linked pull request when the patch does not show the user effect.
7. Read the top active section of each affected component `CHANGELOG.md`.

## Decide if an entry is required

Add an entry for these changes:

- A public API or behavior change.
- A bug fix that changes a response, result, error, data, or generated application output.
- A material performance improvement in a public Rails API or helper.
- A configuration default that changes an application result.

Do not add an entry for these changes:

- Tests, CI, release tools, or repository maintenance.
- Dependency pins or lockfile updates without a Rails behavior change.
- Documentation changes.
- Refactors with no observable effect.
- A minor compatibility fix with no meaningful user impact.

Treat a follow-up fix as covered only when an existing entry explicitly describes the corrected behavior.

## Add an entry

1. Add the entry to the affected component `CHANGELOG.md`.
2. Put the entry in the active top section before the latest release heading.
3. Describe the user result, not the implementation detail.
4. Credit the original contributor, not the merger or backport author.
5. Confirm the contributor name from the pull request or author profile when needed.
6. Follow this format:

```markdown
*   Fix `Example` to return the correct result.

    *Contributor Name*
```

## Verify and report

1. Re-read each changed changelog section.
2. Do not run tests for changelog-only changes.
3. Report each reviewed change as `covered`, `added`, or `not required`.
4. State the component file for each added entry.
