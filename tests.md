# In-game tests - Smokes and Alcohol Psy Relief

Suite `sapr`: runner `tools\autotest\mod\gamedata\scripts\zzz_claude_autotest_sapr.script` (flag `-claudesapr=`,
log `F:\Anomaly\appdata\logs\claude_autotest_sapr.log`). Written 2026-11-26.

## Results
- 2026-11-26 `basics` (`reports\autotest-20261126-165054.txt`): 21 PASS / 4 FAIL, all runner bugs (upvalue search
  root, `and false or` in Lua, radiation check too strict for a DLTX-deleted key that inherits 0.0), fixed.
- 2026-11-26 full run, 0.4.2 (= 1.0.0 code), profile `Claude Test`, save `claude_test`
  (`reports\autotest-20261126-205847.txt`, one launch, ~5 min of scenarios): **12 PASS, 1 FAIL**, 0 skipped.
  FAIL `auto` / `auto_skips`: Yadylin (`kind = i_medical`, drink sound, `eat_alcohol` 0.45) is detected as alcohol =
  **mod bug** (see `CLAUDE.md`). Smokes' `boost_radiation_restore` logged as 0.0 on all 9 (inherited from `[booster]`);
  cigarette applies no radiation booster, vodka does. Auto-detected in this profile: `vodka_script`, `yadylin`.
  After a fix: rerun `basics,auto`.

## How it tests
- The installed zip, unchanged (the mod has no test hooks). Its locals (settings, `doses`, `listed`, `auto_cache`,
  `ticking`, `psy_mod`, `stat_row`, `add_dose`, `clear_doses`...) are read and set through the upvalues of its public
  functions (`on_option_change`, `stat_value`, `on_game_start`), its `ui_item` wrappers and its registered callbacks
  (found in `axr_main`'s `intercepts` table, only functions whose source is `psy_relief.script`). `basics` checks every
  internal the suite needs is readable: a rename shows up there as one FAIL.
- Code of 0.4.2 = 1.0.0 (version label only), so the version check accepts both. Profile `Claude Test` has 0.4.2
  installed (2026-11-26).
- Real uses: item spawned, `db.actor:eat(item)` (engine -> `actor_on_item_use` -> the mod); `box_matches` spawned too.
- Psy health the mod gives = sum of its own `arszi_psy.change_psy_health` calls (runner wrapper, caller source
  `psy_relief.script`). While measuring, Psy Stress Remake's `manage_psy_health` (regen / stress) is a no-op in memory,
  so the measured psy rise must equal what the mod gave.
- Long restore times (smokes ~50-200 s) are cut by setting the active doses' `ms` (data only, same code path);
  the gradual rate is checked before that on the real time.
- Drunkenness set with `conditions():ChangeAlcohol` (monolith), psy losses / gains made by the runner through
  `arszi_psy.change_psy_health`. Expected values from the runner's own copy of the rules (item list, presets,
  `loss x min(0.9, protection x alcohol)`, `gain x restore x alcohol`, dose `x (1 + restore x alcohol)`).
- MCM values in memory; debug log on (mod default off).

## Scenarios (all in one launch, save `claude_test`)
| Scenario | What it proves |
|---|---|
| `basics` | loaded, version 1.0.0/0.4.2 (script header + both text files), internals and 6 callbacks found, defaults / presets / auto amounts = runner's copy, item list = ltx (kind, amount, time = boost_time), DLTX: 9 smokes with `boost_radiation_restore` missing or <= 0 (a deleted key inherits `[booster]`'s 0.0), vodka keeps it, psy via `arszi_psy`, alcohol API on, tooltip wrappers, MCM menu (18 entries, defaults, drunk presets 50..200 / 30..70, preset widget), all eng strings, rus ids + cp1251, no tick while idle |
| `smoke_dose` | real cigarette: debug line (5.0%, boost_time, 1 dose), no radiation booster, gradual rate = 5% / boost_time, all 5% delivered, psy rose by that, tick off afterwards |
| `alcohol` | real vodka, drunk effects off: 8.0%, radiation booster kept, alcohol level rises, exactly 8% delivered, tick off although drunk |
| `stack` | cigarettes + cigar + Stolichnaya = 3 doses, 27% total; 18 doses -> 16 (last 3 merged); clear_doses |
| `presets` | 5 steps x2 kinds via `stat_value` (independent), unknown values -> Medium, real Soviet cigar at Strong (12.8%), real Stolichnaya at Light (6%) |
| `drunk` | alcohol 0.5: tick starts from `on_option_change`; psy loss given back x0.5x0.5, gain raised x1.0x0.5; protection 90 at alcohol 1 (cap 0.9); slider 200 clamped to 0.9, 300 restore; dose x (1 + alcohol); sober -> tick off |
| `drunk_off` | switch off: no tick from alcohol, loss not given back, vodka gives exactly 8%; sliders 0/0 = off; negative sliders -> 0 |
| `disabled` | master switch off: no dose, nothing restored, stat value 0; on again: dose; off during a dose: cleared, restore stops |
| `auto` | every config section with use_sound / eat_alcohol / required_tool: mod's detection = runner's copy of the rule (kind, 5% / 7%, time); meds with alcohol (akvatab, tetanus, yadylin, medkit), weed, food never; real use of a detected unlisted item (line says auto-detected); option off: only listed items, cache reset |
| `tooltip` | stats row in `utils_ui.stats_table.booster` (index 3.5, icon, name, functor), no description line then; re-added after the table is replaced (sec and item tooltip); no table -> description line "+5%"; value per item, 0 for others; restore time in the unit only when it differs; option off |
| `state` | `load_state` with no data / no doses / junk / bad entries (dropped, amount capped at 1) / 20 doses (-> 16); `save_state` format `{v, doses = {{a, t}}}`; `actor_on_net_destroy` clears doses and tick (direct callback calls) |
| `saveload` | dose active at save (overwrites save `claude_autotest`), after the load: "1 doses carried over", amount left, tick on, restore continues |
| `no_mcm` | every MCM value missing: mod defaults (debug off: no debug line), a dose still delivers 5% |

## Not automated (for Expand)
- Tooltip looks: the row's icon, alignment, value column not wrapped, Russian text (the game runs in English).
- The row after a real difficulty change in the GAMMA menu (`ish_item_stats` replacing the table is simulated).
- Real level change (doses cleared by `actor_on_net_destroy`: called directly in `state`), actor death during a dose
  (the tick's dead-actor branch), sleeping, a real psy attack while drunk (a runner-made psy loss is used).
- Profile `G.A.M.M.A` with G.A.M.M.A. Psy rework instead of Psy Stress Remake: same suite, own save, `-Isolated`.
- MCM preset widget moving the sliders (MCM UI).
