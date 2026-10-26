# Super Robot Wars OG Gaiden — English Fan Translation

![Platform: PlayStation 2](https://img.shields.io/badge/platform-PlayStation%202-555555)
![Translation: Japanese to English](https://img.shields.io/badge/translation-Japanese%20%E2%86%92%20English-blue)
![Status: In development](https://img.shields.io/badge/status-in%20development-orange)
![Font: VWF](https://img.shields.io/badge/font-variable--width%20(VWF)-purple)

A work-in-progress English fan translation of **Super Robot Wars Original Generation: Gaiden** for the PlayStation 2.

The goal is to make the game easier to play in English while preserving its story, character voices, terminology, menus, and original presentation. This repository contains the translation work and the tools used to modify and test the game.

> **Project status:** active development. English draft text exists across much of the game, but review and in-game testing are still ongoing. There is no finished public patch release yet.

### We need your support! 💫

While every line is translated and the game is fully playable from the title screen to the battle animations, we are looking for players to test this build on emulators and physical consoles. 💥

Grab your original Japanese ISO, check the README.md for the quick xdelta patching instructions, and help us polish this masterpiece! Please report any visual bugs🐛, or text issues on our GitHub.

**AND CONSIDER SUPPORTING FURTHER DEVELOPMENT!** 

<a href="https://www.buymeacoffee.com/Srwa_en" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-blue.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>

## Screenshots

The image slots below are reserved for captures from the translated build. They can be replaced with real screenshots as visual testing progresses.

| Title screen | Story dialogue |
|---|---|
| _Screenshot placeholder — add `screenshots/title-screen.png`_ | _Screenshot placeholder — add `screenshots/story-dialogue.png`_ |
| **Menu and Library** | **Battle scene** |
| _Screenshot placeholder — add `screenshots/menu-library.png`_ | _Screenshot placeholder — add `screenshots/battle-scene.png`_ |

## What is being translated?

| Area | Current state |
|---|---|
| Story dialogue | English draft is available for most tracked lines; review and playtesting remain |
| Battle quotes | English drafts are prepared for the tracked phrases; style and in-game checks remain |
| Database and Library | Unit, weapon, pilot, ability, glossary, and card descriptions have English drafts; some entries still need review or follow-up |
| Menus and system text | Many executable and overlay strings have been translated; layout checks and remaining labels are in progress |
| Help, tutorials, terrain, and music labels | Several groups have English text; the remaining miscellaneous strings are being handled separately |
| Graphics and UI textures | Some menu labels and game graphics have been edited; not every change has been visually verified |

The latest tracked count, recorded on **October 19, 2026**, was approximately **76,300 of 98,000 text records with English draft text (about 78%)**. This is a measure of draft coverage, **not** of finished, reviewed, or fully playable content.

## How does it work?

The original game stores text in several different formats. Some content is ordinary text, while other labels are stored inside graphics or packed data files. The project uses small tools to handle those formats safely.

1. **Extract:** game text is read into editable files so it can be translated without manually changing the disc image.
2. **Translate and review:** text is worked on in batches, with story context, character notes, and a shared glossary to keep names and terms consistent.
3. **Fit the text:** the game was designed for Japanese characters, which usually take a fixed amount of space. The project adds a **variable-width font (VWF)** so English letters can use the space they actually need.
4. **Reinsert and rebuild:** the tools put the English text back into the game data and rebuild a test disc image. Checks help catch malformed text, broken data, and strings that would be cut off.
5. **Test in the game:** experimental builds are checked in PCSX2, with automated smoke tests and memory checks supporting manual visual review.

In simple terms, the project is not just replacing Japanese words with English ones. It also adapts the game's text layout and data so the English can fit and the game can continue to run correctly.

## Technical highlights

- Custom extractors and rebuilders for the story script, battle lines, menus, databases, and other game data.
- A patched rendering routine and an English font with variable character widths.
- Re-implemented compression support for packed game data where needed.
- Text validation and byte-for-byte round-trip checks for supported formats.
- Automated build and emulator-testing tools, including a patch-generation workflow.

The main build entry point is the root-level build script. The more detailed environment and implementation notes are linked below.

## Testing status

Experimental builds have been tested in PCSX2, including checks of the opening stage and selected menus and Library screens. Work is still ongoing, and **the translated game has not completed full end-to-end validation on real PS2 hardware / Open PS2 Loader (OPL)**. Some recent fixes still need to be included in a rebuilt image and rechecked in-game.

Treat the current build output as development material, not as a final release.

## Repository guide

- [Detailed development status (Spanish)](docs/PROJECT_STATUS_ES.md) — preserved from the previous project README, including progress notes and issue references.
The source code, build tools, translation data, and detailed working notes have not yet been migrated from the original private workspace. This repository currently holds the public-facing project documentation.

## Project scope and rights

This is an unofficial, non-commercial fan translation project. It is not affiliated with or endorsed by Banpresto, Bandai Namco, or the original rights holders.

The repository is a private development workspace. It does not announce a public release, and no original game disc image or console BIOS is provided. Use only game data obtained from a copy you are legally entitled to use. All trademarks and copyrighted material belong to their respective owners.



<a href="https://www.buymeacoffee.com/Srwa_en" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me a Coffee" style="height: 60px !important;width: 217px !important;" ></a>
