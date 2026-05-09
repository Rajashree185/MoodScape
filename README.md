# 🌊 MoodScape — Emotional Wellness Intelligence Platform

A full-stack AI-powered mood tracking application that combines journaling with sentiment analysis, biometric reflection, ambient soundscapes, and an anonymous community — designed to support student mental wellness.

> **Stack:** React 18 · Vite · Flask · PostgreSQL · Gemini AI · VADER · ElevenLabs TTS · Chart.js · YouTube IFrame API

---

## ✨ Features

### 📓 Smart Journal
- Write daily entries about your thoughts and feelings
- **Hybrid Sentiment Analysis** — VADER heuristic scoring + Gemini 1.5 Flash resolution for sarcasm, slang, and context
- Context-aware mood scoring with trajectory drag, streak compounding, and reversion resistance
- **Mood Intelligence Panel** — rolling averages, velocity, momentum, volatility, streaks, best/worst scores
- Interactive mood chart (Chart.js) with rolling average overlay
- **AI Wellness Suggestions** — contextual coping strategies based on journal content
- **Voice-to-Text** — browser speech recognition for hands-free journaling
- **Text-to-Speech** — ElevenLabs playback of journal entries and AI alerts

### 🧘 Coping Toolkit
- **🎵 Soothing Ambient Music Player** — 12 YouTube-powered soundscapes:
  - *Nature Sounds:* Rain & Thunder, Ocean Waves, Forest & Birds, Babbling Brook, Deep Thunder, Snowy Night
  - *Ambience:* Crackling Fire, Night Crickets, Cafe Ambience
  - *Music & Meditation:* Lo-Fi Chill, Calm Piano, Deep Space
- Categorized track grid with per-track play/pause toggle
- **Ambient Visual Overlay** — dynamic particle effects, animated backgrounds, and glowing themes that match your sound
- Now Playing banner with animated visualization bars
- Volume control with dynamic icon

### ✨ MindMirror — AI Reflection (Privacy-First)
- Camera-based emotional micro-expression analysis
- **100% local** — all processing happens in-browser, no video data sent to servers
- Consent-gated privacy-first activation
- Session-based tracking with emotion spectrum summary

### 👥 Anonymous Community
- Safe, AI-moderated social feed
- Reflective Rejection — negative posts are gently reframed rather than censored
- Support/like system for positive engagement
- Verified user badges

### ⚡ Gamification
- **XP System** — earn points for journal entries and completing exercises
- **Reward Badges** — unlock achievements like "📔 Journal Pro" and "🧘 Mindful Master"
- Persistent progress via localStorage

---

## 🏗 Architecture

```
moodtracker_enhanced/
├── backend/                    # Python Flask API server (port 5000)
│   ├── app.py                  # Routes: auth, journal, community, NLP, TTS, health
│   ├── nlp.py                  # VADER + Gemini hybrid sentiment engine
│   ├── trend.py                # Mood trajectory & momentum analysis
│   ├── chatbot.py              # Sage companion (Gemini + fallback)
│   ├── tts.py                  # ElevenLabs text-to-speech bridge
│   ├── setup_db.py             # Database initialization script
│   ├── requirements.txt        # Python dependencies
│   └── .env                    # Secrets & configuration
│
└── frontend/                   # React + Vite SPA (port 5173)
    └── src/
        ├── App.jsx             # Router with PrivateRoute guard
        ├── App.css             # Full design system (~1380 lines)
        ├── index.css           # Reset/base styles
        └── components/
            ├── api.js          # apiRequest / authFetch / authFetchBlob
            ├── Login.jsx       # Sign-in page
            ├── Register.jsx    # Registration page
            ├── Forgot.jsx      # OTP password reset flow
            └── Dashboard.jsx   # Main app: journal, coping, mind mirror, community
```

### Key Design Decisions

| Concern | Approach |
|---|---|
| **Sentiment** | VADER for low-latency + Gemini for ambiguous cases (hybrid chain) |
| **Auth** | JWT with bcrypt, OTP email reset via Gmail SMTP |
| **Biometrics** | Browser-local only — no server transmission |
| **Moderation** | Reflective Rejection: guides users to reframe, not just censor |
| **Ambient Audio** | YouTube IFrame API (free, reliable streaming) |
| **Mood Trends** | Client-side rolling window calculation for instant dashboard load |

---

## 🚀 Getting Started

### Prerequisites
- **Python 3.11+**
- **Node.js 18+**
- **PostgreSQL** running locally on port 5432

### 1. Database Setup

```bash
# Create the database
psql -U postgres -c "CREATE DATABASE moodtracker;"

# Or run the setup script
cd backend
python setup_db.py
```

### 2. Backend

```bash
cd backend

# Configure environment
cp .env.example .env
# Edit .env with your keys (see below)

# Install & run
pip install -r requirements.txt
python app.py
# → http://localhost:5000
```

### 3. Frontend

```bash
cd frontend
npm install
npm run dev
# → http://localhost:5173
```

---

## 🔐 Environment Variables (`backend/.env`)

| Variable | Description | Required |
|---|---|---|
| `PORT` | Flask server port (default: 5000) | Optional |
| `DATABASE_URL` | PostgreSQL connection string | ✅ |
| `JWT_SECRET` | Secret for JWT signing | ✅ |
| `JWT_EXPIRES_IN_SECONDS` | Token lifetime (default: 3600) | Optional |
| `EMAIL` | Gmail address for OTP emails | For password reset |
| `EMAIL_PASS` | Gmail App Password (16 chars) | For password reset |
| `GEMINI_API_KEY` | Google Gemini API key | For AI features |
| `ELEVENLABS_API_KEY` | ElevenLabs API key | For TTS features |

> **Gmail App Password:** Google Account → Security → 2-Step Verification → App passwords

---

## 📡 API Reference

### Auth (public)

| Method | Route | Body |
|---|---|---|
| POST | `/api/auth/register` | `{ username, email, password }` |
| POST | `/api/auth/login` | `{ username, password }` → `{ token }` |
| POST | `/api/auth/forgot-password` | `{ email }` |
| POST | `/api/auth/reset-password` | `{ email, otp, newPassword }` |

### Protected (`Authorization: Bearer <token>`)

| Method | Route | Description |
|---|---|---|
| POST | `/api/journal` | Submit entry → returns mood, trend, alert |
| GET | `/api/history` | Get all entries for user |
| POST | `/api/sentiment` | Classify text sentiment |
| POST | `/api/chat` | Talk to Sage AI companion |
| POST | `/api/speak` | TTS — returns MP3 audio blob |
| GET | `/api/community` | Fetch community posts |
| POST | `/api/community` | Create post (AI-moderated) |
| POST | `/api/community/:id/like` | Like a community post |
| POST | `/api/clear` | Dev: clear all entries |
| GET | `/health` | Health check |

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 18, Vite, Chart.js, react-chartjs-2, react-router-dom |
| **Backend** | Python 3, Flask, Flask-JWT-Extended, Flask-CORS, bcrypt |
| **Database** | PostgreSQL, psycopg2 |
| **NLP** | VADER (NLTK), Google Gemini 1.5 Flash |
| **Audio** | YouTube IFrame API, ElevenLabs TTS, Web Speech API |
| **Email** | Gmail SMTP (App Password) |

---

## 📄 License

MIT — Built for educational and wellness purposes.
