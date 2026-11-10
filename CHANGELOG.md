# Changelog

All notable changes to this project are documented in this file.

## [0.4.2] - 2026-11-02
### Fixed
- Fixed inline comment stripping in `.env` loader when `#` appears inside single- or double-quoted values.
- Improved error message formatting when multiple fields fail validation simultaneously.

## [0.4.0] - 2026-10-14
### Added
- Added `${VAR:-fallback}` default syntax support during `.env` variable interpolation.
- Added `envshield dump` CLI subcommand to print redacted JSON configuration snapshots.

## [0.3.0] - 2026-09-19
### Added
- Added `Url` field validator with allowed scheme enforcement (`schemes=["https", "postgresql"]`).
- Added `min_val` and `max_val` bounds checking on `Int` and `Float` fields.

## [0.2.0] - 2026-08-25
### Added
- Added `SecretStr` wrapper to prevent accidental credential exposure in `repr()`, `str()`, and structured log serialization.
- Added `envshield example` CLI command to auto-generate `.env.example` files from `EnvSchema` subclasses.
