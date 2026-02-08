# 🤖 Agent AI for Free  
Run Ollama on Google Colab, expose it with ngrok, and use it like an AI agent at **$0 cost**

> ⚠️ READ THIS FIRST
- **Never hardcode secrets** (ngrok tokens, API keys) in public repos.
- Ollama runs **open-source models locally** → no Anthropic / OpenAI billing.
- Google Colab and ngrok may have **usage limits** depending on your plan.
- Claude CLI is designed for Anthropic APIs, so we use a **proxy approach** to make it work with Ollama.

---

## 1️⃣ Open Google Colab

```bash
# 1. Go to https://colab.research.google.com
# 2. Click "New Notebook"
# 3. (Optional) Runtime → Change runtime type → GPU
2️⃣ Install Ollama + ngrok (NO API KEY INSIDE)
Run this entire block in ONE Colab cell:

apt-get update && apt-get install -y zstd curl python3

# -------------------------
# Install ngrok
# -------------------------
curl -sSL https://ngrok-agent.s3.amazonaws.com/ngrok.asc \
  | tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null

echo "deb https://ngrok-agent.s3.amazonaws.com buster main" \
  | tee /etc/apt/sources.list.d/ngrok.list

apt-get update && apt-get install -y ngrok

# -------------------------
# Install Ollama
# -------------------------
curl -fsSL https://ollama.com/install.sh | sh
3️⃣ Get your ngrok authtoken (DO THIS MANUALLY)
# 1. Create / login to ngrok
# 2. Go to Dashboard → Your Authtoken
# 3. Copy it
# 4. Paste it below (DO NOT COMMIT THIS)
ngrok config add-authtoken "<PASTE_YOUR_NGROK_AUTHTOKEN_HERE>"
4️⃣ Start Ollama server
export OLLAMA_HOST=0.0.0.0

nohup ollama serve > ollama.log 2>&1 &
sleep 5
5️⃣ Download a model
⚠️ Big models may crash Colab. Start small if needed.

# Small & fast
# ollama pull qwen2.5-coder:3b

# Bigger & stronger (needs more RAM)
ollama pull gpt-oss:20b
6️⃣ Expose Ollama with ngrok
nohup ngrok http 11434 --host-header="localhost:11434" > ngrok.log 2>&1 &
sleep 8
echo "---------------------------------------------"
echo "✅ OLLAMA SERVER IS LIVE"
echo "PUBLIC URL:"
curl -s http://localhost:4040/api/tunnels \
  | python3 -c "import json,sys; print(json.load(sys.stdin)['tunnels'][0]['public_url'])"
echo "---------------------------------------------"
7️⃣ Test Ollama locally (inside Colab)
curl http://localhost:11434/api/tags
8️⃣ (OPTIONAL) Make Claude CLI work with Ollama (FREE)
Claude CLI expects an Anthropic-style API, so we use LiteLLM as a proxy.

Install LiteLLM
pip -q install litellm
Run LiteLLM proxy (Anthropic-compatible)
nohup litellm \
  --host 0.0.0.0 \
  --port 4000 \
  --model ollama/gpt-oss:20b \
  --api_base http://localhost:11434 \
  > litellm.log 2>&1 &

sleep 5
9️⃣ Expose LiteLLM with ngrok
nohup ngrok http 4000 --host-header="localhost:4000" > ngrok_litellm.log 2>&1 &
sleep 8
echo "LITELLM PUBLIC URL:"
curl -s http://localhost:4040/api/tunnels \
  | python3 -c "import json,sys; print([t['public_url'] for t in json.load(sys.stdin)['tunnels']])"
➡️ Copy the URL that points to port 4000

🔟 Configure Claude CLI environment variables
⚠️ This does NOT give real Anthropic Claude access.
It routes Claude CLI requests to your Ollama model.

export ANTHROPIC_AUTH_TOKEN=ollama
export ANTHROPIC_API_KEY="DUMMY_KEY"
export ANTHROPIC_BASE_URL="https://PASTE_LITELLM_NGROK_URL_HERE"
1️⃣1️⃣ Run Claude CLI (FREE)
claude --model gpt-oss:20b
🎉 You now have an AI Agent running for free, powered by:

Google Colab

Ollama

ngrok

Claude CLI (proxy mode)

⭐ Tips to make this repo popular
# Add a Colab badge
# Add troubleshooting section
# Add security warnings (do NOT expose secrets)
# Add example curl / Python requests
# Recommend models by RAM/GPU
🛠 Troubleshooting
# ngrok issues
tail -n 50 ngrok.log

# Ollama issues
tail -n 50 ollama.log

# LiteLLM issues
tail -n 50 litellm.log
📜 Disclaimer
# You are responsible for exposed endpoints.
# Do not leak tokens.
# Respect Colab & ngrok ToS.
🔥 Agent AI for Free — no subscriptions, no API bills, just compute.
