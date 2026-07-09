# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-06-12

### Added
- Initial release of depwalk-action
- Composite GitHub Action wrapping @sulthonzh/depwalk
- Inputs: command, directory, severity, output, fail-on
- Outputs: result (raw depwalk output)
- Support for text, JSON, and markdown output formats
- GitHub Actions Step Summary integration for markdown output
- Severity-based workflow failure (high, medium, low, none)
