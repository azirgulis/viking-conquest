# Audio assets

Sound files live here. `AudioManager` (`lib/core/audio/audio_manager.dart`)
loads them by the exact filenames below; a missing file simply stays silent —
nothing breaks. The whole directory is registered in `pubspec.yaml`
(`assets/audio/`), so any file added here is bundled automatically.

## Music (`MusicMood`, `GameSettings.musicVolume`)

| Filename   | Plays                                                      |
| ---------- | ---------------------------------------------------------- |
| `lyre.mp3` | Everywhere: a plucked lyre and wooden flute, calm, 2.4 min |

It repeats with a few seconds' rest between plays. Music is off until the
player turns it on. Browsers play nothing until the page is first touched,
so on web the music starts with the first tap. (Of four generated tracks the
user kept this one; the bowed-lyre menu theme was dropped as terrible.)

## Effects (`Sfx`, `GameSettings.sfxVolume`)

| Filename      | Role                                                 |
| ------------- | ---------------------------------------------------- |
| `battle.mp3`  | A soft, distant horn call: a battle begins           |
| `ui_tick.mp3` | Wooden knock: ending the turn                        |
| `victory.mp3` | Horns, drums and cheering: the game won              |
| `march.mp3`   | War drums and footsteps: an army ordered to march    |
| `attack.mp3`  | A low, distant horn: an attack ordered               |
| `recruit.mp3` | Mail, spears and a shout: troops recruited           |
| `build.mp3`   | Hammer blows on timber: a building ordered           |
| `capture.mp3` | A short lyre strum: a battle won                     |
| `defeat.mp3`  | A low, falling horn: a battle or the game lost       |
| `charge.mp3`  | Drums into a battle cry: CHARGE! / SALLY OUT!        |
| `coin.mp3`    | Coins clinking: gold gained                          |

## Where they come from

Generated on fal.ai in Oct 2026 — effects with ElevenLabs Sound Effects v2,
music with Stable Audio 2.5 — then trimmed, faded, levelled (EBU R128:
stingers about -16 LUFS, frequent sounds -20 to -24, music -20), cut above
2.5 kHz (the user: "no high pitched sounds") and
encoded to MP3 by `scripts/fal_audio.py`, which keeps the prompts, the takes
chosen and the music seeds. (The procedural placeholders before them came
from a numpy synthesizer, since removed.)
