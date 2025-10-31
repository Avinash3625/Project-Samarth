Project Samarth — Intelligent Q&A Prototype (LM Studio)

🧠 Project Overview

Project Samarth is a lightweight, browser-based chat interface designed to interact with a local Large Language Model (LLM) hosted on LM Studio using OpenAI-compatible REST endpoints.
It demonstrates a Tabular RAG (Retrieval-Augmented Generation) simulation using two mock datasets — `df_crops` and `df_rainfall`.

The model is instructed to respond in a strict 3-part Markdown format:

1. Direct Natural Language Answer
2. Supporting Data (Markdown Table)
3. Sources (Mock dataset references)

This prototype emphasizes traceability, data provenance, and explainability in LLM responses while maintaining simplicity and portability.

🚀 What the Prototype Does

* Accepts natural-language questions related to Indian agriculture and climate.
* Sends the prompt and user query to LM Studio’s OpenAI-like `/v1/chat/completions` endpoint.
* Displays Markdown-formatted responses directly in the browser.
* Handles CORS, connection, and non-JSON errors gracefully.
* Fully configurable LM Studio URL and model ID within the HTML file.

🏗️ Architecture & Design

* Frontend-only prototype: Built using a single HTML + JS file for simplicity and demo readiness.
* Local LLM (LM Studio): Ensures data sovereignty and offline use.
* System Prompt: Encodes RAG simulation instructions and formatting constraints for consistent outputs.
* Mock Data Simulation: No real dataset integration — the model "simulates" querying two mock tables.
* Security Considerations:

  * Enable CORS in LM Studio
  * Optionally serve over HTTP (`python -m http.server`)
  * DOMPurify can be added for secure Markdown-to-HTML rendering.

📂 Repository Contents

| File                         | Description                                                   |
| ---------------------------- | ------------------------------------------------------------- |
| `project_samarth_local.html` | Main frontend file containing UI, configuration, and JS logic |
| `README.md`                  | Documentation file (this file)                                |

🧰 Prerequisites

* LM Studio (installed and running locally or on LAN)
* Model required - Gpt-OSS-20b
* Model loaded (e.g., `openai/gpt-oss-20b` or similar)
* Browser: Chrome / Firefox
* Python 3 (for quick local hosting) *or* Node.js (for `http-server`)

⚙️ Configuring LM Studio

Ensure LM Studio is set up as follows:

| Setting                    | Recommended Value                                |
| -------------------------- | ------------------------------------------------ |
|   Server Port              | `8880` or `5500` (use what your LM Studio shows) |
|   Serve on Local Network   | ✅ ON (if frontend runs on another device)       |
|   Enable CORS              | ✅ ON                                            |
|   Reachable URL            | e.g. `http://192.168.29.200:8880`                |
|   Model ID                 | e.g. `openai/gpt-oss-20b`                        |

> The frontend must target the exact same endpoint:
> `http://<host>:<port>/v1/chat/completions`

🧾 How to Run the Frontend

1️⃣ Save the HTML

Save the provided file as:

```
project_samarth_local.html
```

2️⃣ Configure LM Studio URL & Model

Open the file and locate:

```js
const lmApiUrl = "http://192.168.29.200:8880/v1/chat/completions";
const LM_MODEL_NAME = "openai/gpt-oss-20b";
```

Edit these values to match your **LM Studio host**, **port**, and **model ID**.

3️⃣ Serve the Page Locally

From the folder containing your file:

Option 1 — Python

```bash
python -m http.server 8000
# Visit: http://localhost:8000/project_samarth_local.html
```

Option 2 — Node.js

```bash
npx http-server -p 8000
```

4️⃣ Open in Browser

Navigate to:

```
http://localhost:8000/project_samarth_local.html
```

5️⃣ Test the Chat

Try queries such as:

* `Hello`
* `What was the total rice production in Maharashtra in 2020?`

LM Studio’s **Developer Logs** should show a POST request to `/v1/chat/completions`.

🧪 Quick Connectivity Test (Optional)

Before using the UI, verify LM Studio connectivity:

```bash
curl -s -X POST "http://192.168.29.200:8880/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "openai/gpt-oss-20b",
    "messages": [{"role": "user", "content": "hello"}]
  }' | jq
```

Expected: a valid JSON response.
If not, check LM Studio’s logs or CORS settings.

🔄 Response Handling

The frontend gracefully handles multiple LM Studio response formats:

* `choices[0].message.content` — OpenAI format
* `choices[0].text` — older local models
* `output[0].content[0].text` — alternative runtimes
* Fallback: stringifies unknown response types

(Streaming responses are not supported in this prototype.)

💻 System Requirements

| Component     | Minimum                                    | Recommended        |
| ------------- | ------------------------------------------ | ------------------ |
|   Processor   | Intel i5 (10th Gen) / Ryzen 5              | Intel i7 / Ryzen 7 |
|   RAM         | 12 GB                                      | 16 GB+             |
|   Storage     | 256 GB SSD                                 | 512 GB SSD         |
|   OS          | Windows 10/11 (64-bit) or Ubuntu 22.04 LTS | —                  |
|   Internet    | Required for API / model download          | —                  |

🧩 Optional Tools

* MySQL Workbench — if database integration added later
* VS Code — recommended editor
* Postman — for API testing
