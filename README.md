# 🌿 Nature Bingo

A bingo game that gets you off the screen and into the world. Gemma generates a bingo card for where you are, you go outside and photograph what you find, and Gemma checks each photo and shares a fun fact. Finish with a Wordle-style result to share with friends.

Built for the [Hacktoberfest Open-Source AI Challenge, Week 1: Touch Grass](https://dev.to/challenges/hacktoberfest-week1-2026-10-05).

**Demo:** `https://nature-bingo.streamlit.app/`

## How it works

| Step | What happens |
|---|---|
| 1. Setup | Pick a 3×3 or 4×4 grid and a difficulty. Optionally add your region, season and setting. |
| 2. Generate | Gemma creates a bingo card of visual, photographable squares that fit your context. |
| 3. Explore | Walk around. Tap **📷 Photo** on a square and upload a picture of what you found. |
| 4. Verify | Gemma checks the photo against the square and answers in a popup: "🎉 Great job!" plus a fun fact. |
| 5. Finish | Complete the card, or tap **End game** at any time. You get your found count, bingos, total time and a shareable text. |

## Share text

```
🌿 Nature Bingo · 10 Oct 2026
Nagpur · Monsoon · Medium · 3×3

🟩🟩⬜
🟩🟩🟩
⬜🟩🟩

7/9 found · 2 bingos
⏱️ 24m 12s
🏁 Ended early
```

## Features

| Feature | Details |
|---|---|
| Local-context cards | Squares adapt to region, season, setting and difficulty |
| Photo-only input | JPG, PNG and WebP. No audio or video. |
| AI verification | Gemma judges each photo and explains misses |
| Fun facts | One short fact per verified find, shown in the popup |
| Early finish | Optional **End game** button, with its own shareable result |
| Share text | Copy-paste emoji grid with score and total time |

## Safeguards

| Safeguard | Why |
|---|---|
| Photos are downscaled to 768px before sending | Fewer tokens and faster checks |
| Screenshots and photos of screens are rejected | Photos have to be taken outdoors |
| A photo can't be reused for another square | Prevents duplicate cheating |
| Text inside images is ignored by the judge prompt | Guards against prompt injection |
| 60 photo checks per session | Protects a shared API key |
| Fallback card if generation fails | The game still works |

## Tech stack

| Layer | Choice |
|---|---|
| UI | [Streamlit](https://streamlit.io) (light, minimal theme) |
| Model | Gemma (open-weight), default `gemma-3-27b-it` |
| Model access | Google Gemini API through the `google-genai` SDK |
| Image handling | Pillow |
| Hosting | Streamlit Community Cloud |

Gemma on the Gemini API has no system-instruction or JSON mode, so the prompts carry all the rules and the app parses the JSON itself.

## Run locally

```bash
git clone <your-repo-url>
cd nature-bingo
pip install -r requirements.txt
```

Create `.streamlit/secrets.toml`:

```toml
GEMINI_API_KEY = "your-key"
# GEMMA_MODEL = "gemma-3-27b-it"   # optional
```

Then start the app:

```bash
streamlit run app.py
```

You can also set `GEMINI_API_KEY` as an environment variable instead.

## Deploy on Streamlit Community Cloud

| Step | Action |
|---|---|
| 1 | Push this repo to GitHub, with `app.py` and `requirements.txt` at the root |
| 2 | In Streamlit Community Cloud, create a new app pointing at `app.py` |
| 3 | Open **Settings → Secrets** and add `GEMINI_API_KEY = "your-key"` |
| 4 | Optionally add `GEMMA_MODEL` to switch models without changing code |

## Configuration

| Setting | Where | Default |
|---|---|---|
| `GEMINI_API_KEY` | Secrets or env var | required |
| `GEMMA_MODEL` | Secrets or env var | `gemma-4-26b-a4b-it` |
| `MAX_CHECKS_PER_SESSION` | `app.py` | 60 |
| `MAX_IMAGE_SIDE` | `app.py` | 768 |

## Project structure

```
nature-bingo/
├── app.py                  # the whole app
├── requirements.txt
└── .streamlit/
    └── config.toml         # light theme
```

