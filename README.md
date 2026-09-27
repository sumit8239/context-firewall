# 🛡️ Cognitive Context Firewall

> **Real-time stream-level context pruning for AI agents and coding assistants.**  
> Cut multi-turn LLM token consumption and API costs by up to **91.5%** with zero prompt rewriting, zero model migration, and single-digit millisecond latency.

[![Protocol](https://img.shields.io/badge/Protocol-Anthropic%20%7C%20OpenAI-blue)](https://crazy-project-m302.onrender.com)
[![Streaming](https://img.shields.io/badge/Streaming-SSE%20Native-emerald)](https://crazy-project-m302.onrender.com)
[![Status](https://img.shields.io/badge/Status-Public%20Prototype%20Beta-orange)](https://crazy-project-m302.onrender.com)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

---

## ⚡ The Breakthrough: 91.5% Token Reduction

In multi-turn agent workflows (such as **Cursor**, **Trae**, **Cline**, autonomous coding agents, and multi-step tool loops), context windows rapidly saturate with:
* Repetitive tool outputs and voluminous file dumps
* Stale historical turns and conversational echoes
* Semantic entropy that degrades reasoning precision and blows up API bills

The **Cognitive Context Firewall** sits transparently on the wire as a high-performance reverse proxy. It inspects raw request streams in real time, dynamic-prunes conversational bloat, and delivers high-density signal to downstream models.

### Real Production Benchmark (`claude-opus-4-8` Multi-Turn Session)

| Request State | Input Tokens | Completion Tokens | Turn Cost | Token Reduction |
| :--- | :---: | :---: | :---: | :---: |
| 🔴 **Raw Unfiltered Agent Loop** | 78,762 | 752 | $0.04126 | Baseline (0%) |
| 🔴 **Intermediate Turn** | 71,282 | 705 | $0.03740 | Baseline (0%) |
| 🟢 **Firewall Pruned Turn #1** | **6,782** | 12 | **$0.00342** | **-91.3%** |
| 🟢 **Firewall Pruned Turn #2** | **6,658** | 12 | **$0.00336** | **-91.5%** |

*Measured on live streaming requests with zero loss in code or reasoning precision.*

---

## 🌐 Live Prototype Endpoint

* **Base URL:** `https://crazy-project-m302.onrender.com`
* **Anthropic Route:** `https://crazy-project-m302.onrender.com/v1/messages`
* **OpenAI Route:** `https://crazy-project-m302.onrender.com/v1/chat/completions`

> [!NOTE]
> **Free Prototype Instance Notice:** The public demo node runs on a free-tier container. If the server has been idle, please allow **20–30 seconds** on the very first ping for the container cold start. Subsequent requests stream with real-time speed.

---

## 🚀 Quickstart & Setup Guides

### 1. Trae / Cursor / Cline IDE Integration

Connect your favorite AI IDE in under 60 seconds:

1. Open **Settings** $\rightarrow$ **Models** / **API Configuration** in Trae or Cursor.
2. Configure the following fields:
   * **Base URL:** `https://crazy-project-m302.onrender.com`
   * **Model:** `claude-3-5-sonnet-20241022` *(or `claude-3-7-sonnet-20250219`, `claude-opus-4-8`, `gpt-4o`)*
   * **API Key:** Your standard Anthropic or OpenAI API key.
3. Save settings. Your agent calls will now automatically pass through the context firewall before reaching the model provider.

---

### 2. cURL Terminal Test

Run a live streaming ping test directly from your command line:

```bash
curl https://crazy-project-m302.onrender.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-3-5-sonnet-20241022",
    "max_tokens": 512,
    "stream": true,
    "messages": [
      {
        "role": "user",
        "content": "Explain context pruning in autonomous AI agents in 2 sentences."
      }
    ]
  }'
```

---

### 3. Python (Anthropic SDK)

```python
import os
from anthropic import Anthropic

client = Anthropic(
    api_key=os.environ.get("ANTHROPIC_API_KEY"),
    base_url="https://crazy-project-m302.onrender.com"
)

response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "How do context firewalls improve agent unit economics?"}
    ]
)

print(response.content[0].text)
```

---

### 4. Node.js / TypeScript (`@anthropic-ai/sdk`)

```typescript
import Anthropic from '@anthropic-ai/sdk';

const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
  baseURL: 'https://crazy-project-m302.onrender.com',
});

async function main() {
  const stream = await anthropic.messages.create({
    model: 'claude-3-5-sonnet-20241022',
    max_tokens: 1024,
    stream: true,
    messages: [{ role: 'user', content: 'Ping test through context firewall' }],
  });

  for await (const chunk of stream) {
    if (chunk.type === 'content_block_delta' && chunk.delta.type === 'text_delta') {
      process.stdout.write(chunk.delta.text);
    }
  }
}

main();
```

---

## 🔀 Custom Upstream Enterprise Routing

If your team runs dedicated models, custom LLM gateways, or private VPC proxies, you can dynamically route cleansed traffic by passing the `X-Upstream-URL` header:

```http
X-Upstream-URL: https://your-enterprise-proxy.com/v1/messages
```

The firewall intercepts your multi-turn payload, dynamic-prunes token entropy, and passes the high-density payload directly to your designated upstream endpoint.

---

## 🔒 Security & Privacy

* **Stateless Stream Processing:** The proxy operates in-memory. Request bodies, prompts, files, and outputs are never stored, logged to disk, or retained.
* **Direct Secret Pass-Through:** Your API keys are transmitted over TLS and passed directly to upstream model providers to authenticate your calls.
* **Fail-Open Architecture:** If any parsing uncertainty occurs, the firewall defaults to safe pass-through to ensure zero disruption to live developer sessions.

---

## 🤝 For Investors, Angels & Design Partners

We are currently opening conversations with select angel investors, pre-seed venture funds, and enterprise design partners who recognize that **context infrastructure and unit economics** are the defining bottlenecks for autonomous AI in 2026.

* **Benchmark Whitepaper & Investor Deck:** DM on X [@sumitmehta](https://x.com) with the keyword **`FIREWALL`**.
* **Enterprise Pilots & Inquiries:** Reach out via GitHub issues or email to discuss dedicated high-throughput cluster deployments.

---

<div align="center">
  <sub>Built for the next generation of autonomous AI software.</sub>
</div>
