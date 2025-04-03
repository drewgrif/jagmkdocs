---
tags:
    - software
    - homelab
    - chat
    - ai
    - gpt
---
# 🧠 Chat

## 🗂️ AI Setup Timeline on TrueNAS SCALE (LLaMA + Open WebUI + Cloud APIs)

### 🖥️ **1. System Overview**
- Running on **TrueNAS SCALE**
- Hardware:
  - 🧠 **AMD Ryzen 5 PRO 4650G (6C/12T)**
  - 💾 **32GB RAM**
  - 🧩 **GIGABYTE B550I AORUS PRO AX motherboard**
- No discrete GPU (using integrated Vega graphics)
- AI services hosted via app containers on TrueNAS

---

### 🧠 **2. Install Local LLM Runtime (Ollama)**
- **Ollama** was used to run local language models like:
  - `phi4-mini`, `gemma:4b`, `mistral`, `llama2`
- The API became accessible at:  
  `http://localhost:30068`  
  (after deploying the app on TrueNAS SCALE)

**Setup Notes:**
- Ollama on TrueNAS SCALE wasn’t plug-and-play — I had to open the **app logs** to find the **public API key** required to connect it to Open WebUI.
- I allocated **6 CPUs** and **20GB RAM** to the app container.

**Performance Reality:**
- While models like `mistral` and `llama2` technically ran, performance was underwhelming:
  - Some responses took **25+ seconds** or didn’t complete
  - Usage was CPU-bound, even with aggressive RAM allocation

**Final Local Model Choices (based on speed + practicality):**
- ✅ **phi4-mini** — Reasonably fast, smart, low memory usage
- ✅ **gemma:4b** — Also fast and stable for general use

These are now my go-to **free models** for everyday tasks.  Gemma is more verbose, but I like it.

---

### 🌐 **3. Install Open WebUI**
- **Open WebUI** is a sleek browser-based interface for chatting with AI models.
- It connects directly to your Ollama backend and offers:
  - ✅ Multi-user support (great for sharing with family)
  - 🕓 Chat history and per-user settings
  - 🔀 Easy model switching between local and API models
- Hosted directly on the **TrueNAS SCALE** box.

**Note:**  
I was able to add family members using group permissions, and I limited which models each could access — great for keeping things simple and budget-conscious.

---

### ☁️ **4. Add Cloud API Models**
- Connected Open WebUI to **OpenAI** (GPT-4o, GPT-3.5) and **Anthropic** (Claude 3.5/3.7) by adding API keys.
- Cloud models are only used when local ones aren’t enough — keeping costs in check.

#### 🔐 API Setup Notes:
- 🧠 **OpenAI**:
  - Added **$5** to my OpenAI account and retrieved an API key from [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
  - Important: **Check the pricing page first** to avoid surprises

- 🤖 **Anthropic (Claude)**:
  - Added **$15** via [Anthropic Console](https://console.anthropic.com/dashboard)
  - Integration required a **custom provider pipeline** to get Claude working:
    [anthropic_manifold_pipeline.py](https://raw.githubusercontent.com/justinh-rahb/open-webui-pipelines/6cdac4f4d1947e4bcf1e96a89d4989860871ccfb/examples/pipelines/providers/anthropic_manifold_pipeline.py)

---

### ✅ Chosen Cloud Models (Based on Cost-Effectiveness):

| Model                       | Why I Chose It                           |
|----------------------------|------------------------------------------|
| `anthropic/claude-3.7-sonnet` | Great quality, lower cost than GPT-4     |
| `gpt-4o-mini`              | Smart & responsive at a good price       |
| `anthropic/claude-3.5-haiku` | Super cheap and fast for general tasks   |
| `o1-mini`                  | Lightweight experimental model           |
| `gpt-3.5-turbo`            | Cheap fallback with decent capability    |
