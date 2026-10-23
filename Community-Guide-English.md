# PES 2021 Master League Enhancements: User Guide (r4)

**For normal play, complete installation and verification below.** The Sider modules need neither Python nor database-path setup. Existing Master League saves can be used; a new career is not required.

You need a working Sider installation. Runtime modules check executable code and refuse unsupported signatures. Offline tools require the supported save format and **575-club table structure**, and must also pass built-in team-boundary and name checks. Other languages or reordered rosters may be rejected; matching the club count alone is insufficient. Renaming files, removing checks or changing signatures does not establish compatibility.

## 1. Install and verify

1. Fully exit the game and Sider. Extract the complete ZIP into a writable folder.
2. Locate the Sider directory actually used to launch your game; it must contain `sider.ini`.
3. Open PowerShell in the extracted package and choose one command. Replace the example paths with your own.

**Edition A: fixed-player control**

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\Install-Mods.ps1 -SiderDir 'D:\Games\PES2021\SiderAddon' -TeammateMode Fixed
```

**Edition B: full-team control with player switching**

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\Install-Mods.ps1 -SiderDir 'D:\Games\PES2021\SiderAddon' -TeammateMode Team
```

Choose only one AI edition. The installer loads it and four other modules, enables required Lua/match-stat settings, removes loading entries for this package's old AI modules and the alternate edition, and preserves unrelated configuration. Existing configuration and same-name files are backed up under `SiderAddon\ml-enhancements-backups\timestamp`. **Keep the backup path printed by the installer: removal uses it.** Installation does not edit saves.

Restart Sider and the game, then verify:

1. Open `sider.log` in the active Sider directory. Check the current launch for each module's successful initialization and for `error`, `inactive` or unsupported-code messages.
2. Open the Sider overlay and visit each module. Space normally opens it and **1** cycles modules; use your own `sider.ini` key settings if different.
3. Enter a match and inspect player identification, enabled status and call counters. A visible page confirms loading; increasing counters show that the relevant native path was actually called. Instant-result counters update after directly skipping a match.

If an error appears, retain the log and stop using the affected module. Fully restart the game and Sider after updates or switching AI editions.

## 2. Features, controls and teammate AI level

These features start enabled, subject to their identity, competition and code checks. Use each key while **that module's overlay is selected**.

| Module | Function | Temporary controls |
|---|---|---|
| `teammate_fixed_ai.lua` (A) or `teammate_team_ai.lua` (B) | Adjusts selected teammate ball, passing and dribbling decision branches; default local tier **6** | **5 / 6** selects a tier; **0** restores native behavior |
| `ml_mentality.lua` | Adjusts attack/defence levels using score, time and knockout aggregate | **F8** pauses/resumes; **F6** reloads configuration and disarms current control |
| `skip_results.lua` | Adjusts immediately generated Skip Match results | **F7** enables; **0** restores native behavior |
| `opponent_fluid_off.lua` | Keeps each opponent preset's own kickoff shape across possession phases | **F6** enables; **0** observes without changing later conversions |
| `opponent_lineup.lua` | Requests native By Ability at recognized opponent auto-selection calls | **2** enables; **0** observes without changing later selections |

A protects the fixed player; B protects whichever player is controlled after each switch. Both use the same local decision mechanism and do not directly raise attributes. They override selected decision inputs, not the entire AI at Legend difficulty. Some branches behave identically at tiers 5 and 6, so improvement in every action is not guaranteed. Multiplayer or unclear control identity keeps on-field behavior native.

To make tier 5 persistent, exit the game, open the selected AI file in the installed `modules` folder, change `local DEFAULT_AI_TIER = 6` at the top to `local DEFAULT_AI_TIER = 5`, and restart. Only 5 and 6 are supported.

Temporary controls generally affect the next relevant call; they do not undo an already completed lineup or formation conversion. For formation/lineup comparisons, select the mode before preparation and reload the same match. To disable a module persistently, exit the game, remove or comment out its `lua.module` line in `sider.ini`, then restart.

The shape fix preserves differences between the three presets. It does not freeze all movement or disable marking, advanced instructions or preset switches. By Ability keeps native availability/fitness processing; not every match's lineup is guaranteed to pass through the recognized calls.

## 3. Tune instant results

Edit the top of the installed `modules\skip_results.lua`, save and restart. Keep the existing `local` declarations and change their values; do not append duplicate variables.

```lua
local SPIRIT_WEIGHT = 0.25
local CAPTAIN_DIVISOR = 5
local SOFT_CURVE = true
local USE_SELECTED_XI = true
local SOFT_CURVE_OPTIONS = {
    base_win = 35, base_loss = 35, strength_scale = 10,
    win_gain = 55, loss_drop = 30, max_win = 90,
    min_win = 5, min_loss = 5, min_draw = 5,
}
```

| Setting | Use |
|---|---|
| `SPIRIT_WEIGHT` | 0..1; default retains 25% of the spirit effect, 1 retains its original size, 0 removes this term |
| `CAPTAIN_DIVISOR` | Integer 1..100; default 5 reduces native +10/+20 percentage-point role bonuses to +2/+4; 1 retains their original size |
| `SOFT_CURVE` | true uses the softer curve below; false restores only the native curve, leaving other settings independent |
| `USE_SELECTED_XI` | true uses this fixture's actual starters; false restores the squad's highest-11 base strength term |

**Actual starters** means the 11 players recorded in the current fixture's starting lineup. Both teams use the average of those players' current native **registered-position OVR**. High-rated unused substitutes no longer raise this base automatically. This is not the displayed OVR at each assigned position. Native formation and stamina adjustments remain. Missing, duplicate or invalid lineup identities retain the native base; the module does not invent replacement starters.

The nine `SOFT_CURVE_OPTIONS` parameters apply only when `SOFT_CURVE=true`:

| Parameter | Default | Meaning |
|---|---:|---|
| `base_win` | 35 | Initial stronger-side win percentage at equal strength, before captain effects |
| `base_loss` | 35 | Initial weaker-side win percentage under the same conditions |
| `strength_scale` | 10 | Strength gap at which each gain/drop reaches half its amplitude |
| `win_gain` | 55 | Maximum stronger-side win increase as the gap grows |
| `loss_drop` | 30 | Maximum weaker-side win decrease as the gap grows |
| `max_win` | 90 | Stronger-side win ceiling |
| `min_win` | 5 | Stronger-side win floor |
| `min_loss` | 5 | Weaker-side win floor |
| `min_draw` | 5 | Draw probability floor |

For strength gap d and scaled captain bonus b, before bounds: `W=base_win+floor(win_gain*d/(d+strength_scale))+b`, `L=base_loss-floor(loss_drop*d/(d+strength_scale))-b`. After bounds, the draw probability is `100-W-L`.

All nine values must be integers. `strength_scale` accepts 1..1000; the others accept 0..100, with `min_win <= max_win <= 100-min_loss-min_draw`. Invalid settings refuse activation. Defaults are gameplay adjustments, not fitted real-league statistics.

This module handles only the player's **Skip Match / immediate result** fixtures. It does not change unrelated CPU fixtures or saved historical results. **Press 0 to disable all adjustments from this module.** Setting only `SOFT_CURVE=false` does not disable the module.

## 4. Supply team-strength estimates to the mentality module

The default `content\ml-mentality\match.ini` uses `strength_source=neutral`: equal baseline strength, while decisions still respond to score, time and aggregate. You do not need to enter teams or aggregate scores before every match. Unknown competitions, unclear player identity or unsynchronized scores prevent writes. Loading/reloading mid-match may wait for a new kickoff.

To use strengths from your own career:

1. Run the annual tool below to produce a preview.
2. Copy its `strengths_before.csv` to the installed `content\ml-mentality\strengths.csv`.
3. Set `strength_source=snapshot` in `match.ini`, then restart or press F6 on its overlay to reload. Do this before the next match's kickoff.

Use `strengths_after.csv` only after actually adopting that exact recruitment candidate. Regenerate after substantial ability or roster changes; do not mix files from different saves. The bundled CSV contains only a header, so supply valid data before selecting snapshot mode.

## 5. Offline tools: configure database paths first

**Only this section and the save tools below require Python 3.10+. No third-party Python packages are needed. Sider modules do not need this setup.** If Python is not on PATH, pass `-Python` with the full path to your own Python executable.

Open the profile you intend to use, starting with `transfers\public_profile.json`. Replace these placeholders with databases from your **currently active roster**:

- `player_database`: replace `CONFIGURE_PATH_TO_Player.bin` with the actual `Player.bin` path.
- `team_databases`: replace `CONFIGURE_PATH_TO_Team.bin`. This is an array; list multiple actual `Team.bin` files if needed, in active priority order.

The example below shows only the fields to edit; preserve all other settings. Replace `YourRoster` with your actual directory too:

```json
{
  "player_database": "D:/Games/PES2021/SiderAddon/livecpk/YourRoster/common/etc/pesdb/Player.bin",
  "team_databases": [
    "D:/Games/PES2021/SiderAddon/livecpk/YourRoster/common/etc/pesdb/Team.bin"
  ]
}
```

JSON accepts forward slashes; backslashes must be escaped as `\\`. Alternatively, use relative paths such as `SiderAddon/livecpk/YourRoster/common/etc/pesdb/Player.bin` and pass `-GameDir 'D:\Games\PES2021'`. `-GameDir` does not discover databases or convert roster formats. Configure each profile you use. Resolve missing paths or roster incompatibility instead of bypassing validation.

## 6. Annual recruitment: recommended Public dynamic profile

After a transfer window, save and exit the game. Run against your latest save:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\Run-AnnualStrengthening.ps1 -Save 'D:\Saves\ML00000000' -GameDir 'D:\Games\PES2021' -Profile '.\transfers\public_profile.json'
```

Public selects the **strongest 16 current clubs excluding the player's club**. Edit `target_selection.count` to change the number. Ranking uses the best theoretical XI average across the current formation and six common templates, assuming everyone available, fully fit and in neutral condition. Ties use save team-reference order. Clubs from any region, including classic teams, can qualify; it is not necessarily a list of European giants. Public removes fixed target lists but still requires the supported format and 575-club structure.

By default, each target receives at most two players; other non-player, non-target clubs supply players. Signings require at least 87 OVR and must improve the XI. Donors retain at least 22 players. Ratings come from the current career. The player's club, its current players and verified outgoing loans are protected; all loaned players are excluded from permanent moves.

Output appears under `staging\annual\timestamp` in the package: original backup, recruitment list, reports and a new encrypted candidate. **The original save is not automatically overwritten.** Review the list, separately preserve your original, then place the candidate in the game's save folder under the corresponding slot filename to test loading, advancing the date and saving separately. Use once per season; do not repeatedly stack different plans. Roster corrections do not create transfer fees, budget entries, news or transfer history, and do not renew contracts. The annual workflow currently cannot create new free-agent contracts or sign free agents.

**Optional fixed elite-club profiles:** `elite_profile.json`, `elite_profile_stronger.json` and `elite_profile_worldwide.json`. They retain predefined target/donor lists; the latter profiles offer more aggressive settings or wider sources. Before use, configure paths and verify that **internal save references and team names** identify the intended clubs in your roster. These references are not Sider database team IDs. A mismatch stops execution; changing only the checked name is not a valid adaptation. Once adapted, substitute the desired file in `-Profile`. Do not stack it with Public in the same season.

## 7. Offline tactical adaptation

First configure database paths in `transfers\elite_profile.json`. This command selects a better-fitting option from each CPU club's three existing native presets using its current squad, while protecting the player's club:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\Run-TacticsFix.ps1 -Save 'D:\Saves\ML00000000' -GameDir 'D:\Games\PES2021' -Mode native-preset
```

Candidates default to `staging\tactics\timestamp-native-preset`. The tool does not invent formations or manage live tactical switches.

To combine recruitment and tactical adaptation, run recruitment first, then use its **candidate save path** as the tactics command's `-Save`. Use the final tactics output. Two candidates created independently from the original cannot be combined by overwriting one with the other.

## 8. Uninstall, saves and feedback

Fully exit the game and Sider. Use the backup path recorded during installation:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\Uninstall-Mods.ps1 -BackupDir 'D:\Games\PES2021\SiderAddon\ml-enhancements-backups\installation-timestamp'
```

Removal restores the original configuration and same-name files. If an installed file changed afterwards, including your parameter edits, removal stops before writing. Retain your changes and, if necessary, manually remove this package's loading lines from `sider.ini`, then fully restart. Do not delete the whole `sider.ini`.

Runtime modules affect future matches and work with existing careers. Uninstalling does not undo saved results or an adopted recruitment/tactics candidate; restore a prior save backup to reverse those changes. Changing your managed club does not require editing team IDs.

For feedback, include this package's release, control mode, reproduction steps and relevant `sider.log` messages from the current launch. State whether checks passed, the player side was identified and call counters increased. Retain the bundled licenses and attribution when sharing.
