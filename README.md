# 🤖 AI Free Agent - Run Claude with Ollama on Google Colab (100% Free!)

> **Run powerful AI models like Claude completely free using Google Colab and Ollama!**

## 🌟 Why This Is Awesome

- ✅ **Completely FREE** - No API costs, no subscriptions
- 🚀 **Powerful Models** - Run advanced models like `gpt-oss:20b` and `qwen2.5-coder:3b`
- ☁️ **Cloud-Based** - Uses Google Colab's free GPU resources
- 🔧 **Easy Setup** - Just follow the steps below!

---

## 📋 Prerequisites

Before you start, you'll need:
1. A Google account (for Google Colab)
2. An Ngrok account (free) - [Sign up here](https://ngrok.com/)

---

## 🔑 Step 1: Get Your Ngrok Auth Token

1. Go to [https://ngrok.com/](https://ngrok.com/) and sign up for a free account
2. After logging in, go to [https://dashboard.ngrok.com/get-started/your-authtoken](https://dashboard.ngrok.com/get-started/your-authtoken)
3. Copy your authtoken - it should look like: `2abc123XYZ_yourTokenHere`
4. Keep this token handy - you'll need it in the next step!

---

## 🚀 Step 2: Open Google Colab

1. Go to [Google Colab](https://colab.research.google.com/)
2. Click **"New Notebook"** to create a fresh notebook
3. You're ready to start!

---

## ⚙️ Step 3: Run the Setup Script

Copy and paste the following code into a code cell in your Colab notebook, **replacing `YOUR_NGROK_TOKEN_HERE` with your actual ngrok authtoken**:

```bash
%%bash
# Update system and install dependencies
apt-get update && apt-get install -y zstd

# Install ngrok
curl -sSL https://ngrok-agent.s3.amazonaws.com/ngrok.asc | tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null
echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | tee /etc/apt/sources.list.d/ngrok.list
apt-get update && apt-get install -y ngrok

# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Configure ngrok with YOUR token
ngrok config add-authtoken YOUR_NGROK_TOKEN_HERE

# Start Ollama server
export OLLAMA_HOST=0.0.0.0
nohup ollama serve > ollama.log 2>&1 &
sleep 5

# Download AI model (choose one or both)
echo "📥 Downloading AI model..."
# Option 1: Lightweight coding model (faster)
# ollama pull qwen2.5-coder:3b

# Option 2: More powerful general model (recommended)
ollama pull gpt-oss:20b

# Start ngrok tunnel
nohup ngrok http 11434 --host-header="localhost:11434" > ngrok.log 2>&1 &
sleep 8

# Display the public URL
echo "---------------------------------------------------"
echo "✅ SERVER IS LIVE!"
echo ""
echo "🌐 OLLAMA PUBLIC URL:"
curl -s http://localhost:4040/api/tunnels | python3 -c "import sys, json; print(json.load(sys.stdin)['tunnels'][0]['public_url'])"
echo ""
echo "---------------------------------------------------"
echo "📋 Copy the URL above - you'll need it in the next step!"
```

**Important:** Make sure to replace `YOUR_NGROK_TOKEN_HERE` with your actual token!

Click the **Run** button (▶️) and wait for the script to complete. This will take 5-10 minutes.

---

## 🔗 Step 4: Get Your Public URL

After the script finishes, you'll see output like:

```
---------------------------------------------------
✅ SERVER IS LIVE!

🌐 OLLAMA PUBLIC URL:
https://abc123-xyz-456.ngrok-free.app

---------------------------------------------------
```

**Copy this URL!** You'll need it for the next step.

---

## 🎯 Step 5: Configure Claude CLI

Now install and configure the Claude CLI on your local machine (or in a new Colab cell):

### Option A: Install Claude CLI (if not installed)

```bash
# Install Claude CLI
pip install claude-cli
```

### Option B: Configure Environment Variables

Run these commands in your terminal, **replacing `YOUR_URL_HERE` with the ngrok URL from Step 4**:

```bash
export ANTHROPIC_AUTH_TOKEN=ollama
export ANTHROPIC_API_KEY=""
export ANTHROPIC_BASE_URL=YOUR_URL_HERE
```

**Example:**
```bash
export ANTHROPIC_AUTH_TOKEN=ollama
export ANTHROPIC_API_KEY=""
export ANTHROPIC_BASE_URL=https://abc123-xyz-456.ngrok-free.app
```

---

## 🎉 Step 6: Run Claude!

Now you can start using Claude with Ollama:

```bash
claude --model gpt-oss:20b
```

That's it! You now have a **free AI agent** running on powerful models! 🎊

---

## 💡 Available Models

You can use different models based on your needs:

| Model | Size | Best For | Pull Command |
|-------|------|----------|--------------|
| `gpt-oss:20b` | 20B params | General tasks, conversations | `ollama pull gpt-oss:20b` |
| `qwen2.5-coder:3b` | 3B params | Coding, lightweight tasks | `ollama pull qwen2.5-coder:3b` |

To switch models, just change the `--model` parameter:
```bash
claude --model qwen2.5-coder:3b
```

---

## 🛠️ Troubleshooting

### Issue: "Connection refused" error
**Solution:** Make sure the Colab notebook is still running and the ngrok URL hasn't expired.

### Issue: "Model not found"
**Solution:** Wait for the model download to complete in Step 3, or manually pull it:
```bash
ollama pull gpt-oss:20b
```

### Issue: Ngrok URL stopped working
**Solution:** Ngrok free tier URLs expire after some time. Just restart the setup script to get a new URL.

### Issue: Colab disconnected
**Solution:** Google Colab free tier disconnects after ~12 hours of inactivity. Simply run the setup script again.

---

## ⚡ Pro Tips

1. **Keep Colab Active:** Interact with your Colab notebook occasionally to prevent disconnection
2. **Save Your Work:** Colab sessions are temporary - save important conversations locally
3. **Use GPU Runtime:** For faster model loading, change Colab runtime to GPU (Runtime → Change runtime type → GPU)
4. **Bookmark Your URL:** Save your ngrok URL somewhere handy while working

---

## 🤝 Contributing

Found this helpful? Give it a ⭐ and share it with others!

Want to improve this guide? Pull requests are welcome!

---

## ⚠️ Disclaimer

This setup uses:
- Google Colab's free tier (subject to their terms of service)
- Ngrok's free tier (subject to their terms of service)
- Ollama (open source)

Make sure you comply with all respective terms of service.

---

## 📚 Additional Resources

- [Ollama Documentation](https://ollama.com/)
- [Ngrok Documentation](https://ngrok.com/docs)
- [Google Colab Guide](https://colab.research.google.com/)
- [Claude CLI Documentation](https://github.com/anthropics/claude-cli)

---

## 🌟 Star This Repo!

If this helped you run AI models for free, please give this repo a star! ⭐

**Happy AI coding! 🚀**

---

*Made with ❤️ for the AI community*
