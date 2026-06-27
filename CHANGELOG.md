# Changelog

All notable changes to the Solo AI DM system are documented here.

## [Unreleased]

## [0.3.0] — 2026-05-08

### Fixed
- Apply 35-finding audit (2026-05-01): internal contradictions, coverage gaps, 5e rules accuracy, LLM attention risks, cross-file dependency gaps, README accuracy, worked-example coverage, and tone/audience.
- Correct 2024 Exhaustion: spell save DCs are not d20 Tests and are not affected (#11).
- Flag 2024 critical hit change: only weapon/cantrip dice double; Sneak Attack and Divine Smite do not (#12).
- Flag 2024 Grapple/Shove change: target makes a STR/DEX save vs. DC 8 + STR + PB (#13).
- Flag 2024 ritual casting: any class with Ritual Casting can ritual cast unprepared spells (#14).
- Flag 2024 Ranger features: Deft Explorer and Favored Foe replace Favored Enemy/Natural Explorer (#15).
- Flag 2024 prepared casters: all classes prepare spells daily; "spells known" distinction removed (#23).
- Add Medicine check (DC 10) stabilization rule for downed PCs (#4).
- Remove slash prefixes from `stats` and `inventory` references for consistency (#1).

### Added
- Add Advantage/Disadvantage core rule (no stacking; both cancel) to DM Cheat Sheet (#6).
- Add difficult terrain rules to combat section (#5).
- Add flanking rule status (off by default; opt-in via meta) (#10).
- Add legendary actions, legendary resistance, and lair actions for boss encounters (#9).
- Clarify multiple concentration saves per turn (each damage instance is a separate save) (#7).
- Add Flow C failure-tracking guidance to prevent leakage in subsequent narration (#16).
- Add Example 14: beginner-mode combat (formula-showing, concept introduction) (#31, #35).
- Add Example 15: environmental hazard (trap detection, disarm, failure → combat) (#33).
- Add flow classification note to Example 3 Insight check (#30).
- Expand stat block format reminder in dm-campaign-ops.md to be self-contained (#2, #20).
- Add ASI definition above the level-up quick reference table (#34).
- Add minimal combat format reminder to travel section (#21).
- Add post-rest in-world time verification step (#18).
- Add long-rest worked example showing partial Hit Dice recovery and world advancement (#17, #32).
- Add cross-reference for full companion replacement protocol from session-zero to campaign-ops (#3).
- Add format skeleton at Session Zero → Chapter One transition (#22).
- Add 12th Critical Rule for concentration save prompting (instruction files; Feature #4) (#19).
- Add Downtime Activities section: training, crafting, research, faction work (Feature #3).
- Add auto-recap protocol at chapter breaks for soft save points (Feature #5).
- Add version comment header to instruction files for player visibility (#25).

### Changed
- Update README: Claude subscription text now lists Pro/Team/Enterprise (#24).
- Update README worked-example count to 18; reinforced-rules count to 12.

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
