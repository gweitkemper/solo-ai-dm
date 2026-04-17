# Changelog

All notable changes to the Solo AI DM system are documented here.

## [Unreleased]

## [0.2.0] — 2026-04-12

### Changed
- Migrate from single dm-rules.md to 5-file architecture (dm-core-rules.md, dm-session-zero.md, dm-campaign-ops.md, claude-instructions.md, gpt-instructions.md)
- Apply character sheet completeness audit (70-field audit, 3-pass fix cycle)
- Apply cross-file dependency fixes (11 findings)
- Update README for new architecture

### Added
- Add claude-instructions.md for Claude Projects platform support
- Add CLAUDE.md for Claude Code integration
- Add ops/ directory with continuous improvement workflow

## [0.1.0] — 2026-03-28

### Added
- Initial public release
- Single dm-rules.md file with full Session Zero, combat, social, and exploration rules
- gpt-instructions.md for ChatGPT Custom GPT setup
- README with setup guide
- Worked session examples (now merged into dm-core-rules.md)
