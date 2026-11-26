# Changelog - Smokes and Alcohol Psy Relief by Expand

- 1.0.1: Yadylin no longer restores psy health (a medicine with alcohol and a drink sound, which the auto-detection
  took for alcohol): listed as `none` in `[psy_relief_items]`. Smokes get `boost_radiation_restore = 0` instead of
  the DLTX key removal `!boost_radiation_restore`: same in game (no radiation restore; a deleted key falls back to
  the parent's 0.0), but it also wins if a mod changes the parent section.
- 1.0.0: first public release. No code changes since 0.4.2; the version label only.
- 0.4.2: the psy restore is now a row in the item info's consumable stats block ("Psy health restoration" /
  «Восст. пси-здоровья», psy icon, after health/radiation restore) instead of a description line. The restore
  time is shown only when it differs from the item's effect time. UIs without a stats table keep the description line.
- 0.4.1: Russian MCM title «Курево и выпивка помогают психике» (was «Курево и алкоголь от пси»). Hints of
  the smokes/alcohol strength dropdowns list concrete amounts per step (Marlboro 3 / 4 / 5 / 6.5 / 8%,
  Stolichnaya sip 6 / 8 / 10 / 13 / 16%) instead of "% of Medium"; drunk preset hints explain the numbers
  with an example (one sip of vodka = about 20% drunk). Text only.
- 0.4.0: drunk effects get an on/off switch. Extra psy restore and psy protection each have a preset that
  moves its slider (MCM 1.7.1+ preset widget; plain list on older MCM), values shown in the preset names:
  restore +50 / +75 / +100 / +150 / +200%, protection 30 / 40 / 50 / 60 / 70% (Light..Strong, Medium default).
  Sliders: restore 0-300%, protection 0-90%.
- 0.3.0: drunk psy restore and protection: one MCM dropdown with five presets instead of two sliders.
  Light +60% / 30%, Mild +80% / 40%, Medium +100% / 50% (the 0.2.0 defaults), Potent +130% / 65%,
  Strong +160% / 80% (restore bonus / loss given back, at full drunkenness). No Off step.
- 0.2.0: smokes and alcohol from other mods are detected from their config (smoke sound or matches needed;
  alcohol = drink with eat_alcohol > 0; weed with its own psy booster and meds are skipped) and restore 5% / 7% at
  Medium (`[psy_relief_auto]`), MCM switch. Item list accepts kind `none` to exclude an item. Being drunk raises
  every psy gain (up to +100% at full drunkenness) and gives back part of every psy loss (up to 50%), two MCM
  sliders. Check interval 0.5 s, only while a dose is active or the actor is drunk.
- 0.1.0: first test build. Smokes lose their radiation restore (DLTX) and restore psy health; alcohol keeps its
  radiation restore and also restores psy health. Gradual restore over the item's boost time, doses stack and are
  kept in the save. Separate 5-step strength presets for smokes and alcohol, tooltip line, debug log.
