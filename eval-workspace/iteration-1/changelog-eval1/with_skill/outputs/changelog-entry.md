## [2.4.0] - 2026-03-19

### Added

- **OAuth2 Support**: Add OAuth2 authentication flow for secure third-party integrations
- **CSV Export**: Add CSV export capability for reports, enabling offline analysis and data sharing
- **Dark Mode**: Add dark mode toggle for improved readability in low-light environments

### Fixed

- **WebSocket Handler**: Fix memory leak in WebSocket handler that caused increasing memory consumption under sustained connections
- **Scheduled Jobs**: Fix incorrect timezone calculation in scheduled jobs that caused tasks to execute at wrong times
