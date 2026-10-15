# 🎵 Orpheus-FastAPI

A high-performance Text-to-Speech server built on **FastAPI** and the **Orpheus TTS** model — with a clean web UI, an OpenAI-compatible API, and Docker orchestration for CPU, CUDA, and ROCm setups.

> **Custom build maintained by [Vemmba](https://github.com/Vemmba)**

---

## ✨ Features

- ⚡ **FastAPI** backend with async generation
- 🌐 **Web UI** for quick voice previews
- 🔌 **OpenAI-compatible endpoint** — drop-in for existing clients (`/v1/audio/speech`)
- 🐳 **Docker Compose** setups for **CUDA**, **ROCm**, and **CPU-only** machines
- 🗣️ Multiple voices with speed control (WAV output)

---

## 📋 Prerequisites

- Python 3.8–3.11 (3.12 not supported — `pkgutil.ImpImporter` removal)
- A CUDA- or ROCm-compatible GPU (e.g., RTX series) recommended
- Either Docker Compose **or** a separate LLM inference server running the Orpheus model (LM Studio, llama.cpp server, GPUStack…)
- For Docker GPU support: NVIDIA GPU with CUDA 12.4+ and the NVIDIA Container Toolkit

---

## 🐳 Quick Start — Docker

1. Create your environment file:

   ```bash
   cp .env.example .env        # Linux/macOS
   copy .env.example .env      # Windows CMD
   ```

2. *(Optional)* For a multilingual model, edit `.env`:

   ```env
   ORPHEUS_MODEL_NAME=Orpheus-3b-French-FT-Q8_0.gguf
   ```

3. Start the stack:

   ```bash
   docker compose -f docker-compose-gpu.yml up        # CUDA
   docker compose -f docker-compose-gpu-rocm.yml up  # ROCm
   docker compose -f docker-compose-cpu.yaml up      # CPU only
   ```

The GGUF model is downloaded automatically on first start.

---

## 🖥️ Quick Start — Native Install

```bash
# 1. Clone
git clone https://github.com/Vemmba/Orpheus-FastAPI.git
cd Orpheus-FastAPI

# 2. Virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. PyTorch (CUDA)
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
# ...or ROCm:
pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/rocm6.4/

# 4. Dependencies
pip3 install -r requirements.txt

# 5. Required directories
mkdir -p outputs static

# 6. Configure
cp .env.example .env   # then set ORPHEUS_API_URL to your inference server
```

### Run

```bash
python app.py
# or
uvicorn app:app --host 0.0.0.0 --port 5005 --reload
```

| URL | What |
|---|---|
| `http://localhost:5005/` | Web UI |
| `http://localhost:5005/docs` | API docs (Swagger) |

---

## 🔌 API Usage

OpenAI-compatible endpoint:

```bash
curl http://localhost:5005/v1/audio/speech \
  -H "Content-Type: application/json" \
  -d '{
    "model": "orpheus",
    "input": "Hello world! This is a test of the Orpheus TTS system.",
    "voice": "tara",
    "response_format": "wav",
    "speed": 1.0
  }' \
  --output speech.wav
```

**Parameters**

| Parameter | Required | Default | Notes |
|---|---|---|---|
| `input` | ✅ | — | Text to synthesize |
| `model` | ❌ | `orpheus` | Model name |
| `voice` | ❌ | `tara` | Voice selection |
| `response_format` | ❌ | `wav` | Only `wav` currently |
| `speed` | ❌ | `1.0` | Speech rate |

---

## 🗣️ Voices

Voices include `tara`, `leo`, `thomas`, and more — see the Web UI dropdown for the full list with descriptions.

---

## 🔗 Integration

Works great with [OpenWebUI](https://github.com/open-webui/open-webui) as a TTS backend — point it at the `/v1/audio/speech` endpoint.

**Recommended inference servers:**

- [GPUStack](https://github.com/gpustack/gpustack) — GPU-optimized, tensor-split parallelism
- [llama.cpp server](https://github.com/ggerganov/llama.cpp)

---

## 📄 License

Apache License 2.0 — see [LICENSE](LICENSE).

---

<div align="center">

**Maintained by [Vemmba](https://github.com/Vemmba)** · ⭐ the repo if it's useful!

</div>
