![Python](https://img.shields.io/badge/Python-3.8+-blue)
![Platform](https://img.shields.io/badge/Platform-Linux-lightgrey)
![Offline](https://img.shields.io/badge/Offline-Capable-brightgreen)
![License](https://img.shields.io/badge/License-MIT-orange)
![Version](https://img.shields.io/badge/Version-2.0.0-blueviolet)

# C.R.A.V.I.A.S.
### Computer Response Automation - Voice Input Assistance

## Why I Built This?

Pure laziness. Started this back in 2020 because pressing buttons felt like too much effort. If I could just say what I wanted done instead of typing it, that'd be the dream. So I built the first version, it did the basics, and it just kept growing from there into whatever this is now.

## What It Actually Is

A voice assistant that runs on your Linux machine and controls it. You talk, it does things. Searches the web, plays your music, opens apps, types for you, scrolls, takes screenshots, locks the screen, shuts down. The whole OS is basically fair game.

The brain isn't keyword matching. It uses TF-IDF semantic similarity to figure out what you mean, so you don't have to say things exactly right. If it's not confident enough, it asks before acting. If you correct it, *"no I meant scroll down"*, it learns that for next time. You can also just straight up teach it: *"whenever I say tunes, play music"* and it'll remember.

Memory works the same way. Tell it your name, what you like, drop a note. It stores everything locally and recalls it when relevant. Nothing leaves your machine.

Three ways to run it: voice, text, or silent mode if you want voice input but no TTS output.

## Jump-Start

**1. System dependencies**
```bash
sudo apt install espeak portaudio19-dev python3-pyaudio
sudo apt install vlc   # or mpv, for local music
```

**2. Python dependencies**
```bash
pip install -r requirements.txt
```

**3. Offline voice model**

Skip this and it falls back to Google STT, which needs internet.
```bash
cd ~
wget https://alphacephei.com/vosk/models/vosk-model-small-en-us-0.15.zip
unzip vosk-model-small-en-us-0.15.zip
mv vosk-model-small-en-us-0.15 vosk-model-small-en-us
```

**4. Run it**
```bash
python main.py            # voice mode
python main.py --text     # text mode, no mic needed, good for testing
python main.py --silent   # voice input, text output only, no TTS
```

## What You Can Tell It

**Search and web:** *"search for..."*, *"wikipedia..."*, *"open YouTube"*, *"go to..."*

**Time and date:** *"what time is it"*, *"what's today's date"*

**Music:** *"play music"*, *"play something by..."*, *"shuffle"*, *"stop"*

**System:** *"volume up/down"*, *"mute"*, *"battery status"*, *"CPU and RAM"*, *"open Firefox"*, *"lock screen"*, *"shutdown"*, *"reboot"*

**Keyboard and automation:** *"type..."*, *"copy"*, *"paste"*, *"undo"*, *"save"*, *"press enter"*, *"close window"*, *"scroll down"*, *"screenshot"*, *"fullscreen"*. Basically any key you'd otherwise have to press yourself.

**Memory:** *"remember that..."*, *"what do you know about me"*

**Teaching:** *"whenever I say X, mean Y"*. It saves that mapping and uses it from then on.

## Configuration

Everything lives in `config/settings.yaml`. Voice engine, TTS model path, music directory, browser, whether shutdown commands are allowed, memory limits, your name, humor level. Commented throughout so it's easy to navigate.

## Project Structure

```text
crax/
├── main.py                   # entry point, run this
├── requirements.txt
├── config/
│   └── settings.yaml         # everything configurable is here
├── core/
│   ├── assistant.py          # wires everything together, runs the loop
│   ├── brain.py              # intent engine, TF-IDF semantic matching
│   ├── listener.py           # STT, Vosk offline / Google fallback
│   └── speaker.py            # TTS, Piper / espeak / pyttsx3
├── modules/
│   ├── greet.py              # personality, small talk, confirmations
│   ├── memory.py             # conversation history + learning
│   ├── music.py              # local music via VLC or MPV
│   ├── search.py             # web search + Wikipedia
│   ├── system.py             # OS controls
│   └── keyboard.py           # keyboard automation
└── data/
    ├── memory.json           # conversation history (auto-generated)
    ├── learned.json          # facts learned about you (auto-generated)
    └── commands.json         # phrase aliases you've taught it (auto-generated)
```

## Something Not Working?

**No audio:** `sudo apt install alsa-utils` then check `aplay -l`

**Vosk model not found:** Make sure the folder is named exactly `vosk-model-small-en-us` in your home directory, or update the path in `settings.yaml`.

**Keyboard module not doing anything:** Check if `pynput` installed correctly. Some systems need extra permissions for input control.

**It keeps asking for confirmation on everything:** The semantic confidence threshold might be too high. Tweak it in `brain.py` or just teach it your phrasing directly.

## License

MIT. See LICENSE.
