**Smokes and Alcohol Psy Relief by Expand 1.0.0**

For S.T.A.L.K.E.R. Anomaly G.A.M.M.A. Author: Expand

A smoke break or a drink calms the nerves; it doesn't flush radiation. Smokes now restore psy health instead of radiation. Alcohol keeps its radiation restore and also restores psy health, and being drunk makes you tougher against psy.

**How it works**
- Smokes (all cigarettes, the Cuban cigar, Russian and hand-rolling tobacco) no longer restore radiation. Their other stats (health, thirst, hunger, sleepiness, alcohol) stay as they are.
- Smokes and alcohol (all vodkas and beer) restore psy health gradually while the item's effect lasts (its boost time). Several items stack.
- Psy health per use at the Medium preset:
  - Marlboro 5%, Lucky Strike 6%, Russian brand 4%, Soviet cigarettes 8 / 9 / 10%, Cuban cigar 12%, Russian tobacco 9%, hand-rolling tobacco 8%.
  - Nemiroff 8%, Putinka 7%, Stolichnaya 10%, vodka in a metal bottle 8%, beer 5%.
- For comparison: calm psy regeneration in G.A.M.M.A. is about 4% per in-game hour.
- Smokes and alcohol from other mods are detected automatically and restore 5% (smokes) / 7% (alcohol) per use at Medium. Weed (joint, marijuana) and meds with alcohol are not counted. Their radiation restore, if they have one, is not removed.
- Being drunk (one sip of vodka = about 20% drunk; it wears off over time):
  - all psy health restore (items, regeneration, campfires, sleep) is raised;
  - part of every psy health loss (psy attacks, psy fields, stress) is given back. It does not save you from a single lethal psy hit.
  - At full drunkenness (less when only tipsy). Extra restore presets: Light +50%, Mild +75%, Medium +100%, Potent +150%, Strong +200%. Loss given back presets: Light 30%, Mild 40%, Medium 50%, Potent 60%, Strong 70%. Each preset moves its slider, which can then be fine-tuned (0 = that effect off).
  - Own on/off switch for the drunk effects.
- Tooltip: a "Psy health restoration" row in the item's stats block (next to effect time, satiety...) shows how much psy health one use restores.
- Restores still in progress are kept in the save.
- No per-frame work: a twice-per-second check runs only while an item is restoring psy or you are drunk.

**MCM (Smokes and Alcohol Psy Relief tab)**
Enable, smokes strength (a Marlboro cigarette restores Light 3% / Mild 4% / Medium 5% / Potent 6.5% / Strong 8%), alcohol strength (a sip of Stolichnaya: 6 / 8 / 10 / 13 / 16%; other items in the same proportion), detect items from other mods, drunk psy effects on/off, drunk extra psy restore (preset + slider), drunk psy protection (preset + slider), tooltip line, debug log.

**Requirements and compatibility**
- Made for G.A.M.M.A. Psy health goes through arszi_psy.script (G.A.M.M.A. Psy rework or Psy Stress Remake); without it the engine's psy health is used.
- The drunk effects need the drunkenness value of the modded exes (G.A.M.M.A.'s engine, also Quark). Without it they are off (the log says so).
- Any position in the load order. The radiation change uses DLTX (mod_system_zzz_psy_relief.ltx), no game file is replaced. It is applied after other DLTX files that change smokes, e.g. "drinks and radiation item rebalancing" (its longer smoke times are kept and used for the psy restore).
- Other mods can add, change or exclude items: see the comments in mod_system_zzz_psy_relief.ltx.
- Safe to add to and remove from an existing save. After removal smokes restore radiation again.

**Version history**
- 1.0.0: first public release (same as 0.4.2).
- 0.4.2: the psy health restore moved from the description into the item's stats block as its own row with the psy icon.
- 0.4.1: Russian MCM title renamed; MCM hints of the strength dropdowns show concrete psy health amounts per step, the drunk preset hints explain what the numbers mean.
- 0.4.0: on/off switch for the drunk effects; each drunk effect has a slider moved by its preset (values in the preset names); new extra restore steps +50 / 75 / 100 / 150 / 200%, protection 30 / 40 / 50 / 60 / 70%.
- 0.3.0: the two drunk sliders are replaced by one dropdown with five presets (Medium = the old defaults).
- 0.2.0: smokes and alcohol from other mods are detected automatically; being drunk boosts psy restore and gives psy protection (MCM sliders).
- 0.1.0: first test build.
