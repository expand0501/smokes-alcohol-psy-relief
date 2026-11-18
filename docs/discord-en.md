## Description
A smoke break or a drink calms the nerves, it doesn't flush radiation. Smokes now restore **psy health** instead of radiation. Alcohol keeps its radiation restore and restores psy health too.

## Features
- All cigarettes, the Cuban cigar and tobacco lose their radiation restore and restore psy health gradually over the item's effect time. Several items stack.
- Alcohol (vodka, beer, ...) restores psy health on top of its usual effects.
- **Being drunk helps:** all psy restore is raised and part of every psy loss is given back, scaling with how drunk you are. It can't save you from one lethal psy hit.
- Tooltip row in the item's stats block shows how much psy health one use restores.
- Smokes and alcohol from other mods are detected automatically. MCM configurable.
- Strength presets (Light / Mild / Medium / Potent / Strong) for smokes and for alcohol are MCM configurable. The drunk bonus and protection each have their own preset, slider and on/off switch in MCM.
- No game files replaced (DLTX). No per-frame work while you are sober and nothing is restoring.

## Install / remove
Safe to install and to remove in the middle of a playthrough: the save holds only a small list of restores in progress. After removal smokes restore radiation again.
Needs G.A.M.M.A. with the default modded exes (drunk effects need the engine's drunkenness value; without it only they are off).

## Download
https://github.com/expand0501/smokes-alcohol-psy-relief/releases

## Bug reports
Post them here with:
- `xray_*.log` from `Anomaly\appdata\logs\` (lines with `[PsyRelief]`)
- mod version, GAMMA version, engine
- what happened vs. expected, and steps to reproduce
- your MCM settings and other mods touching smokes, alcohol or psy
