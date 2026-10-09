# Phase 3 — Gemini AI Setup & Initialization

> Phase 3 of 8 · [Index](README.md) · ← [Phase 2](Phase-2-Architecture-and-Tech-Stack.md) · Next → [Phase 4: Data Layer & Authentication](Phase-4-Data-Layer-and-Authentication.md)

## 1. Objective of this phase
Get authenticated access to the Gemini API, confirm it works for text and image prompts, and prepare the local project environment.

## 2. Activities

### Activity 3.1 — Create a Google AI Studio account and API key
1. Sign in to Google AI Studio (Google account required) and accept the terms.
2. Click **Get API key** → **Create API key**.
3. Copy the key and keep it private. Never commit it to GitHub.

![Google AI Studio landing](images/ai-studio-landing.png)

### Activity 3.2 — Choose the model
In AI Studio's *Run settings*, pick a Gemini Flash model. The project defaults to `gemini-2.5-flash` and reads the model name from `GEMINI_MODEL`, so it can be changed without touching code.

![Model selection in AI Studio](images/ai-studio-model-selection.png)

### Activity 3.3 — Configure environment variables
Copy `env.example` to `.env`:
```env
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-2.5-flash
SECRET_KEY=change-this-to-any-long-random-string
```
Add `.env` and `pocketsmart.db` to `.gitignore`.

### Activity 3.4 — Install dependencies
```bash
python -m venv venv
venv\Scripts\activate          # Windows   (Linux/macOS: source venv/bin/activate)
pip install -r requirements.txt
```
`requirements.txt`: `fastapi`, `uvicorn[standard]`, `jinja2`, `python-multipart`, `python-dotenv`, `PyJWT`, `google-genai`.

### Activity 3.5 — Validate connectivity (text and image)
A minimal check using the same SDK the app uses:
```python
import os
from dotenv import load_dotenv
from google import genai
from google.genai import types

load_dotenv()
client = genai.Client(api_key=os.getenv("GEMINI_API_KEY"))
model = os.getenv("GEMINI_MODEL", "gemini-2.5-flash")

# 1) Text-only
r = client.models.generate_content(model=model, contents="Say hello in one short sentence.")
print(r.text)

# 2) Image + text (multimodal)
with open("sample_outfit.jpg", "rb") as f:
    img = types.Part.from_bytes(data=f.read(), mime_type="image/jpeg")
r = client.models.generate_content(model=model, contents=[img, "Describe the main colours of this outfit."])
print(r.text)
```
If both calls return text, text and multimodal support are confirmed.

## 3. How the app handles a missing or broken key
`gemini_utils.py` creates the client only when `GEMINI_API_KEY` exists. If the SDK is missing, the key is wrong, or any call fails, the app does **not** crash. It switches to offline fallback recommendations (see [Phase 5](Phase-5-AI-Recommendation-Engine.md)).

## 4. Security notes
- Keep the API key only in `.env`
- Rotate the key immediately if it is ever exposed
- Use a long random `SECRET_KEY` (the code default `dev-secret-change-me` is for development only)

---
## 5. Output of Phase 3
**Result:** a working API key, `.env` configured, dependencies installed and a verified text + image connection.

**Visual reference — API Keys page in Google AI Studio:**

![AI Studio API keys page](images/ai-studio-api-keys.png)

> The demo video shows the same setup in VS Code: virtual environment activated and `pip install -r requirements.txt` running.
