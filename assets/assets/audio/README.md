# Audio assets

Sound files live here. `AudioManager` (`lib/core/audio/audio_manager.dart`)
loads them by the exact filenames below; a missing file simply stays silent —
nothing breaks. The whole directory is registered in `pubspec.yaml`
(`assets/audio/`), so any file added here is bundled automatically.

## Files

| Filename       | Role                                                    | Volume channel             |
| -------------- | ------------------------------------------------------- | -------------------------- |
| `ambient.mp3`  | Looping background music                                | `GameSettings.musicVolume` |
| `battle.mp3`   | Stinger when a battle starts / the turn had battles     | `GameSettings.sfxVolume`   |
| `ui_tick.mp3`  | Click on ending the turn                                | `GameSettings.sfxVolume`   |
| `victory.mp3`  | Flourish on winning the game                            | `GameSettings.sfxVolume`   |
| `march.mp3`    | Two war-drum beats: an army ordered to march            | `GameSettings.sfxVolume`   |
| `attack.mp3`   | Short war-horn blat: an attack ordered                  | `GameSettings.sfxVolume`   |
| `recruit.mp3`  | Stamp, clank of arms, horn call: troops recruited       | `GameSettings.sfxVolume`   |
| `build.mp3`    | Three hammer knocks: a building ordered                 | `GameSettings.sfxVolume`   |
| `capture.mp3`  | Horn flourish: a battle won / a province taken          | `GameSettings.sfxVolume`   |
| `defeat.mp3`   | Low, falling horn: a battle or the game lost            | `GameSettings.sfxVolume`   |
| `charge.mp3`   | Drum burst into a roar: CHARGE! / SALLY OUT!            | `GameSettings.sfxVolume`   |
| `coin.mp3`     | Coin clink: gold gained (pillage)                       | `GameSettings.sfxVolume`   |

## Current state — procedural placeholders

Every file here is an **original procedurally-generated placeholder**
(pure-numpy synthesis, royalty-free, no attribution needed), so the game is
audible out of the box. The generator is `scripts/gen_audio.py`:

- The first four (stereo): a Nordic drone + folk motif loop, a war-horn
  battle stinger, an ascending fanfare, and a UI click.
- The action effects (mono, 0.6–1.5 s): drums, horns, clanks, knocks
  and coins, each loudness-normalized (K-weighted LUFS) below the battle
  stinger so the frequent ones never get tiring.

The generator renders 16-bit 44.1 kHz WAV, then encodes it to MP3 with
`ffmpeg` (LAME VBR `-q:a 2`, no ID3 tag) and deletes the WAV. MP3 plays in
every major browser, and its LAME header records the encoder padding, so the
ambient loop stays gapless. (As WAV these files were 8.5 MB of the web
download; as MP3 they are 0.5 MB.)

`python3 scripts/gen_audio.py assets/audio/` rewrites the action effects —
they are fully seeded, so the output is identical (with the same ffmpeg/LAME
version). Name sounds to make just those (`... assets/audio/ march coin`), or
`all`. Note that `battle`, `ui_tick` and `victory` use unseeded noise, so
regenerating them changes the committed files slightly (`ambient` comes out
identical).

Swap any file for a professional or AI-generated track using the same filename
(keep it MP3 or AAC `.m4a`: `.ogg` doesn't play in older Safari, and `.wav` is
ten times the download; if you change the extension, update the names in
`Sfx` / `_ambientFile` in `audio_manager.dart`).

## Notes

- **Music volume 0** pauses the ambient loop; **effects volume 0** skips SFX.
  Both are driven live from the Settings screen sliders.
- The ambient track is authored to loop seamlessly (tail→head crossfade).
- Keep effect files short and normalized so stingers don't clip.
- Every effect plays on its own fresh player, disposed when it ends, so
  overlapping effects never cut each other off; the same effect re-triggered
  within 100 ms is dropped (double clicks don't stack).
