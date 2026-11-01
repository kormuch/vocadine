# VocaDine

Voice-controlled restaurant recommendation assistant for English-speaking tourists in Germany.

## What It Does

VocaDine lets tourists find restaurants in German cities using only their voice. It listens to spoken English, extracts preferences like cuisine, budget, and dietary needs through natural language understanding, then queries Google Places to return ranked recommendations. The system handles the unique challenge of recognizing German city names spoken with English pronunciation through a phonetic alias dictionary covering 2058+ cities.

## Architecture

```
Microphone
    │
    ▼
┌─────────────────────────────────┐
│  STT (Google Web Speech API)    │  en-US speech recognition
└──────────────┬──────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│  NLU Pipeline                   │  spaCy NER (custom trained)
│  ├─ Slot extraction             │  + rule-based fallback
│  └─ City name correction        │  2058 phonetic aliases
└──────────────┬──────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│  Dialog State Manager           │  Tracks 10 slots across turns
└──────────────┬──────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│  Restaurant Search              │  Google Places API (New)
│  ├─ locationBias                │  + TF-IDF cosine similarity
│  └─ Rating bonus ranking        │
└──────────────┬──────────────────┘
               │
               ▼
┌─────────────────────────────────┐
│  TTS (edge-tts)                 │  en-US-JennyNeural voice
└─────────────────────────────────┘
```

## Key Features

- **10 dialog slots**: location, cuisine, budget, diet, group size, occasion, distance, past experience, datetime, special features
- **Custom spaCy NER model** trained for slot extraction from tourist queries
- **German city phonetic aliases**: corrects STT misrecognitions of German city names (2058+ entries)
- **TF-IDF cosine similarity ranking** with rating bonuses for result ordering
- **Multi-turn dialog** with state tracking across conversation turns
- **Session logging** for interaction analysis

## Tech Stack

| Component         | Technology                          |
|-------------------|-------------------------------------|
| Language          | Python                              |
| Speech-to-Text   | Google Web Speech API               |
| Text-to-Speech   | edge-tts (JennyNeural)              |
| Audio playback    | pygame                              |
| NLU              | spaCy (custom NER model)            |
| Ranking          | scikit-learn (TF-IDF)               |
| Restaurant data  | Google Places API (New)             |
| City matching    | Custom phonetic alias dictionary    |

## Quick Start

```bash
git clone https://github.com/kormuch/vocadine.git
cd vocadine
pip install -r requirements.txt
```

Set your Google Places API key, then run:

```bash
python main.py
```

## Dialog Flow

```
Assistant:  Welcome to VocaDine! Which city are you visiting?
User:       I am in Munich.
Assistant:  What kind of food are you looking for?
User:       Something Italian, not too expensive.
Assistant:  How many people in your group?
User:       Just two of us, it is a date night.
Assistant:  Here are my top picks for you...
```

## Context

Academic project developed for the NLP Virtual Assistant course (DLMAIWNLPVA02) at IU International University of Applied Sciences.

