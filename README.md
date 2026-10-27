# projectWarData

Game data for [projectWar](https://github.com/Axtaris/projectWar), a Warhammer 40,000 army list builder. This repository contains **only data**: one JSON file per faction plus two small index files.

- Current data version: **11.3** (see `DataInfo.json`). The number means *11th edition, third balance update since release*.
- The files are produced by [projectWarDataConverter](https://github.com/Axtaris/projectWarDataConverter) from community sources. They are not written by hand, except for a small set of corrections kept in the converter.

> **Work in progress.** The data has known errors and gaps, listed below. Do not treat it as an authoritative rules reference; check the official publications.

## Using it

projectWar expects these files in its `data/` folder:

```bash
cd projectWar
git clone https://github.com/Axtaris/projectWarData.git data
```

Any other program can read the files directly; they are plain UTF-8 JSON.

## Files

| File | Content |
|---|---|
| `DataInfo.json` | Data set id, data version (`11.3`), minimum app version and publish date. |
| `FactionInfoData.json` | Factions (name, category, colour, allies), battle sizes with detachment points, core stratagems and weapon-ability definitions. |
| `<Faction>.json` | One file per faction: units, detachments and army rules. |
| `<Faction> Legends.json` | Legends datasheets for that faction. **Not updated to 11th edition.** |

### Space Marine chapters

*Space Marines* is one rules family, and the chapters are customisations of it, not separate armies. Every datasheet has the keyword `Adeptus Astartes`; a chapter adds a handful of its own characters and units plus a few detachments. So the data is laid out as:

- `Space Marines.json`: the shared roster (81 datasheets) and the 22 Space Marines detachments. Chapter-only units are not in it.
- One file per chapter (Black Templars, Blood Angels, Dark Angels, Deathwatch, Space Wolves, Imperial Fists, Iron Hands, Raven Guard, Salamanders, Ultramarines, White Scars): **the whole shared roster plus the chapter's own datasheets**. Picking a chapter never takes anything away from the base roster, except units another chapter owns (for example Deathwatch kill teams) and the few units the chapter's Munitorum entry leaves out.
- Detachments per chapter: Black Templars, Blood Angels, Dark Angels, Deathwatch and Space Wolves use the list in their own Munitorum Field Manual file. The six chapters without one get every Space Marines detachment except those locked to another chapter, plus their own.

| Chapter-locked detachment | Chapter |
|---|---|
| Blade of Ultramar | Ultramarines |
| Ceramite Sentinels | Imperial Fists |
| Forgefather’s Seekers | Salamanders |
| Shadowmark Talon | Raven Guard |
| Spearpoint Task Force | White Scars |

Chaos works differently in the sources: Death Guard, Thousand Sons, World Eaters and Emperor's Children are standalone factions with their own full set of datasheets, and Chaos Space Marines has no legion sub-factions (no legion keywords or legion detachments). They are therefore kept as separate files.

## Faction file format

```jsonc
{
  "Name": "Necrons",
  "Units": [ /* datasheets */ ],
  "Dettachments": [ /* detachment rule, enhancements, stratagems */ ],   // spelling kept: it is the key the app reads
  "ArmyRules": [ /* faction-wide rules */ ]
}
```

A unit has, among other fields:

- `Name`, `Faction`, `Keywords`, `FactionKeywords`, `CoreAbilities`
- `StatLines` (M, T, Sv, W, Ld, OC, invulnerable save and base size) and `Weapons` (ranged and melee groups with their abilities)
- `UnitAbilities`, `LeaderInfo` (which units it can lead) and `Infos`
- `UnitComposition`: model groups with size limits, default wargear and the replace/add options, plus wargear definitions (cost, granted abilities, stat adjustments)
- `Upgrades` and `Points`: a list of `{ModelCount, Cost}` entries. When a unit's price depends on how many copies are in the list, `UnitCount` marks the point where a tier changes: for a bounded tier it is the last copy that uses it, for the open-ended tier it is the first copy.

## Where the data comes from

| Data | Source | Policy |
|---|---|---|
| Points, detachment points, enhancement costs | Munitorum Field Manual data from [BSData/wh40k-11e-mfm](https://github.com/BSData/wh40k-11e-mfm) | Always wins for points. |
| Units, weapons, abilities, keywords | [BSData/wh40k-11e](https://github.com/BSData/wh40k-11e) catalogues | Used as the base; known to contain typos. |
| Rules text, loadouts, stratagems, detachment rules | Wahapedia 11th edition data export | Wins over the other sources when they disagree about text or starting wargear. |
| Previously curated fields | Earlier version of this data set | Kept (special abilities, base types, enhancement effects) when no source covers them. |

Units that appear in neither the MFM nor Wahapedia are treated as retired and removed. Legends units are out of scope for 11th edition.

Please check the licence and terms of each source before reusing the data. *Warhammer 40,000* and all related names, rules and datasheets are the property of Games Workshop Ltd; this is an unofficial fan project.

## Known problems

- Wargear options are partly inferred and can differ from the printed datasheet (for example options that exist only as separate lines on the source).
- Some entries still disagree between sources; about 60 units were flagged for manual review and were not all checked.
- Enhancements and detachments new in 11th have text and costs but no machine-readable effects (stat adjustments, restrictions).
- Legends files and Adeptus Titanicus are unchanged and out of date.
- Typos that exist in both upstream sources (for example "unmodifed") are kept as they are.

## Versioning

`DataInfo.json` carries the version. It changes only when the source data changes (a new balance update), not when the converter is improved.

## License

The compilation, structure and corrections made by this project are dedicated to the public domain under CC0 1.0 (see `LICENSE`). The underlying game content is **not** covered by that dedication; see "Where the data comes from".
