# The Common House

The radio in the old telecom office never goes fully quiet. Most nights it
carries ordinary voices from other refuges. Some calls come from somewhere
else.

The Common House is a first-person narrative investigation game in
development, built in Unity. You play a newcomer at a converted
telecommunications office that a found family has made into a home and a radio
outpost, some months after an incident that disrupted time and space. Strange
calls lead to cases about the people who took you in. The tone leans toward
suspense, mystery and unease.

It is an early proof of concept. There is no public build or release, and
character names, story details and art are working material that will change.

![Style board render of the radio desk](docs/design/style-board/radio-desk.jpg)

*Style board render. More in [docs/design/style-board](docs/design/style-board.md).*

## What is playable

- **Case 01**, the first case: receive a call, inspect documents, review
  transmissions, compare sources in a notebook, talk to residents, check who a
  response will reach, and confirm or refuse a send. The case has three
  outcomes, each with its own household aftermath.
- **The outpost**: a converted-office home base with first-person movement,
  the three Case 01 rooms and provisional shared spaces (concourse, reception
  commons, repair hackerspace, sleeping bay, utility room). Upper-floor
  interiors are not built.
- **Residents** with talking, sitting and walking routines who turn to face
  the player in conversation.
- **Arrival intro**: a separate playable opening scene with the arrival,
  radio, greeting and orientation. It is not yet connected to Case 01.
- **Saves**: local, versioned saves with save, checkpoint and load.
- **Keyboard play**: every step of the Case 01 route works from the keyboard,
  with no timed choices, no colour-only decisions and captions for essential
  audio. Text scales from 100% to 150%.
- **Look**: HDRP on Vulkan, rendered in a low-poly retro style, with CC0
  scanned materials and models.

Asset kits for later episodes (city streets for flashbacks, a Season 1 prop
kit, an exterior camera set) have their own review scenes and are not in the
playable build.

### Controls

`WASD` moves and the arrow keys look. `E` opens the focused object, `Enter`
reads, asks, marks or confirms, and `Escape` closes the current panel or
pauses. `N` opens the notebook, `C` and `Tab` compare evidence, `V` checks a
route. `F5` saves, `F8` sets a checkpoint, `F9` loads.
See [keyboard route](docs/development/keyboard-route.md).

## Quick start

You need:

- Unity `6000.4.10f1` with Linux build support (the exact version is in
  `ProjectSettings/ProjectVersion.txt`) and a Vulkan-capable GPU
- Git LFS: textures, models and animations under `Assets/` are LFS objects
- Python 3.10 or newer for the scripts and checks

```sh
git clone https://github.com/subculture-collective/the-common-house.git
cd the-common-house
git lfs install
git lfs pull
```

Open the project folder in the Unity Editor, then open
`Assets/CommonHouse/Scenes/OutpostBlockout.unity` and press Play.

The helper scripts copy `Assets/`, `Packages/` and `ProjectSettings/` into a
disposable project, run the Editor there and remove the copy afterward. Pass
your Editor with `--editor /path/to/Editor/Unity` or the `UNITY_EDITOR`
environment variable:

```sh
python3 scripts/check_unity_import.py --editor /path/to/Editor/Unity     # fresh import
python3 scripts/run_unity_tests.py --platform editmode --filter CommonHouse.Tests.ProjectSmokeTests --editor /path/to/Editor/Unity
python3 scripts/run_unity_tests.py --platform playmode --filter CommonHouse.Tests.ProjectPlayModeSmokeTests --editor /path/to/Editor/Unity
python3 scripts/build_poc.py --target local --smoke --editor /path/to/Editor/Unity   # Linux player build and scripted run
```

The Editor needs your normal Unity licence; the scripts never install Unity or
activate a licence.

Checks that need no Unity:

```sh
python3 -m unittest discover -s tests   # tests for the Python tools
python3 scripts/check_docs.py           # local Markdown links resolve
python3 scripts/check_binary_sizes.py   # binary size and LFS policy
python3 scripts/check_provenance.py     # asset provenance manifest
```

More commands, including scene and asset regeneration, are in the
[development guide](docs/development.md).

## Assets

Textures and models under `Assets/` are stored with Git LFS. Other binaries,
such as documentation images, stay in plain Git, and `check_binary_sizes.py`
fails on any plain-Git binary over 5 MiB or an LFS-pattern file committed
without an LFS pointer. Scanned assets must be CC0 and listed in the
[provenance manifest](docs/development/asset-sourcing.md).

## Design documents

`docs/design/` is the design source. Start with the
[design index](docs/design/README.md), then:

- [Concept brief](docs/design/concept-brief.md)
- [Game design document](docs/design/game-design-document.md)
- [Proof of concept plan](docs/design/proof-of-concept-plan.md)
- [First case](docs/design/case-01-the-road-is-clear.md)
- [Decisions and open questions](docs/design/decisions-and-open-questions.md)

Some design notes contain story spoilers.

## Project layout

```text
Assets/CommonHouse/   game code, scenes, art, content and tests
Packages/             Unity package manifest and lock
ProjectSettings/      Unity project settings and Editor version
scripts/              build, test, capture and check tools (Python)
tests/                tests for the Python tools
tools/                Blender character and mesh-decimation helpers
docs/design/          concept, game design, story and art direction
docs/development/     how each system works and how to test it
```

## Contributing

Open a pull request on GitHub against `main`. CI runs the Python tool tests,
the documentation link check and the binary-size and provenance checks on
every pull request; they must pass before it merges. Unity tests are not run
in CI, so run the relevant ones locally.

## License

Original contributions are licensed under the GNU General Public License,
version 3 or any later version (`GPL-3.0-or-later`). See [LICENSE](LICENSE).
Third-party code, fonts, data and other assets keep their own licences and
attribution.
