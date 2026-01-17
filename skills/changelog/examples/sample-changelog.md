# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- **Analytics**: Add real-time dashboard with live metrics
- **API**: Add bulk import endpoint for user data

### Changed
- **UI**: Update navigation layout for better mobile experience

### Fixed
- **Auth**: Fix session timeout not refreshing on activity

## [2.0.0] - 2024-03-15

### BREAKING CHANGES

- **API**: Authentication method changed from API keys to JWT tokens
  - API keys deprecated and will be removed in v2.1.0
  - Migration guide: [docs/migration/v2.md](link)

  ```bash
  # Before
  curl -H "X-API-Key: your-key" https://api.example.com/users

  # After
  curl -H "Authorization: Bearer your-jwt" https://api.example.com/users
  ```

- **Database**: Renamed `users` table to `accounts`
  - Run migration: `npm run db:migrate`
  - Update queries referencing old table name

### Added
- **Authentication**: JWT-based authentication with refresh tokens
  - Access tokens expire in 15 minutes
  - Refresh tokens expire in 7 days
  - Automatic token rotation on refresh
- **Security**: Add rate limiting to all API endpoints
  - 100 requests/minute for authenticated users
  - 20 requests/minute for unauthenticated requests
- **Monitoring**: Add Prometheus metrics endpoint at `/metrics`

### Changed
- **Performance**: Optimize database queries for user listing
  - Reduced query time by 60%
  - Added pagination support
- **Dependencies**: Update Node.js requirement to v20 LTS

### Deprecated
- **API**: The `/v1/` API prefix is deprecated
  - Use `/v2/` for all new integrations
  - `/v1/` will be removed in v3.0.0

### Removed
- **CLI**: Remove `--legacy` flag (deprecated since v1.5.0)
- **Config**: Remove support for `.env.local` (use `.env` instead)

### Fixed
- **Authentication**: Fix race condition in token refresh logic
- **UI**: Fix chart rendering on Safari mobile
- **API**: Fix pagination returning duplicate items

### Security
- **Auth**: Fix session fixation vulnerability
- **Dependencies**: Update `lodash` to fix prototype pollution

## [1.2.0] - 2024-02-01

### Added
- **Dashboard**: Add export functionality for reports (PDF, CSV)
- **API**: Add filtering options to list endpoints
  - Filter by date range
  - Filter by status
  - Filter by category
- **Notifications**: Add email notifications for critical events

### Changed
- **UI**: Improve loading states with skeleton components
- **Performance**: Implement Redis caching for frequently accessed data

### Fixed
- **Database**: Fix connection pool exhaustion under high load
- **Export**: Fix CSV export failing for large datasets
- **Dates**: Fix timezone handling in date filters

## [1.1.0] - 2024-01-15

### Added
- **User Management**: Add user roles and permissions
  - Admin, Editor, and Viewer roles
  - Granular permission controls
- **Audit**: Add audit logging for all data changes
- **Search**: Add full-text search across all resources

### Changed
- **API**: Standardize error response format
  ```json
  {
    "error": {
      "code": "VALIDATION_ERROR",
      "message": "Human-readable message",
      "details": [...]
    }
  }
  ```

### Fixed
- **Auth**: Fix login redirect loop on expired sessions
- **Forms**: Fix form validation not showing all errors

## [1.0.0] - 2024-01-01

### Added
- **Core**: Initial release
- **Authentication**: Email/password authentication
- **Users**: User registration and profile management
- **API**: RESTful API with OpenAPI documentation
- **Dashboard**: Admin dashboard with basic analytics
- **Database**: PostgreSQL with Drizzle ORM
- **Caching**: Redis for session and data caching
- **Deployment**: Docker and Kubernetes support

[Unreleased]: https://github.com/example/project/compare/v2.0.0...HEAD
[2.0.0]: https://github.com/example/project/compare/v1.2.0...v2.0.0
[1.2.0]: https://github.com/example/project/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/example/project/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/example/project/releases/tag/v1.0.0
