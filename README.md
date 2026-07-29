# Warble Tone Generator

A browser-based **warble tone generator** for sound-field audiometry, built with the
Web Audio API. It produces a frequency-modulated pure tone whose frequency sweeps
back and forth around a center frequency to minimize standing-wave effects in a room.

A single, dependency-free `index.html` — no build step, no server, no install.

## A personal note

This tool has a personal origin. I built it while helping my 3-year-old get
comfortable responding to warble tones ahead of an audiometry test - letting them
press the button and hear the familiar sound at home took the mystery
(and a lot of the nerves) out of the clinic visit. I'm sharing it in case it helps
another family prepare the same way.

## Features

- **Center Frequency** — adjustable from 125 Hz to 8000 Hz
- **Warble Rate** — modulation frequency, 3–10 Hz (typical 4–6 Hz)
- **Warble Depth** — modulation depth, 5–25% of center frequency (typical 10–20%)
- **Volume** — 0–100%
- All controls update the tone **live** while playing
- Works on any modern browser (desktop or mobile)

## Usage

1. Open [`index.html`](index.html) directly in a web browser — double-click it, or use:
   ```bash
   # Option A: just open the file
   xdg-open index.html      # Linux
   open index.html          # macOS

   # Option B: serve it locally
   python3 -m http.server 8000
   # then visit http://localhost:8000
   ```
2. Set the desired frequency, warble rate, depth, and volume.
3. Press **▶ Start Warble Tone**. Adjust controls while it plays. Press **■ Stop** when done.

> **Browser autoplay policy:** the tone starts on a button click, which satisfies
> browsers' requirement for a user gesture before audio can play.

## How it works

The warble tone is created with two Web Audio oscillators connected for
**frequency modulation (FM)**:

| Node | Role |
| --- | --- |
| `oscillator` (carrier, sine) | Produces the pure tone at the center frequency |
| `modulatorOscillator` (LFO, sine) | Sweeps the carrier's frequency at the warble rate |
| `modulatorGain` | Scales the LFO to `centerFreq × depth`, setting the peak frequency deviation |
| `gainNode` | Final output volume |

Connection graph:

```
modulatorOscillator → modulatorGain → oscillator.frequency   (FM)
oscillator → gainNode → destination                            (audio out)
```

So the instantaneous frequency oscillates as
`f(t) = f_center + (f_center × depth) × sin(2π × rate × t)`.

## ⚠️ Safety & disclaimer

- **Keep volumes safe.** Sustained or loud tones can damage hearing. Start low.
- **This tool is not a medical device** and must not be used for self-diagnosis.
- Clinical audiometry should be performed by qualified professionals using calibrated
  equipment. Browser audio output is **not calibrated** — dB values are relative only.

## Acknowledgements

The initial version of this tool (the `index.html` application and this README)
was generated using [Anthropic's Claude](https://www.anthropic.com).

## License

None specified. Treat as the author's personal tool unless otherwise noted.
