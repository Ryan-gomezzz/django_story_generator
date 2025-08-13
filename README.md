# Django AI Story Generator Project

This repository contains a complete Django web application that integrates LangChain, Ollama, Whisper, and local image generation tools to create short stories with descriptive character and background images. The app:

1. Accepts text or audio prompts from users
2. Generates a short story, character description, and background description using LangChain and a local LLM (via Ollama)
3. Builds detailed image prompts for character and background images
4. Generates two images using local image generation models (defaults to FLUX 1 [schnell])
5. Combines the images into a unified scene using Pillow
6. Displays the story, character description, and combined image in the web interface

---

## Quick Start

### 1. Clone and set up the project

```bash
# Clone the repo
 git clone <your-repo-url> django_story_generator
 cd django_story_generator

# Create virtual environment
 python3 -m venv venv
 source venv/bin/activate  # Windows: venv\Scripts\activate

# Install dependencies
 pip install -r requirements.txt

# Copy environment variables template
 cp .env.example .env
# Edit .env to configure your SECRET_KEY and Ollama settings
```

### 2. Install and run Ollama

```bash
# Install Ollama (Linux/macOS)
curl -fsSL https://ollama.com/install.sh | sh

# Pull models
ollama pull llama3.2:latest        # language model
ollama pull llava:13b              # vision model (if needed)
ollama pull flux:schnell           # image model

# Start Ollama server
ollama serve  # Runs on http://localhost:11434 by default
```

> **Tip:** Modify `.env` to point to a remote Ollama server if you run models elsewhere.

### 3. Run database migrations and start Django

```bash
python manage.py migrate
python manage.py runserver
```

Open your browser at `http://127.0.0.1:8000` and start generating stories!

---

## Command-line Cheatsheet (Windows PowerShell)

```powershell
# Create venv
python -m venv .\.venv
.\.venv\Scripts\Activate.ps1

# Install reqs
pip install -r requirements.txt

# Run the dev server
python manage.py migrate
python manage.py runserver
```

---

## Project Structure (highlights)

```
django_story_generator/
├─ manage.py
├─ requirements.txt
├─ .env.example
├─ django_story_generator/       # core Django project
│  ├─ settings.py                # environment‐based settings
│  ├─ urls.py
│  ├─ wsgi.py / asgi.py
│  └─ ...
└─ apps/
   └─ story_generator/           # main app
      ├─ models.py               # StoryGeneration, GenerationStep
      ├─ services.py             # business logic + AI orchestration
      ├─ chains.py               # LangChain pipelines
      ├─ views.py / urls.py
      ├─ templates/
      └─ static/
```

---

## Environment Variables (`.env`)

```
SECRET_KEY=your-secret-key-here
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1

# Ollama API
OLLAMA_BASE_URL=http://localhost:11434
OLLAMA_TEXT_MODEL=llama3.2:latest
OLLAMA_VISION_MODEL=llava:13b
IMAGE_MODEL_CHARACTER=flux:schnell
IMAGE_MODEL_BACKGROUND=flux:schnell
```

---

## CMD / Bash one-liners

Generate and combine images without the web UI (optional helper):

```bash
python - << 'PY'
from apps.story_generator.services import StoryGenerationService
svc = StoryGenerationService()
sg = svc.process_generation(1)  # regenerate record #1
print('Done:', sg)
PY
```

---

### Enjoy your local AI pipeline! ✨
