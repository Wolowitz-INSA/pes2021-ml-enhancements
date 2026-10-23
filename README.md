# PES 2021 Master League Enhancements

An unofficial, experimental collection of Sider modules and optional offline tools for Master League. It adds teammate decision adjustments, score-aware mentality control, configurable instant results, opponent formation/lineup changes, and annual recruitment candidates. Compatible existing careers can be used without starting over.

**[English user guide](Community-Guide-English.md) · [中文使用说明](社区使用说明-中文.md) · [Release notes](RELEASE_NOTES.md)**

## Download and install

Download **[PES2021-ML-Enhancements-Community-r4.zip](https://github.com/Wolowitz-INSA/pes2021-ml-enhancements/releases/download/community-r4/PES2021-ML-Enhancements-Community-r4.zip)** from **[Community r4](https://github.com/Wolowitz-INSA/pes2021-ml-enhancements/releases/tag/community-r4)**. This named ZIP contains the installable modules, offline-tool source and licenses. GitHub's automatic source-code archives contain this documentation repository only.

1. Fully exit the game and Sider, then extract the ZIP into a writable folder.
2. Open PowerShell in the extracted folder. Replace the example Sider path with your active installation:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\Install-Mods.ps1 -SiderDir 'D:\Games\PES2021\SiderAddon' -TeammateMode Fixed
```

3. Keep the backup path printed by the installer. Restart Sider and the game.
4. Check the current `sider.log` for successful initialization and errors. Open each module's overlay; in a match, inspect its status and call counters. Loading successfully is not proof that a gameplay path has been used.

Choose **one** teammate edition:

- **A — `Fixed`:** fixed-player control; protects the player you control.
- **B — `Team`:** full-team play with player switching. Use `-TeammateMode Team` instead; protects the currently controlled player after switches.

The installer enables the selected edition plus four other modules, removes this package's conflicting AI loading entries, and backs up existing configuration/files. It does not edit saves. **Runtime modules require no Python.** See the guides for controls, parameter editing and backup-based removal. Fully restart after updates or removal.

## Five modules enabled by default

| Module | Default behavior |
|---|---|
| Teammate AI A **or** B | Local decision tier **6**, configurable to **5**, for selected ball, passing and dribbling branches |
| Mentality | Attack/defence decisions based on score, time and aggregate; neutral baseline team strengths until you supply your own snapshot |
| Instant results | Reduced spirit/captain advantages, an editable probability curve, and both teams' actual starting-XI registered-position OVR averages |
| Opponent phase shape | Uses each preset's kickoff shape across possession phases, while preserving distinct presets |
| Opponent lineup | Requests native **By Ability** at recognized automatic selection calls |

Teammate changes affect selected native decision inputs, **not the entire AI at Legend difficulty**. Some branches treat tiers 5 and 6 identically. Player attributes are not directly boosted, and improvement in every action is not guaranteed. Identity and code checks remain active; the guides explain each module's scope and off switch.

## Optional annual recruitment and tactics

Offline tools require **Python 3.10+**, with no third-party Python packages, and database paths configured in the chosen profile. Sider users do not need this setup.

- **Recommended: `public_profile.json`.** Dynamically selects the strongest **16 non-player clubs** using current positional ratings and theoretical best XIs. The count is adjustable. Other eligible clubs supply players; selection is not restricted to a region or famous-club list.
- **Optional fixed elite profiles.** Predefined targets/donors require adapting and verifying their internal save references and team names against your roster. Changing only a checked name is not a valid adaptation.

Recruitment protects the current player's club and verified outgoing loans, excludes loaned players from permanent moves, and writes a separate candidate for review. It does not create free-agent contracts. Tactical adaptation chooses among existing native presets. Preserve the original save and review candidates before trying them; neither tool automatically overwrites it.

## r4 fixes and validation limits

- Fixed corrupted teammate overlay text and crashes when switching pages by retaining registered callbacks.
- Fixed startup failures caused by Lua functions unavailable in Sider's restricted environment.

**Live validation currently covers main-menu module initialization, overlay display and repeated page switching.** Full-match effects, controller-switch timing, candidate-save progression and multi-season balance still need playtesting. Offline tests and native-code emulation do not replace that validation.

Runtime modules require matching executable signatures. Offline tools require the supported **575-club layout plus built-in team-boundary and name checks**; other languages or reordered rosters may be rejected. Public selection does not make every overhaul compatible. Do not bypass failed checks.

## Notices and feedback

No game executables, Sider, roster databases or personal saves are included. Retain [third-party notices](THIRD_PARTY_NOTICES.md) and the bundled license files when redistributing. This README does not grant additional license rights.

For feedback, include the package release, control mode, reproduction steps and relevant current-launch Sider log messages. State whether initialization passed and call counters increased.
