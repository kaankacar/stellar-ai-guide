# Stellar Development Foundation Hackathon: AI Tools Guide
## Using Open-Source AI Models for Coding

**Prepared for: Stellar AI Guide Brazil**
**Date: March 2026**


## Table of Contents

1. [Introduction](#introduction)
2. [Free AI Coding Alternatives](#free-alternatives)
3. [Running Your Model on a VPS](#vps-guide)
4. [About Stella: Stellar's AI Assistant](#stella)
5. [Practical Open-Source Models for Hackathon Use](#top-models)
6. [How to Use a Local Model with Claude Code](#claude-code-guide)


<a id="introduction"></a>

## 1. Introduction

As a hacker at this event, you have several options when it comes to AI-powered coding assistance:

- **Paid plans**: If you already have a subscription to Claude, ChatGPT, GitHub Copilot, or another AI service, you can continue using those normally.
- **Open-source local models**: You can download a free, open-source AI model and run it on your own computer, with no subscription required and no data leaving your machine.
- **Cloud VPS**: If your laptop doesn't have enough power to run a large model, you can rent a GPU server for a few dollars and run the model there.
- **Free tiers**: Tools like Cursor offer free plans with access to capable AI models.

This guide focuses on the practical path: use free cloud tools first, use a VPS if you need heavier compute, and only run local models when your laptop can realistically handle them.


<a id="free-alternatives"></a>

## 2. Free AI Coding Alternatives

If you don't want to set up a local model or rent a GPU VPS, you can still build with AI for free by combining free API tiers, model gateways, cloud notebooks, and startup credit programs. Treat these as a stack:

1. Use **Stella** for Stellar-specific questions.
2. Use **Cursor**, **GitHub Copilot Free**, or **Google AI Studio** when you need a tool immediately.
3. Use **Groq**, **Cerebras**, **OpenRouter**, **NVIDIA NIM**, or **xAI Grok** when you need API access.
4. Use **Hugging Face Spaces**, **Kaggle**, **Colab**, or **Lightning AI** when you need free hosted compute.
5. Use **AWS Activate**, **Google for Startups**, **Microsoft Founders Hub**, or AI startup accelerators if your project becomes a real startup.

> Important: Free AI limits change often. Before relying on any provider for a demo, check the provider's current dashboard limits and keep a fallback model ready.


### 2a. Best no-card options for a hackathon

These are the fastest ways to get usable AI access without setting up local inference.

| Need | Best option | Why | Signup |
|---|---|---|---|
| Best quick coding chat | Cursor free tier | Built into a VS Code-like editor | https://cursor.com |
| Best Stellar-specific help | Stella | Trained on Stellar docs and ecosystem material | https://developers.stellar.org |
| Best long-context browser tool | Google AI Studio | Gemini models are strong for large pasted files | https://aistudio.google.com |
| Best low-latency API | Groq | Very fast inference for chat and agent loops | https://console.groq.com |
| Best free model gateway | OpenRouter | One API for many free models with `:free` IDs | https://openrouter.ai |
| Best high-throughput API | Cerebras | Strong for batch generation and fast token output | https://cloud.cerebras.ai |
| Best NVIDIA-hosted sandbox | NVIDIA NIM | OpenAI-compatible API for NVIDIA-hosted models | https://build.nvidia.com |
| Best model demo hosting | Hugging Face Spaces | Free community hosting for Gradio/Streamlit demos | https://huggingface.co/spaces |


### 2b. OpenRouter: one API for many free models

OpenRouter is useful because it gives you one API key and one endpoint for many providers. Free models usually have `:free` at the end of the model ID.

**Claude Code config:**
```bash
export ANTHROPIC_BASE_URL="https://openrouter.ai/api/v1"
export ANTHROPIC_API_KEY="your-openrouter-key"
claude --model nvidia/nemotron-3-super-120b-a12b:free
```

Good free-model categories to try:

| Category | Try this when | Example model pattern |
|---|---|---|
| Large coding model | You need code edits, debugging, or architecture help | Qwen Coder or Nemotron coding models with `:free` |
| Reasoning model | You need planning, math, or multi-step debugging | DeepSeek R1-style models with `:free` |
| General chat | You need explanations and project guidance | Llama 3.3 70B-style models with `:free` |
| Multimodal | You need image or screenshot analysis | Qwen VL-style models with `:free` |

Browse the current free list at https://openrouter.ai/models?q=free.

**When OpenRouter is a good fit:**
- Testing several models quickly
- Switching models if one free model is rate-limited
- Using a single API key in demos
- Routing Claude Code or other OpenAI-compatible clients to free hosted models

**When not to use it:**
- Sensitive code or private keys
- Hard production SLAs
- Anything that cannot tolerate free-tier rate limits


### 2c. Groq and Cerebras: fastest free inference

Groq and Cerebras are specialized inference providers. They are especially useful for hackathon apps because they respond quickly and expose OpenAI-compatible APIs.

| Provider | Best for | Notes | Signup |
|---|---|---|---|
| **Groq** | Low-latency chat, agents, repeated prompts | Very fast responses; useful for interactive demos | https://console.groq.com |
| **Cerebras** | High-throughput generation and batch tasks | Strong token throughput; useful for synthetic data and long generations | https://cloud.cerebras.ai |
| **SambaNova** | Large open-weight reasoning models | Often offers trial credits for larger models | https://cloud.sambanova.ai |

**Claude Code config for Groq:**
```bash
export ANTHROPIC_BASE_URL="https://api.groq.com/openai/v1"
export ANTHROPIC_API_KEY="your-groq-key"
claude --model llama-3.3-70b-versatile
```

Use Groq when your app needs a fast response for every user interaction. Use Cerebras when you need to generate a lot of text quickly, such as test data, summaries, or batch analysis.


### 2d. Google AI Studio: free long-context Gemini access

Google AI Studio is one of the easiest free tools for large-codebase analysis. It is especially helpful when you want to paste a contract, frontend file, backend route, logs, and an error message into one prompt.

**Claude Code config for Gemini's OpenAI-compatible endpoint:**
```bash
export ANTHROPIC_BASE_URL="https://generativelanguage.googleapis.com/v1beta/openai/"
export ANTHROPIC_API_KEY="your-gemini-key"
claude --model gemini-2.0-flash
```

Good uses:
- Explaining large code files
- Reviewing API design
- Summarizing docs
- Creating implementation plans
- Debugging logs with lots of context

Avoid using the free tier for secrets, customer data, or private production code unless your team has reviewed the data policy.


### 2e. NVIDIA NIM: free NVIDIA-hosted model APIs

NVIDIA NIM provides hosted inference microservices through https://build.nvidia.com. It is useful when you want to test models deployed on NVIDIA infrastructure without setting up your own GPU server.

**Why builders use it:**
- OpenAI-compatible API
- Hosted models from NVIDIA and partner ecosystems
- Good path from cloud prototyping to later self-hosting with NIM containers
- Useful for agent and tool-use experiments

**OpenAI-compatible configuration pattern:**
```bash
export OPENAI_BASE_URL="https://integrate.api.nvidia.com/v1"
export OPENAI_API_KEY="your-nvidia-api-key"
```

Check the current free credit amount and model catalog in the NVIDIA dashboard before the event. NIM credits and model availability can change.


### 2f. xAI Grok credits: useful, but understand the tradeoff

xAI's developer console is at https://console.x.ai. The Grok API can be useful for reasoning-heavy demos and products that benefit from Grok's ecosystem integrations.

Some xAI credit programs involve a tradeoff: you may receive recurring credits in exchange for allowing API inputs and outputs to be used for model improvement. That can be fine for non-sensitive prototypes, but it is not appropriate for private code, customer data, wallet secrets, API keys, legal documents, or unreleased product strategy.

Use Grok credits for:
- Public-data summarization
- News or social-content analysis
- Non-sensitive reasoning features
- Demo workloads where training-data reuse is acceptable

Do not use Grok free-credit data sharing for:
- Proprietary source code
- User PII
- Financial data
- Private keys, seed phrases, or wallet credentials
- Anything your team would not want in a provider training pipeline


### 2g. Hugging Face: models, demos, and free community hosting

Hugging Face is not just a model download site. For hackathons, it gives you three useful free paths:

| Tool | Use it for | Notes |
|---|---|---|
| **Model Hub** | Finding open-source models | Best place to compare model cards, licenses, and examples |
| **Spaces** | Hosting a Gradio or Streamlit demo | Good for public demos without managing servers |
| **Serverless Inference API** | Testing smaller community models | Can have cold starts and variable latency |

Use Hugging Face Spaces when your team wants a live demo URL quickly. Use the Model Hub when checking whether a model's license allows commercial or hackathon use.


### 2h. Free GPU notebooks and sandboxes

If you need a GPU but do not need a permanent server, use a notebook or cloud sandbox.

| Platform | Best for | Notes |
|---|---|---|
| **Google Colab** | Simple notebooks and quick experiments | Sessions are temporary; connect Google Drive for files |
| **Kaggle Notebooks** | Dataset work, ML experiments, TPU/GPU access | Good free compute for notebook workflows |
| **Lightning AI Studios** | More complete cloud dev environments | Useful when available because it feels closer to a full VM |
| **Hugging Face Spaces ZeroGPU** | Public AI demos | Good for Gradio/Streamlit apps that need occasional GPU access |

Use these for model experiments, fine-tuning tests, embeddings, dataset cleaning, and demos. For running a full backend reliably during judging, a VPS or managed API is usually easier.


### 2i. Free trial credits and startup programs

If your hackathon project turns into a real startup, apply for startup credits. These are often much more valuable than public free tiers.

| Program | Best for | Notes |
|---|---|---|
| **AWS Activate** | Bedrock, SageMaker, general cloud infrastructure | Higher tiers usually require partner, accelerator, or investor affiliation |
| **Google for Startups Cloud Program** | Vertex AI, Gemini, Firebase, Google Cloud | Strong fit for AI-native and Firebase-backed apps |
| **Microsoft Founders Hub** | Azure, Azure OpenAI, GitHub tooling | Lower-friction entry for early-stage startups |
| **Together AI Startup Accelerator** | Open model inference and fine-tuning | Useful for AI-native products using open models |

For a hackathon, do not depend on startup credits being approved immediately. Use public free tiers for the weekend, then apply for startup programs after you have a working prototype and a clear project description.


### 2j. Free IDE tools (no API key needed)

### Cursor (https://cursor.com)

Cursor is a code editor (based on VS Code) with built-in AI coding assistance.

**Free plan includes:**
- 2,000 code completions per month
- Limited access to GPT-4o and Claude Sonnet
- Unlimited access to cursor-small (a fast, lightweight model)

**How to get started:**
1. Download Cursor from https://cursor.com
2. Install it like any application
3. Open your project folder
4. Use `Ctrl+K` (or `Cmd+K` on Mac) to ask AI to write or edit code
5. Use `Ctrl+L` to open the AI chat sidebar


### GitHub Copilot Free Tier (https://github.com/features/copilot)

GitHub Copilot now has a free tier:
- 2,000 code completions per month
- 50 chat messages per month
- Powered by Claude Sonnet and GPT-4o

**How to get started:**
1. Go to GitHub Settings > Copilot
2. Enable the free plan
3. Install the Copilot extension in VS Code
4. Start coding; suggestions appear automatically


### Google AI Studio (https://aistudio.google.com)

- Free access to Gemini 2.5 Flash (1M context window)
- Great for pasting large amounts of code and getting explanations
- No subscription required, just a Google account
- Check the Google AI Studio dashboard for the current free request and token limits


<a id="vps-guide"></a>

## 3. Running Your Model on a VPS

If your laptop doesn't have enough GPU power to run a capable model locally, you can rent a cloud server (VPS) with a GPU for just a few dollars and run the model there. You then connect to it from your laptop as if it were running locally.


### Recommended VPS Providers for LLMs

| Provider | Starting Price | Best For | URL |
|----------|---------------|----------|-----|
| RunPod | ~$0.20/hr (RTX 4090) | Best value for hackers | https://runpod.io |
| Vast.ai | ~$0.15/hr | Cheapest option, peer marketplace | https://vast.ai |
| Lambda Labs | ~$1.10/hr (A100) | Reliable, easy to use | https://lambdalabs.com |
| Paperspace | ~$0.45/hr (A4000) | Good free tier available | https://paperspace.com |
| Hostinger VPS | From $5/mo (CPU) | Budget CPU-only option | https://hostinger.com |
| Contabo VPS | From $5/mo (CPU) | Very affordable CPU servers | https://contabo.com |

> For a hackathon, **RunPod** or **Vast.ai** are recommended because you only pay for the time you use (hourly billing).


### Step-by-Step: Setting Up a RunPod GPU Server

**Step 1: Create an account**

Go to https://runpod.io and sign up. Add a payment method (credit card or crypto accepted). You only need ~$10-20 for a full hackathon weekend.

**Step 2: Create a new Pod**

1. Click "Pods" in the left sidebar
2. Click "+ Deploy"
3. Select a GPU:
   - **RTX 4090 (24 GB VRAM)**: best value, runs 7B-32B models
   - **RTX 3090 (24 GB VRAM)**: slightly cheaper
   - **A100 (80 GB VRAM)**: for 70B models, more expensive
4. For the template/container, select **"RunPod PyTorch"** or search for "Ollama"
5. Set disk storage to at least **50 GB** (models are large files)
6. Click "Deploy"

**Step 3: Connect to your server**

Once the pod is running, click "Connect" and choose "SSH Terminal". You'll get a command like:

```bash
ssh root@<your-pod-ip> -p <port> -i ~/.ssh/id_rsa
```

Run this in your local terminal.

**Step 4: Install Ollama on the server**

```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama serve &
```

**Step 5: Download your model**

```bash
# Coding-agent option
ollama pull devstral

# Reasoning option
ollama pull gpt-oss:20b
```

**Step 6: Expose Ollama to your local machine**

On your VPS, Ollama runs on port 11434. Forward this port to your local machine using SSH tunneling:

```bash
# Run this on your LOCAL machine
ssh -L 11434:localhost:11434 root@<your-pod-ip> -p <port>
```

Keep this terminal open. Now your local Claude Code will see the VPS model as if it were running on your laptop.

**Step 7: Connect Claude Code (same as local setup)**

```bash
export ANTHROPIC_BASE_URL="http://localhost:11434"
export ANTHROPIC_AUTH_TOKEN="ollama"
export ANTHROPIC_API_KEY=""
claude --model devstral
```

**Step 8: Stop your pod when done**

To avoid charges, go to the RunPod dashboard and stop or terminate your pod when you're not using it.


### Step-by-Step: Setting Up on Vast.ai (Budget Option)

**Step 1:** Sign up at https://vast.ai and add credit ($5-10 is enough for a weekend)

**Step 2:** Go to "Search" and filter by:
- GPU: RTX 3090 or RTX 4090
- VRAM: at least 16 GB
- Sort by: Price (cheapest first)

**Step 3:** Click "Rent" on your chosen machine and select the **"PyTorch"** image

**Step 4:** Once running, open the SSH connection and follow the same Ollama installation steps as above (Steps 4-7 from RunPod guide)


### Which GPU to Choose?

| GPU | VRAM | Models it can run | Approx. Cost |
|-----|------|-------------------|-------------|
| RTX 3060 / 4060 | 12 GB | Up to 7B (quantized) | ~$0.10-0.20/hr |
| RTX 3090 / 4090 | 24 GB | Up to 32B (quantized) | ~$0.20-0.50/hr |
| A100 40 GB | 40 GB | Up to 70B (quantized) | ~$1.00-1.50/hr |
| A100 80 GB | 80 GB | 70B+ full precision | ~$1.50-2.50/hr |
| H100 | 80 GB | Largest models | ~$2.50-4.00/hr |


<a id="stella"></a>

## 4. About Stella: Stellar's AI Assistant

**What is Stella?**

Stella is the official AI assistant developed by the Stellar Development Foundation (SDF) and available on the Stellar developer documentation website at https://developers.stellar.org.

Stella is specifically designed to help developers who are building on the Stellar blockchain network. It is currently in beta and was created to make Stellar development faster and more accessible, especially for developers who are new to the ecosystem.

**What can Stella do?**

- Answer questions about the Stellar network, protocols, and SDKs
- Help you understand Stellar Consensus Protocol (SCP)
- Explain Soroban smart contracts (Stellar's smart contract platform built on Rust and WebAssembly)
- Guide you through the Stellar documentation
- Answer questions about Stellar assets, accounts, transactions, and fees
- Help debug Stellar-specific code and API calls

**Where to find Stella:**

- Look for the yellow chat icon at the bottom-right corner of https://developers.stellar.org
- Join the **#stella-help** channel in the Stellar Developer Discord server

**What Stella knows:**

Stella draws from a curated knowledge base that includes:
- Stellar's official documentation and developer guides
- Stack Overflow answers related to Stellar
- The SCF (Stellar Community Fund) Handbook
- YouTube tutorials and educational content
- GitHub repositories from the Stellar ecosystem

**How to use Stella at the Hackathon:**

During the hackathon, use Stella when you have Stellar-specific questions. It's your first stop for:
- "How do I create a Soroban smart contract?"
- "What is the difference between Stellar assets and Soroban tokens?"
- "How do I set up a testnet account?"
- "How do I interact with the Stellar network using JavaScript?"

For general programming questions, use one of the open-source models described in this guide or a free tool like Cursor or Google AI Studio.

<a id="top-models"></a>

## 5. Practical Open-Source Models for Hackathon Use

Hugging Face (https://huggingface.co) is the central hub for open-source and open-weight AI models. This list is focused on models that coding tools and agentic coding workflows actually use or track. Most of these are **not normal laptop downloads**. Treat frontier-size MoE models as hosted/VPS models unless you already know your hardware can run them.

### Recommended Order

| Rank | Model family | Best use | Practical access |
|---|---|---|---|
| 1 | Qwen3.5 / Qwen3-Coder-Next | Agentic coding, codebase edits, tool use | Hosted or smaller/GGUF local builds |
| 2 | DeepSeek V4 | Hard reasoning, coding, long-context debugging | Hosted/VPS |
| 3 | Kimi K2.5 | Agentic coding, multimodal/front-end work, long context | Hosted/VPS |
| 4 | GLM-4.6 | Coding agents, polished frontend generation, long context | Hosted/VPS |
| 5 | MiniMax M2.5 | SWE-bench-style coding, agentic tool use, full-stack tasks | Hosted/VPS |
| 6 | Devstral | Local/near-local coding agent workflows | Realistic high-end local option |
| 7 | gpt-oss | Open-weight reasoning and agentic tasks | 20B local-ish; 120B VPS/cloud |

> Rule of thumb: if a model is 30B+ or MoE/frontier-sized, treat it as a cloud/VPS model unless you already know your machine can run it.


### Model 1: Qwen3.5 / Qwen3-Coder-Next (Qwen Team)

**What changed:**
Qwen's current generation is Qwen3.5, and the current coding-focused branch to watch is Qwen3-Coder-Next. The headline Qwen models are very large, so do not blindly download the biggest one. Look for smaller or quantized variants if you are running locally.

**Use it for:**
- Code generation and refactoring
- Multi-file debugging
- Agentic coding workflows
- TypeScript, Python, Rust, and smart-contract-adjacent work

**Practical choices:**
| Model style | Best For | Notes |
|---|---|---|
| Qwen3.5 4B/9B or GGUF variants | Local laptops | Better practical target than giant models |
| Qwen3-Coder-Next / Qwen3-Coder GGUF | Coding-focused local/VPS option | Use quantized builds if available |
| Qwen3.5 122B+ / 397B+ | Frontier-size Qwen models | Hosted/cloud only for most teams |

**References:**
- https://huggingface.co/Qwen
- https://huggingface.co/Qwen/Qwen3.5-122B-A10B-FP8
- https://huggingface.co/Qwen/Qwen3-Coder-Next-FP8


### Model 2: DeepSeek V4 (DeepSeek AI)

**What changed:**
DeepSeek V4 is the current generation to track, with Flash and Pro variants. It is strong, but it is not a good default laptop recommendation; treat it as a hosted or VPS model unless you have serious hardware.

**Use it for:**
- Algorithmic coding tasks
- Hard debugging sessions
- Large-context code reasoning
- Cloud/VPS experiments when free/local models are not enough

**Practical choices:**
| Model | Notes |
|---|---|
| DeepSeek V4 Flash | Faster/cheaper hosted option |
| DeepSeek V4 Pro | Stronger hosted option |

**Reference:** https://huggingface.co/docs/transformers/main/model_doc/deepseek_v4


### Model 3: Kimi K2.5 (Moonshot AI)

**Why coders care:**
Kimi K2.5 is a frontier-scale open model that has become important in coding-agent discussions. It is strong at agentic coding, long-context work, multimodal input, and front-end tasks.

**Use it for:**
- Full-stack app generation
- Multi-file refactors
- UI work that benefits from image/screenshot context
- Long-context agentic workflows

**Practical note:**
Use it through a hosted provider. It is not a practical laptop model.

**Reference:** https://huggingface.co/moonshotai/Kimi-K2.5


### Model 4: GLM-4.6 (Z.ai)

**Why coders care:**
GLM-4.6 is explicitly positioned for real-world coding, long-context processing, reasoning, and agentic applications. Z.ai's docs call out usage in Claude Code, Cline, Roo Code, and Kilo Code-style workflows.

**Use it for:**
- Polished frontend generation
- Long-context codebase work
- Tool-using coding agents
- Writing plus code tasks in the same project

**Practical note:**
Use the hosted API or a GPU server. This is not a default local model for hackathon laptops.

**References:**
- https://docs.z.ai/guides/llm/glm-4.6
- https://huggingface.co/zai-org/GLM-4.6


### Model 5: MiniMax M2.5

**Why coders care:**
MiniMax M2.5 is trained for complex real-world environments and agentic tool use. Its model card emphasizes coding, search, office work, and full development lifecycle tasks rather than only autocomplete.

**Use it for:**
- SWE-bench-style bug fixing
- Full-stack project iteration
- Code review and test generation
- Agentic workflows where speed matters

**Practical note:**
Use hosted inference unless you have a serious GPU setup.

**Reference:** https://huggingface.co/MiniMaxAI/MiniMax-M2.5


### Model 6: Devstral (Mistral AI + All Hands AI)

**Why coders care:**
Devstral is designed specifically for software engineering agents. It is one of the more realistic options for people who want to run an agentic coding model locally or on a single rented GPU.

**Use it for:**
- Exploring codebases with tools
- Editing multiple files
- OpenHands-style coding agents
- Local or semi-local coding workflows

**Practical note:**
Devstral is still heavy compared with tiny local chat models, but the official model card says it can run on a single RTX 4090 or a Mac with 32GB RAM. It also supports Ollama:

```bash
ollama run devstral
```

**Reference:** https://huggingface.co/mistralai/Devstral-Small-2505


### Model 7: gpt-oss (OpenAI open-weight)

**Why coders care:**
`gpt-oss-20b` and `gpt-oss-120b` are open-weight reasoning models with tool-use and agentic capabilities. They are not code-only models, but they are useful for debugging, planning, and agent workflows.

**Use it for:**
- Reasoning-heavy debugging
- Architecture planning
- Agentic tool use
- Private deployments where you control the infrastructure

**Practical choices:**
| Model | Notes |
|---|---|
| gpt-oss-20b | More realistic local/VPS option |
| gpt-oss-120b | Single 80GB GPU class; use cloud/VPS |

**Reference:** https://huggingface.co/openai/gpt-oss-120b


### Quick Comparison Summary

| Model | First choice for | Practical recommendation |
|---|---|---|
| Qwen3.5 / Qwen3-Coder-Next | Coding agents and multi-file edits | Small/GGUF local builds or hosted |
| DeepSeek V4 | Hard coding/reasoning | VPS/cloud for most teams |
| Kimi K2.5 | Agentic coding and multimodal UI work | Hosted/VPS |
| GLM-4.6 | Frontend polish and long-context agents | Hosted/VPS |
| MiniMax M2.5 | SWE-bench-style full-stack work | Hosted/VPS |
| Devstral | Local-ish coding agents | High-end laptop/GPU or VPS |
| gpt-oss | Open-weight reasoning | 20B local-ish, 120B cloud |


<a id="claude-code-guide"></a>

## 6. How to Use a Local Model with Claude Code

Claude Code is the AI coding assistant you can run in your terminal. By default it uses Anthropic's Claude models in the cloud, but it can be pointed to a locally running open-source model instead, meaning you can use Claude Code completely free with no API credits required.

This works through **Ollama**, a tool that runs AI models locally and exposes them through an API that Claude Code can talk to.


### Step 1: Install Ollama

Ollama is available for macOS, Linux, and Windows.

**macOS:**
```bash
brew install ollama
```

Or download the installer from: https://ollama.com/download

**Linux:**
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

**Windows:**
Download the installer from: https://ollama.com/download/windows


### Step 2: Start the Ollama Server

```bash
ollama serve
```

This starts Ollama in the background, listening on port 11434. Keep this terminal window open, or run it as a background service.


### Step 3: Pull Your Chosen Model

Download the model you want to use. For most hackers, local models are optional, not the default path. If your machine has enough RAM/VRAM, start with Devstral for coding-agent work or `gpt-oss:20b` for reasoning. If those are too slow, use the free cloud options in Section 2 instead of losing time tuning local inference.

```bash
# Coding-agent option
ollama pull devstral

# Reasoning option
ollama pull gpt-oss:20b
```

For Qwen3.5, Qwen3-Coder-Next, GLM-4.6, Kimi K2.5, MiniMax M2.5, or other giant MoE models, prefer hosted inference or a rented GPU server. Only use local GGUF builds if you already know how to run them with Ollama, LM Studio, or llama.cpp.

You can check what models you have downloaded:
```bash
ollama list
```


### Step 4: Install Claude Code

If you haven't already installed Claude Code:

```bash
npm install -g @anthropic-ai/claude-code
```


### Step 5: Connect Claude Code to Your Local Model

Set the following environment variables to tell Claude Code to use your local Ollama model instead of Anthropic's servers:

**macOS / Linux (Terminal):**
```bash
export ANTHROPIC_BASE_URL="http://localhost:11434"
export ANTHROPIC_AUTH_TOKEN="ollama"
export ANTHROPIC_API_KEY=""
```

**Windows (Command Prompt):**
```cmd
set ANTHROPIC_BASE_URL=http://localhost:11434
set ANTHROPIC_AUTH_TOKEN=ollama
set ANTHROPIC_API_KEY=
```

**Windows (PowerShell):**
```powershell
$env:ANTHROPIC_BASE_URL = "http://localhost:11434"
$env:ANTHROPIC_AUTH_TOKEN = "ollama"
$env:ANTHROPIC_API_KEY = ""
```


### Step 6: Launch Claude Code with Your Local Model

```bash
# Launch Claude Code pointing to your local Ollama model
claude --model devstral

# Or, for reasoning-heavy debugging:
claude --model gpt-oss:20b
```

You can also use the quick launch command that Ollama provides:
```bash
ollama launch claude
```


### Step 7: Increase Context Window (Important!)

Claude Code works best with a large context window. Ollama defaults may be too small. Set it to at least 64,000 tokens:

```bash
# Set context window when running a model
OLLAMA_NUM_CTX=64000 ollama run devstral
```

Or add to your Ollama model configuration permanently:
```bash
# Create a custom Modelfile
cat > Modelfile << 'EOF'
FROM devstral
PARAMETER num_ctx 65536
EOF

ollama create my-coder -f Modelfile
claude --model my-coder
```


### Step 8: Make It Permanent (Optional)

To avoid setting environment variables every session, add them to your shell profile:

**macOS / Linux** - add to `~/.zshrc` or `~/.bashrc`:
```bash
export ANTHROPIC_BASE_URL="http://localhost:11434"
export ANTHROPIC_AUTH_TOKEN="ollama"
export ANTHROPIC_API_KEY=""
```

Then reload:
```bash
source ~/.zshrc
```


### Tips for Best Results with Local Models

- **Do not start local unless your hardware is ready.** If Devstral or `gpt-oss:20b` feels slow, switch to OpenRouter, Groq, Cerebras, Google AI Studio, or a VPS.
- **Quantized models (Q4, Q8)** use much less memory with only a small drop in quality. Prefer GGUF/quantized builds for local use.
- **If tool calls fail**, use a hosted model through an OpenAI-compatible endpoint instead of spending the hackathon debugging local inference.
- **Close other heavy applications** while running local models to free up RAM and VRAM.



## Summary: Your AI Toolkit for the Hackathon

| Situation | Recommended Tool |
|-----------|-----------------|
| Stellar-specific questions | Stella (developers.stellar.org) |
| Best free model gateway | OpenRouter free models |
| Fastest free inference | Groq or Cerebras |
| Best long-context free tool | Google AI Studio |
| Best hosted NVIDIA sandbox | NVIDIA NIM |
| High-end laptop or rented GPU | Devstral or gpt-oss:20b via Ollama + Claude Code |
| Normal laptop | Use free cloud tools instead of local inference |
| Want a stronger coding model | Use Qwen3.5, Qwen3-Coder-Next, Kimi K2.5, GLM-4.6, MiniMax M2.5, or DeepSeek V4 through a hosted provider |
| Just want something that works now | Cursor free tier or Google AI Studio |
| Using Claude Code for free (local) | Devstral or gpt-oss:20b via Ollama |
| Using Claude Code for free (cloud) | OpenRouter, Groq, or Google AI Studio |


*Document prepared by the Stellar Development Foundation for hackathon participants.*
*Open-source models are free to download and use. Always check individual model licenses for commercial use restrictions.*
