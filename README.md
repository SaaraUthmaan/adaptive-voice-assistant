# Adaptive Voice Assistant

A working prototype of **Adaptive Mode**: a setting that any voice assistant or voice search app could turn on so it adapts to the person speaking: their **pace**, their **speech**, and their **memory**.

**Live demo:** https://saarauthmaan.github.io/adaptive-voice-assistant/ (open it in Chrome or Edge and allow the microphone). You can also type, and pick a speaking style to stand in for your voice.

## What Adaptive Mode does

| | Adaptive Mode off (standard assistant) | Adaptive Mode on |
|---|---|---|
| **Pace** | Fixed voice speed; stops listening after a short silence | Measures your words per minute and pauses, matches its speaking speed to you, and waits longer in silence (1.2–4.5 s) so it doesn't cut you off. A ring around the mic shows how long it will wait |
| **Reply style** | Everything at once | Slower speakers get short sentences, pauses between them, and long lists read three at a time |
| **Speech** | Takes words exactly as heard | Removes fillers ("um", "uh"), repeated words, stutters and false starts; handles self-corrections ("five, no, ten minutes"); learns your own phrases ("when I say tea time I mean…") |
| **Unfinished requests** | "Sorry, I didn't understand" | Holds what you've said so far ("set a timer for…") and completes it with your next sentence |
| **Word-finding** | Fails | "add the thing to my list" → waits for the word and reminds you what you last added |
| **Memory** | None between turns | "What was I doing?" gives a recap of recent actions, running timers, reminders and anything left unfinished; greets you with where you left off after a break |

The panel on the right shows each adaptation as it happens (what it heard vs. what it understood), which makes it useful for side-by-side usability testing: try the same sentence with the mode on and off.

## What it can do

Timers, shopping and other lists, reminders (with or without a time), notes, time and date, simple maths, "say that again", "slow down" / "speed up", and remembering your name.

## How it works

One self-contained `index.html`, with no build step and no server.

- **Speech in:** the browser's Web Speech API (`SpeechRecognition`), with its own end-of-speech timer that scales with your measured pauses
- **Speech out:** `speechSynthesis`, with the rate set from your pace
- **Understanding:** a rule-based pipeline: clean-up → self-correction → intent matching, plus a fuzzy fallback that asks you to confirm its guess
- **Memory:** `localStorage` in your own browser only; nothing is sent anywhere

## Browser support

Voice input works in Chrome, Edge and Safari over HTTPS (GitHub Pages). Firefox has no built-in speech recognition, so it falls back to typing. Everything else works in all modern browsers.

## Run locally

Open `index.html` in Chrome, or serve the folder with `python3 -m http.server`.

## Limitations

This is a prototype. Intent matching uses patterns, not a language model, so it covers the tasks above rather than open conversation. Pace estimates come from the timing of recognition results and are approximate.
