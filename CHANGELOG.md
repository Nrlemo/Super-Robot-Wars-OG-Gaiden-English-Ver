# Changelog

## RC02 (2026-11-08) — release candidate

Polish pass on RC01 based on tester screenshots.

**Dialogue**
- Dialogue, backlog and Library text size unified to the UI size (width/height **20**; RC01 used 24/28 for dialogue). See the [text-size discussion](https://github.com/Nrlemo/Super-Robot-Wars-OG-Gaiden-English-Ver/discussions/2).
- New line-breaking: lines now fill the dialogue box (~410 px, up to the box edge when needed) instead of breaking early. Original breaks are kept after `...`, `!`, `?!` and sentence-ending `.`; proper names are not split across lines; one-word last lines are avoided.

**Intermission and menus**
- `Stage ##` no longer overlaps the stage number (the number is drawn 18 px further right).
- Select Weapon: «Select Weapon» title restored; header «Attr.» / « Tot» / «Spec.» no longer clipped or glued; « All» tab separated from «Weapon».
- Parts: header «Left / Tot.» fits inside the panel.
- Develop S.Ammo: «Equip Weapons», «Special Ammo», « No Change», «Barr. Pierce»; terrain letters (`Lnd/Sea/Spc`) no longer glued to their values (labels moved 6 px left, range letters 6 px right in the weapon list).
- `Save` in intermission / Personal Data / Load-Save; `Quick Save` only in the map command menu.
- «Mass-Prod. Hückebein Mk. II» shortened to fit its slot.
- Shuffle Battler: the green «EVADE» label (SBBATTLE) is no longer cut off in combat (#121).

**Known issues**: #84 (HP/EN labels drawn twice in Unit Stats — original game behaviour, won't fix), #129 (Special Ammo title slightly off-centre).

**Patches**: `SRWOGG_EN_RC02.xdelta` (from the Japanese ISO) and `SRWOGG_EN_RC01_to_RC02.xdelta` (from the RC01 ISO). Result MD5: `3cedfb6057e690514fe03fee8823614e`.

## RC01 (2026-10-26) — first release candidate

Complete English translation (script, battle quotes, database/Library, menus, tutorial/Q&A, Shuffle Battler, UI graphics). Validated on PCSX2 and OPL. MD5 `7e834ed083969bc379b43cc2a57714d8`.
