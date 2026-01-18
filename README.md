# 🤖 Portable ESP32 AI Voice Assistant (Deepgram + Gemini + TTS)

A complete **Portable AI Voice Assistant** built on **ESP32** using:
- 🎙️ **Deepgram Speech-to-Text (STT)**
- 🤖 **Google Gemini (LLM)**
- 🔊 **Google TTS / OpenAI TTS**
- 📁 **SD Card WAV Recording**
- 🟦🟥🟩 RGB LED + 🔁 Repeat Button + 🔋 Battery Monitoring  
- 🎯 Optional **Wake Word Support (WakeNet + AFE)**

This project demonstrates a full **Embedded + Cloud AI pipeline** where a user speaks into a microphone and the device responds back with voice output.

---

## 🎯 Key Features

🎙️ **Speech-to-Text (Deepgram STT)**  
Records audio in WAV format and uploads it to Deepgram for transcription.

🤖 **AI Response (Google Gemini API)**  
Generates intelligent contextual responses from the user’s query.

🔊 **Text-to-Speech Output (Google / OpenAI TTS)**  
Speaks the AI response through speaker via I2S amplifier.

📁 **SD Card Audio Recording**  
Stores `/Audio.wav` on SD card for stable uploads and debugging.

🟦🟥🟩 **RGB LED State Indicators**  
Shows Recording / Processing / Speaking states.

🔁 **Repeat Response Button**  
Replay the last response without recording again.

🔋 **Battery Voltage Monitoring**  
Low battery detection with voice warning.

📏 **Long Text Handling (Chunk Split)**  
Splits long Gemini responses to avoid crashes and TTS buffer overflow.

🎯 **Wake Word Support (WakeNet + AFE) [Optional]**  
Wake word triggers auto-recording (prototype scaffold included).

---

## 🚀 End-to-End Working Flow

✅ Press RECORD button  
➡️ ESP32 records audio from I2S mic  
➡️ Saves WAV to SD card (`/Audio.wav`)  
➡️ Uploads WAV to Deepgram STT  
➡️ Gets transcription text  
➡️ Sends transcription to Gemini API  
➡️ Receives AI response  
➡️ Converts response to speech (Google/OpenAI TTS)  
➡️ Plays audio on speaker  
➡️ Stores response for Repeat Button

---

## 🛠️ Required Hardware

### Main Components
- ✅ ESP32 Dev Board *(ESP32 / ESP32-S3 recommended)*
- ✅ I2S Microphone: **INMP441**
- ✅ I2S Amplifier: **MAX98357A**
- ✅ Speaker: **4Ω–8Ω (3W recommended)**
- ✅ SD Card Module + microSD Card *(FAT32)*
- ✅ Push Buttons (Record + Repeat)
- ✅ RGB LED *(or separate LEDs)*
- ✅ Battery + Charging module *(optional)*
- ✅ Resistors for voltage divider *(optional battery sensing)*

---

## 🔌 Example Wiring (Reference)

> ⚠️ Pin mapping depends on your board and code configuration.

### 🎙️ INMP441 Microphone (I2S RX)
| ESP32 | INMP441 |
|------|---------|
| 3.3V | VDD |
| GND  | GND |
| GPIO (BCLK/SCK) | SCK |
| GPIO (WS/LRCL)  | WS |
| GPIO (DATA)     | SD |

### 🔊 MAX98357A Amplifier (I2S TX)
| ESP32 | MAX98357A |
|------|------------|
| 5V / VIN | VIN |
| GND | GND |
| GPIO (BCLK) | BCLK |
| GPIO (LRC)  | LRC |
| GPIO (DIN)  | DIN |

---

## 📁 Project Structure

```bash
Portable-ESP32-AI-Voice-Assistant/
├── README.md
├── Portable_Voice_Assistant.ino
├── lib_audio_recording.ino
├── lib_audio_transcription.ino
└── hardware/
    └── wiring.png   # optional wiring diagram
```

---

## ⚡ Quick Start

### ✅ 1) Prerequisites
- Arduino IDE installed
- ESP32 Board package installed
- 2.4GHz WiFi access
- API Keys:
  - Deepgram API Key
  - Google Gemini API Key
  - OpenAI API Key *(optional)*

---

### ✅ 2) Install Required Libraries
Install using Arduino Library Manager:

- **ArduinoJson (v6+)**
- **WiFiClientSecure**
- **SD**
- **SPI**
- ESP32 audio I2S library *(as used in the code)*

---

### ✅ 3) Configure API Keys & WiFi
Update in your main file:

```cpp
#define WIFI_SSID     "YourWiFiName"
#define WIFI_PASSWORD "YourWiFiPassword"

#define deepgramApiKey "YOUR_DEEPGRAM_KEY"
#define gemini_KEY     "YOUR_GEMINI_KEY"
#define OPENAI_KEY     "YOUR_OPENAI_KEY"   // optional
```

---

### ✅ 4) Select TTS Mode
```cpp
#define TTS_MODEL 0
// 0 = Google TTS
// 1 = OpenAI TTS
```

---

### ✅ 5) Upload to ESP32
1. Open `Portable_Voice_Assistant.ino`
2. Select **ESP32 Dev Module**
3. Select COM Port
4. Upload

---

## ✅ API Request/Response Summary (Interview-Ready)

### 🎙️ Deepgram STT
- **Method:** POST  
- **Body:** Binary WAV audio bytes  
- **Response:** JSON with `"transcript"`

### 🤖 Gemini LLM
- **Method:** POST  
- **Body:** JSON prompt  
- **Response:** JSON with answer text

### 🔊 TTS
- Uses Google/OpenAI TTS via library calls
- Streams audio output to speaker

---

## 🔒 Security Notes

This prototype uses:

```cpp
client.setInsecure();
```

✅ Works fast on ESP32 but disables TLS certificate verification.

### Recommended Production Improvements
- Add root CA verification or certificate pinning
- Store secrets in private config file (`config_private.h`)
- Add retry + rate limit handling

---

## 🚀 Future Scope

✅ Full Wake Word Detection (“Hey Wanda”) using ESP-SR WakeNet + AFE  
✅ Conversation memory (multi-turn context)  
✅ OTA updates  
✅ Low power deep sleep  
✅ Better error recovery & logging


