## [2.4.0] - 2026-03-19

### Added

- **Authentication**: Add OAuth2 support for secure third-party authentication flows
- **Reporting**: Add CSV export for reports, enabling offline analysis and data portability
- **UI**: Add dark mode toggle for improved accessibility and reduced eye strain in low-light environments

### Fixed

- **WebSocket**: Fix memory leak in WebSocket handler that caused increasing memory consumption on long-lived connections
- **Scheduler**: Fix incorrect timezone handling in scheduled jobs that caused jobs to fire at wrong times in non-UTC zones

---

### Comparison link update

The following comparison links should be added/updated at the bottom of `CHANGELOG.md`:

```markdown
[Unreleased]: https://github.com/org/repo/compare/v2.4.0...HEAD
[2.4.0]: https://github.com/org/repo/compare/v2.3.0...v2.4.0
```

The previous `[Unreleased]` link (which pointed to `v2.3.0...HEAD`) must be updated to point to `v2.4.0...HEAD`, and the new `[2.4.0]` comparison link must be inserted between `[Unreleased]` and the `[2.3.0]` entry.

---

### Skill application notes

- **Keep a Changelog format**: Entry uses the standard `## [version] - date` header with `### Added` and `### Fixed` grouping sections.
- **Clear descriptions**: Each entry describes what changed and why it matters to users (e.g., "enabling offline analysis and data portability"), following the skill's guidance to write for users, not developers.
- **Grouped by change type**: All new features are under `Added`, all bug fixes under `Fixed` -- no interleaving of types.
- **Context via sub-details**: Fixes include root cause context (memory leak on long-lived connections, non-UTC timezone mishandling) so users can assess upgrade urgency.
- **Comparison links**: Reminded to update both the `[Unreleased]` link and add the new `[2.4.0]` comparison link, which the skill calls out as "the most commonly forgotten element."
- **Bold category prefixes**: Used `**Category**:` prefixes (Authentication, Reporting, UI, WebSocket, Scheduler) for scannability, consistent with the skill's example entries.
