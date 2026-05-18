# GestureSlide — Assistant Prototype

This workspace contains a demo frontend and a small Flask backend that simulates a local "Jarvis" assistant.

Files:
- `index.html` — main frontend (gesture + voice + assistant panel)
- `app.py` — Flask demo assistant server
- `requirements.txt` — Python dependencies

Run the assistant server (in PowerShell):

```powershell
python -m venv venv
venv\Scripts\Activate.ps1
pip install -r requirements.txt
python app.py
```

Then open the frontend from `http://localhost:8000` (serve the folder) and allow camera & microphone. Use the assistant panel on the right or say "Jarvis, ..." to send the spoken phrase to the backend.

Notes:
- This backend is a safe demo and does not execute system commands. It returns simple canned responses and a Google search URL for `search` commands.
- For production Jarvis-style features you would integrate a secure server and a real NLP/LLM service.
