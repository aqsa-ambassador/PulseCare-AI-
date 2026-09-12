# PulseCare AI — GitHub → Streamlit Deploy Guide

## 1. Files
Push `app.py` and `requirements.txt` to a GitHub repo (public or private).

## 2. Deploy on Streamlit Community Cloud
1. Go to https://share.streamlit.io → **New app**
2. Pick your GitHub repo, branch, and set main file path: `app.py`
3. Click **Deploy**

## 3. Groq API key (OPTIONAL — not required for heart rate detection)
Heart rate, BP status, and glucose status all work **without** any API key —
they use plain signal processing / rule-based ranges, not an LLM.

The Groq key only powers one extra, optional feature: a short AI-written
"wellness note" sentence shown after each check. If you don't add a key,
the app just skips that note — everything else still works.

To enable it:
1. Get a free key at https://console.groq.com
2. In Streamlit Cloud: your app → **Settings → Secrets**, add:
   ```
   GROQ_API_KEY = "your-key-here"
   ```
3. Redeploy/reboot the app.

## 4. Mobile camera notes
- The Heart Rate tab accesses the phone's camera using plain browser
  JavaScript (`getUserMedia` + canvas) embedded via `st.components.v1.html`.
  There is **no Python video library involved** (no streamlit-webrtc, no av,
  no opencv) — the camera capture, timing, and pulse math all run directly
  in the browser, so there's nothing heavy to build on the server.
- Two modes: face (front camera) or fingertip over the back camera + flashlight.
- Blood pressure and blood glucose still require manual entry from a real
  cuff/monitor or glucose meter — a phone camera cannot measure these, on
  mobile or otherwise.

## 5. Why the earlier version failed to deploy
The first version used `streamlit-webrtc`, which depends on the `av` package.
`av` has no pre-built wheel for the Python version Streamlit Cloud was using,
so it tried to build from source and needed `pkg-config` + FFmpeg dev
libraries that aren't available in that environment — causing the
"pkg-config is required for building PyAV" error. Switching to a pure
browser-JS widget removes that dependency entirely, so this version has
nothing to compile.
