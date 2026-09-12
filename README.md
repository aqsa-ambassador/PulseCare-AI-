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
- The Heart Rate tab uses `streamlit-webrtc` to access the phone's camera live
  in the browser (works on mobile Chrome/Safari over HTTPS — Streamlit Cloud
  serves HTTPS by default, so this works out of the box).
- Two modes: face (front camera) or fingertip over the back camera + flashlight.
- Blood pressure and blood glucose still require manual entry from a real
  cuff/monitor or glucose meter — a phone camera cannot measure these, on
  mobile or otherwise.
