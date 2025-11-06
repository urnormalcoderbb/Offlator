# Offlator

Offlator is an offline translation device built with a Raspberry Pi 4 that combines speech recognition and language translation capabilities for English-Hindi translations.

## Hardware Components

- Raspberry Pi 4
- 3 Push Buttons
  - Button 1: Record (GPIO 17)
  - Button 2: Cancel (GPIO 27)
  - Button 3: Shutdown (GPIO 22)
- Rotary Encoder (for language direction selection)
  - CLK: GPIO 5
  - DT: GPIO 6
  - SW: GPIO 13
- OLED Display I2C (shows both original and translated text)
  - SDA: GPIO 2 (PIN 3)
  - SCL: GPIO 3 (PIN 5)
- USB Microphone (for voice input)
- Speaker/Headphones (3.5mm audio jack)

## Features

- Offline speech-to-text using Whisper
- Translation using MarianMT models (English ↔ Hindi)
- Hardware interface with buttons and rotary encoder
- Visual feedback through OLED display
- Text-to-Speech output using eSpeak
- Bidirectional translation support
- One-click recording and translation
- Safe shutdown capability

# Offlator

[Previous sections remain the same until Project Structure]

## Project Structure

```
offlator/
├── README.md
├── requirements.txt
├── src/
│   ├── __init__.py
│   ├── main.py              # Main application entry point
│   ├── config.py            # Configuration and pin settings
│   ├── hardware/
│   │   ├── __init__.py
│   │   ├── buttons.py       # Button handling
│   │   ├── display.py       # OLED display interface
│   │   └── encoder.py       # Rotary encoder interface
│   ├── audio/
│   │   ├── __init__.py
│   │   ├── recorder.py      # Audio recording functions
│   │   └── speaker.py       # TTS and audio output
│   └── translation/
│       ├── __init__.py
│       ├── whisper_stt.py   # Speech to text
│       └── translator.py     # Text translation
├── models/                   # Pre-downloaded models
│   ├── whisper/
│   └── marian/
└── tests/
    ├── __init__.py
    ├── test_hardware.py
    ├── test_audio.py
    └── test_translation.py
```

### Create Project Structure

```bash
# Create main project directory
mkdir offlator
cd offlator

# Create README and requirements files
touch README.md
touch requirements.txt

# Create source directory structure
mkdir -p src/{hardware,audio,translation}
touch src/__init__.py
touch src/main.py
touch src/config.py

# Create hardware module files
touch src/hardware/__init__.py
touch src/hardware/buttons.py
touch src/hardware/display.py
touch src/hardware/encoder.py

# Create audio module files
touch src/audio/__init__.py
touch src/audio/recorder.py
touch src/audio/speaker.py

# Create translation module files
touch src/translation/__init__.py
touch src/translation/whisper_stt.py
touch src/translation/translator.py

# Create models directory structure
mkdir -p models/{whisper,marian}

# Create tests directory and files
mkdir tests
touch tests/__init__.py
touch tests/test_hardware.py
touch tests/test_audio.py
touch tests/test_translation.py

# Set proper permissions
chmod 755 src tests models
```

## Technical Requirements

### Hardware
- Raspberry Pi 4 (2GB RAM minimum recommended)
- SD Card (32GB minimum recommended)
- OLED Display (SSD1306 128x64)
- KY-040 Rotary Encoder
- 3 Push Buttons
- USB Microphone
- Speaker/Headphones
- Breadboard and jumper wires

### Software Dependencies

```bash
# System dependencies
sudo apt-get update
sudo apt-get install -y \
    python3-pip \
    python3-venv \
    espeak \
    portaudio19-dev \
    python3-dev \
    i2c-tools \
    libi2c-dev

# Create and activate virtual environment
python3 -m venv venv
source venv/bin/activate

# Install Python packages
pip3 install -r requirements.txt
```

### requirements.txt
```
torch==2.0.1
transformers==4.30.2
sounddevice==0.4.6
numpy==1.24.3
adafruit-circuitpython-ssd1306==2.12.4
RPi.GPIO==0.7.1
pyaudio==0.2.13
python-espeak==0.5.0
```

## Hardware Setup

### Wiring Diagram

```
Raspberry Pi 4     Component
---------------------------------
GPIO 17 --------- Record Button
GPIO 27 --------- Cancel Button
GPIO 22 --------- Shutdown Button

GPIO 5 ---------- Rotary CLK
GPIO 6 ---------- Rotary DT
GPIO 13 --------- Rotary SW

GPIO 2 (SDA) ---- OLED SDA
GPIO 3 (SCL) ---- OLED SCL

3.3V ------------ VCC (Buttons/Rotary/OLED)
GND ------------- GND (Buttons/Rotary/OLED)
```

## Installation

1. Clone the repository:
```bash
git clone https://github.com/urnormalcoderbb/Offlator.git
cd Offlator
```

2. Set up virtual environment and install dependencies:
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

3. Enable I2C interface:
```bash
sudo raspi-config
# Navigate to Interface Options > I2C > Enable
```

4. Download models:
```bash
python3 src/download_models.py
```

## Usage

1. **Start the Application:**
```bash
python3 src/main.py
```

2. **Translation Operation:**
   - Press Button 1 (GPIO 17) to start recording
   - Speak clearly into the microphone
   - Release Button 1 to process translation
   - Text appears on OLED display
   - Audio output plays through speaker

3. **Language Selection:**
   - Rotate encoder clockwise: English to Hindi
   - Rotate encoder counter-clockwise: Hindi to English

4. **Cancel Operation:**
   - Press Button 2 (GPIO 27) anytime to cancel

5. **Shutdown:**
   - Press Button 3 (GPIO 22) to safely shutdown

## Status Information

The OLED display shows:
- Current translation mode (EN→HI or HI→EN)
- Recording status
- Processing status
- Error messages
- Original text
- Translated text

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT License

Copyright (c) 2025 urnormalcoderbb

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
