# Smokes and Alcohol Psy Relief

Goal: smokes restore psy health instead of radiation; alcohol keeps its radiation restore and also restores psy
health. Started 2026-11-14 (ROADMAP entry "Smokes and alcohol rebalance", split out of Psy Stress Remake and
Radiation FX Overhaul). Shipped readme: `src/readme.md`.

## Files
- `src/gamedata/scripts/psy_relief.script`: all logic (log tag `[PsyRelief]`).
- `src/gamedata/scripts/psy_relief_mcm.script`: MCM menu (id `psy_relief`, single page).
- `src/gamedata/configs/mod_system_zzz_psy_relief.ltx`: DLTX `!boost_radiation_restore` on the 9 smokes +
  `[psy_relief_items]` (item = kind, amount at Medium[, seconds]).
- `src/gamedata/configs/text/{eng,rus}/st_psy_relief.xml`: strings (rus = Windows-1251).

## Version
In `mod.json`, readme title + history, script header, ltx header, both text files (comment line).

## Decisions (Expand, 2026-11-14)
- Smokes: radiation restore removed, psy restore added; their other stats untouched.
- Alcohol: keep radiation restore as is, add psy restore.
- Being drunk boosts psy restore and gives psy protection (Expand, 2026-11-14), linear with the alcohol level.
  0.4.0 (Expand): own on/off switch; two sliders (restore 0-300%, protection 0-90%), each moved by its own
  preset (MCM "preset" widget like Psy Stress Remake, presets defined in psy_relief_mcm.script, script reads only
  the sliders). Restore Light..Strong +50/+75/+100/+150/+200%, protection 30/40/50/60/70%, Medium default;
  the values are part of the preset names.
- Smokes/alcohol from other mods are detected automatically (Expand asked for this).
- Gradual restore by script (not `boost_telepat_protection`: one booster per type, a smoke would cancel Psy-Block).
- Two MCM dropdowns (smokes, alcohol), 5 presets each: Light 0.6 / Mild 0.8 / Medium 1.0 / Potent 1.3 /
  Strong 1.6 (Expand asked for two in-between steps around Light-Medium-Strong). Text keys, not numbers.
- Medium amounts per use: Marlboro 5, Lucky 6, Russian 4, Soviet (cigar1..3) 8/9/10, Cuban cigar 12, Russian
  tobacco 9, hand-rolling 8; Nemiroff 8, Putinka 7, Stolichnaya 10, metal bottle 8, beer 5 (% psy health).
  Scaled roughly by price. Calm regen in GAMMA ~4%/in-game hour.

## How it works (keep)
- Auto-detection on the final config, lazily per section (cache): smoke = `use_sound` contains `inv_smoke` or
  `required_tool = box_matches`, skipped when `boost_telepat_protection > 0` (joint, marijuana); alcohol =
  `eat_alcohol > 0` and (`kind = i_drink` or a `use_sound` with `drink`) - akvatab / tetanus / yadylin have
  alcohol but no kind and are skipped. Amounts from `[psy_relief_auto]` (smoke 0.05, alcohol 0.07). List kind
  `none` excludes. Detected smokes keep their radiation restore (no Lua way to remove a booster).
- Drunkenness: `actor:cast_Actor():conditions():GetAlcohol()` (monolith, "demonized"; checked once on first update).
  Vodka sip +0.195, decay `alcohol_v = -0.00052` (GAMMA actor ltx), smokes have negative eat_alcohol (sober up).
- Drunk effects work on measured psy health: each tick d = psy - psy after our last change; d < 0 -> give back
  -d * protection * alcohol, d > 0 -> add d * bonus * alcohol; doses x (1 + bonus * alcohol). Covers every psy
  source without patching the damage balancer (which subtracts psy in its own psy table) or arszi_psy. A lethal
  hit inside one tick is not prevented. Psy-Block style boosters are untouched (no booster used).
- Psy health via `arszi_psy.change_psy_health` (base G.A.M.M.A. Psy rework and Psy Stress Remake both have it;
  same on GAMMA `main` and `dev2`). Psy rework keeps psy in its own table and overwrites the engine value, so the
  engine must not be written directly. Base `get_psy_health()` is nil before its data loads: checked each tick.
  Fallback without arszi_psy: engine `actor:change_psy_health` (monolith binds it to `conditions().ChangePsyHealth`).
- Restore time = the item's `boost_time` read at game start (real seconds), so other DLTX mods' times count
  (in profile `v31.1 - radiation rework` "drinks and radiation item rebalancing" sets smokes to ~200 s; base GAMMA ~50 s).
- `actor_on_update` registered only while a dose is active or alcohol > 0.005 (any item with eat_alcohol > 0 starts it), 0.5 s tick. Doses saved in `m_data.psy_relief`
  (`v`, `doses = {{a, t}}`), cleared on net_destroy and on disable.
- DLTX file named `zzz` so its key removal comes after "drinks and radiation item rebalancing"
  (`mod_system_drink_nerfs.ltx`), which sets `boost_radiation_restore` on the same items.
- Tooltip: wraps `ui_item.item_description` and `ui_item.get_sec_desc` like S.T.I.M. (chain-safe).
  Since 0.4.2 (Expand asked, 2026-11-17) the psy restore is a row in the consumable stats block
  (`utils_ui.stats_table.booster["psy_relief_restore"]`, index 3.5, icon `ui_am_propery_11`, caption
  `st_psy_relief_stat`, `value_functor = {"psy_relief","stat_value"}`, returns 0 = row hidden). GAMMA's
  `ish_item_stats.override_stats_table` replaces the whole booster table on game start and on `g_game_difficulty`,
  so the row is re-added in the desc wrappers (the description is built before the stats loop in
  `UIInfoItem:Update`). No booster table -> old description line. Restore time is shown only when it differs from
  the item's boost_time (appended to the unit, `stat_row.unit` set per call). utils_ui has 16 stat rows.
  Winning utils_ui: `HD Inventory Framework UI Patch GAMMA` (`v31.1 - radiation rework`), `G.A.M.M.A. Guns Have
  No Condition` (`G.A.M.M.A`); both render rows the same way.

## Environments
- Item stats: `G.A.M.M.A. UI` items_drink.ltx (base, profile `G.A.M.M.A`) / `G.A.M.M.A. UI - Expand` (wins in
  `v31.1 - radiation rework`; same smoke/alcohol values). Not in the GAMMA GitHub repo.
- "drinks and radiation item rebalancing" is enabled only in `v31.1 - radiation rework`.

## Open items
- 0.1.0 built, not tested in game. Check: log `[PsyRelief] 14 listed items restore psy health` (0.2.0 text) and
  `loaded: psy health via arszi_psy`; smokes show no radiation row; psy rises while smoking (debug log).
- Not checked yet: DLTX `!key` removal really drops the radiation booster in game; tooltip under UI Rework.
- GitHub repository: **public** (Expand, 2026-11-14); create it after the in-game test (skill `gamma-release`,
  "New mod repository"), then add the mod to the root README table and SETUP.md.
- 0.4.2 built (psy restore as a stats row), not tested: row on cigarettes and vodka in both languages, value
  column not wrapped, row still there after changing difficulty, row gone with the tooltip option off.
- 0.4.0 built (drunk switch, presets + sliders), not tested. 0.3.0 not tested. 0.2.0 not tested either: check log `drunk effects on`, auto-detected items in the debug log, drunk refund.
