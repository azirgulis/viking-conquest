# Audio assets

Sound files live here. `AudioManager` (`lib/core/audio/audio_manager.dart`)
loads them by the exact filenames below; a missing file simply stays silent —
nothing breaks. The whole directory is registered in `pubspec.yaml`
(`assets/audio/`), so any file added here is bundled automatically.

## Files

| Filename       | Role                                                    | Volume channel             |
| -------------- | ------------------------------------------------------- | -------------------------- |
| `ambient.wav`  | Looping background music                                | `GameSettings.musicVolume` |
| `battle.wav`   | Stinger when a battle starts / the turn had battles     | `GameSettings.sfxVolume`   |
| `ui_tick.wav`  | Click on ending the turn                                | `GameSettings.sfxVolume`   |
| `victory.wav`  | Flourish on winning the game                            | `GameSettings.sfxVolume`   |
| `march.wav`    | Two war-drum beats: an army ordered to march            | `GameSettings.sfxVolume`   |
| `attack.wav`   | Short war-horn blat: an attack ordered                  | `GameSettings.sfxVolume`   |
| `recruit.wav`  | Stamp, clank of arms, horn call: troops recruited       | `GameSettings.sfxVolume`   |
| `build.wav`    | Three hammer knocks: a building ordered                 | `GameSettings.sfxVolume`   |
| `capture.wav`  | Horn flourish: a battle won / a province taken          | `GameSettings.sfxVolume`   |
| `defeat.wav`   | Low, falling horn: a battle or the game lost            | `GameSettings.sfxVolume`   |
| `charge.wav`   | Drum burst into a roar: CHARGE! / SALLY OUT!            | `GameSettings.sfxVolume`   |
| `coin.wav`     | Coin clink: gold gained (pillage)                       | `GameSettings.sfxVolume`   |

## Current state — procedural placeholders

Every `.wav` file here is an **original procedurally-generated placeholder**
(pure-numpy synthesis, royalty-free, no attribution needed), so the game is
audible out of the box. The generator is `scripts/gen_audio.py`:

- The first four (16-bit stereo): a Nordic drone + folk motif loop, a war-horn
  battle stinger, an ascending fanfare, and a UI click.
- The action effects (16-bit mono, 0.6–1.5 s): drums, horns, clanks, knocks
  and coins, each loudness-normalized (K-weighted LUFS) below the battle
  stinger so the frequent ones never get tiring.

`python3 scripts/gen_audio.py assets/audio/` rewrites the action effects —
they are fully seeded, so the output is byte-identical. Name sounds to make
just those (`... assets/audio/ march coin`), or `all`. Note that `battle`,
`ui_tick` and `victory` use unseeded noise, so regenerating them changes the
committed files slightly (`ambient` comes out identical).

Swap any file for a professional or AI-generated track using the same filename
(any format audioplayers supports — `.wav`, `.mp3`, `.ogg`; if you change the
extension, update the names in `Sfx` / `_ambientFile` in `audio_manager.dart`).

## Notes

- **Music volume 0** pauses the ambient loop; **effects volume 0** skips SFX.
  Both are driven live from the Settings screen sliders.
- The ambient track is authored to loop seamlessly (tail→head crossfade).
- Keep effect files short and normalized so stingers don't clip.
- Every effect plays on its own fresh player, disposed when it ends, so
  overlapping effects never cut each other off; the same effect re-triggered
  within 100 ms is dropped (double clicks don't stack).
