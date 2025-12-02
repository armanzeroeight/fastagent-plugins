---
name: changelog-manager
description: Maintains changelogs following Keep a Changelog format, categorizes changes by type. Use when updating CHANGELOG, preparing releases, or documenting version changes.
---

# Changelog Manager

## Quick Start

Create or update a changelog:

```bash
# Review recent commits
git log --oneline --since="2024-01-01"

# Check current version
cat package.json | grep version
```

## Instructions

### Step 1: Review Changes

Gather changes since last release:

```bash
# Get commits since last tag
git log $(git describe --tags --abbrev=0)..HEAD --oneline

# Or since specific date
git log --since="2024-01-01" --pretty=format:"%s"
```

### Step 2: Categorize Changes

Group by type following Keep a Changelog:

| Category | Description | Examples |
|----------|-------------|----------|
| Added | New features | New API endpoint, new command |
| Changed | Changes to existing | Updated algorithm, changed default |
| Deprecated | Soon-to-be removed | Old API marked deprecated |
| Removed | Removed features | Deleted unused function |
| Fixed | Bug fixes | Fixed crash, corrected calculation |
| Security | Security fixes | Patched vulnerability |

### Step 3: Format Entry

Follow Keep a Changelog format:

```markdown
## [Version] - YYYY-MM-DD

### Added
- New feature description
- Another new feature

### Changed
- Modified behavior description

### Fixed
- Bug fix description
```

### Step 4: Update CHANGELOG.md

Add new entry at the top (after "Unreleased"):

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [2.1.0] - 2024-01-15

### Added
- User authentication with JWT tokens
- Rate limiting for API endpoints

### Changed
- Updated database schema for better performance
- Improved error messages

### Fixed
- Fixed memory leak in background worker
- Corrected timezone handling in reports

## [2.0.0] - 2023-12-01

### Added
- Complete API rewrite
- GraphQL support

### Changed
- **BREAKING**: New authentication system

### Removed
- Legacy v1 API endpoints
```

## Keep a Changelog Format

### Version Header

```markdown
## [Version] - YYYY-MM-DD
```

- Use semantic versioning (MAJOR.MINOR.PATCH)
- Include release date
- Link to release tag

### Change Categories

**Order of sections:**
1. Added
2. Changed
3. Deprecated
4. Removed
5. Fixed
6. Security

**Writing guidelines:**
- Start with verb: "Added", "Fixed", "Updated"
- Be specific but concise
- Include issue/PR numbers if applicable
- Highlight breaking changes with **BREAKING**

### Examples

**Good entries:**
```markdown
### Added
- User profile page with avatar upload (#123)
- Export data to CSV functionality

### Fixed
- Fixed crash when uploading files over 10MB (#456)
- Corrected calculation in tax report
```

**Bad entries:**
```markdown
### Added
- Stuff
- Various improvements

### Fixed
- Bug fixes
```

## Semantic Versioning

Determine version bump:

| Change Type | Version Bump | Example |
|-------------|--------------|---------|
| Breaking change | MAJOR | 1.0.0 → 2.0.0 |
| New feature | MINOR | 1.0.0 → 1.1.0 |
| Bug fix | PATCH | 1.0.0 → 1.0.1 |

**Breaking changes:**
- Removed functionality
- Changed API signatures
- Changed behavior that breaks existing code

## Automation

### Generate from Commits

If using conventional commits:

```bash
# Install
npm install -g conventional-changelog-cli

# Generate
conventional-changelog -p angular -i CHANGELOG.md -s
```

### Pre-release Checklist

- [ ] All changes categorized
- [ ] Version number updated
- [ ] Release date added
- [ ] Breaking changes highlighted
- [ ] Links to issues/PRs included
- [ ] Unreleased section cleared
